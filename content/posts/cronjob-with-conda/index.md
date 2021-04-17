---
title: "Cronjob With Conda"
date: 2021-04-17T09:47:32+09:00
draft: false
toc: true
images:
enableEmoji: true
tags:
  - python
  - anaconda
  - conda environment
  - unix
  - cron
  - automation
---

## How to setup a python cronjob with conda

UNIX based machine users can easily setup automated tasks using the default `cronjob` module. However, if your using conda environments, it might not just work auto-magically out of the box. After fiddling around with system path variables this is how I made my automated task work.

TL;DR: Don't try to use `conda activate <env name>`. Set `PATH` to `/opt/anaconda3/envs/<env name>/bin` in your `crontab`. If any local package import error occur, then additionaly set `PYTHONPATH` to `/path/to/your/code/repository` in `crontab`.

## Example Usage

Let's say that I want a python script(`main.py`) in a certain repository(`/home/dongookson/test_scripts/`) to run every minute using a conda environment called `prod`.

### Directory Maps

The repository that I use in this example will look something like this.

```shell
/home/dongookson
├──cron.sh
└──test_scripts/
    ├── main.py
    ├── src/
    │   └── resource.py
    └── utils/
        └── tool.py
```

The `main.py` will just be a simple python script.

```python
from src.resource import *
from utils.tool import *
from datetime import datetime

def foo():
    print(f"foo------------------------------------------")
    print(f"Foo was called! {datetime.now()}")
    print(f"------------------------------------------foo")
    return


if __name__=="__main__":
    foo()
```

### Figuring out the conda environment

First, remember that you have two versions of `crontab`. One will be `sudo crontab` which has super user privilege and the other is `crontab` that is associated with your account.

Then, check your `conda env` path. This will be essential when you want your script to run under a specific conda environment. If the environment is not specified and your python script uses certain packages associated only with that environment, import errors will occur.

```shell
(base) dongookson➜~» conda env list
# conda environments:
#
base                  *  /Users/dongookson/opt/anaconda3
prod                    /Users/dongookson/opt/anaconda3/envs/prod
```

We can observe that the python resides in the `/Users/dongookson/opt/anaconda3/envs/prod`. The actual python interpreter binary will be inside `/Users/dongookson/opt/anaconda3/envs/prod/bin`.

We will explicity set this as the `PATH` variable in our `sudo crontab`. This can be done in the following way. I use neovim as the default editor so the following command will open up the `crontab` in neovim.

```shell
(base) dongookson➜~» sudo crontab -e
```

Then all you have to do is write the following content on top of your job.

**_crontab_**

```shell
PATH=/Users/dongookson/opt/anaconda3/envs/prod/bin
```

### Bash scripting

Now, you can make a `.sh` script which consists of various terminal commands in order to execute the `/home/dongookson/test_scripts/main.py` python script.

When scripting the `cron.sh` file, just recall the commands that you would normally give the terminal. In this case, it can be,

**_cron.sh_**

```shell
#!/bin/sh
python3 /home/dongookson/test_scripts/main.py
```

The first part is the shebang. This will run the `.sh` file with the shell in the shebang directory.

But, you can see that something is missing.

Yes, we did not activate conda environment. In my case, this part is not necessary as I've already set the `PATH` in the crontab to the `prod` environment in the above.

### Editing `crontab`

Given that all the steps above are covered, we can add the cronjob to the `crontab`.

```shell
(base) dongookson➜~» sudo crontab -e
```

**_crontab_**

```shell
PATH=/Users/dongookson/opt/anaconda3/envs/prod/bin
* * * * * /home/dongookson/cron.sh >> /home/dongookson/clogs.log 2>&1
```

The second line is the cronjob which is

1. Run the job every minute.
2. The job is to run the `cron.sh` as a shell script(note that just referencing the shell script will run the commands inside the script due to the shebang)
3. Append(>>) terminal outputs to `/home/dongookson/clogs.log`
4. If an error occurs treat this as a terminal output(`2>&1`)

### Result

I should now get logs in `clogs.log`. We can see that the `foo` function was called every minute.

```shell
(base) dongookson➜~» cat clogs.log
foo------------------------------------------
Foo was called! 2021-04-16 13:47:01.454232
------------------------------------------foo
foo------------------------------------------
Foo was called! 2021-04-16 13:48:01.489774
------------------------------------------foo
foo------------------------------------------
Foo was called! 2021-04-16 13:49:01.529458
------------------------------------------foo
```

### Troubleshooting

Sometimes import problems might occur when you are importing local packages or when you want to run a python script which is not located at the top level in your code repository. From the example above, let's say you want to run `resource.py` under `test_scripts/src/`.

Then I recommend modifying your `crontab` by adding a `PYTHONPATH` variable. This variable is an additional directory where your python interpreter will look for modules or packages. So, if you are using custom python libraries outside of the global python package directory(`site-packages`), this is a mandatory step for running a cronjob.

**_crontab_**

```shell
PATH=/Users/dongookson/opt/anaconda3/envs/prod/bin
PYTHONPATH=/home/dongookson/test_scripts
* * * * * /home/dongookson/cron.sh >> /home/dongookson/clogs.log 2>&1
```
