# weewx-clone

This is a 'clone' of [WeeWX](https://github.com/weewx/weewx) to prototype testing WeeWX in a Github action.
**Do not use it for anything else!**

## WeeWX code updates

The following changes were made to the WeeWX code base.

### bin/weedb/tests/setup_mysql.sh

Obviously this cannot be run interactively.
For the prototype I took a 'quick and dirty' approach.
This leverages the fact that running `mysql` as root does not require a password.
Fully configuring mysql/mariadb would get by the removal of `--protocol=tcp`.
Passing in the password as an optional parameter would be an option to eliminate the need to run interactive.

### bin/weeutil/tests/test_config.py -> test_weeutil_config.py

Renamed the file `bin/weeutil/tests/test_config.py` to `test_weeutil_config.py`.
With tests as part of the source tree and discovering tests, pytest requires unique file names or `__init__.py` file in the directory.
For additional information see [test discovery](https://docs.pytest.org/en/7.2.x/explanation/goodpractices.html#tests-as-part-of-application-code)

### bin/weewdb/tests/test_weedb.py

When running under pytest received the following error
```AttributeError: 'Common' object has no attribute 'db_dict'```

## python -m unittest discover -v bin

I also ran the tests in `discover`mode  with unittest.
First, this requires a `__init__.py` in each test directory.
Once these were added I got the following error, `ModuleNotFoundError: No module named 'gen_fake_data'`.
I worked around this by using this invocation, `PYTHONPATH=bin/weewx/tests python -m unittest discover -v bin`.
Next challenge is that some tests run code that use logging.
So a logger had to be installed in the `act` docker image.
The only one I got to work was `syslog-ng`.
Note, another option would be to mock the logger.

## Running coverage

```text
PYTHONPATH=bin/weewx/tests coverage run --branch -m unittest discover -v bin
coverage xml -i
coverage report -i
```

## Miscellaneous notes

### Use of mariadb

I couldn't get mysql to work when running locally using [act](https://github.com/nektos/act) (Plus I prefer mariadb).

### Running Github actions locally

- I have used [act](https://github.com/nektos/act) to run the tests locally.
I run it locally for two reasons. The first is to test the actual workflow.
The second is to test WeeWX.
Installing the necessary prerequisites can be time consuming.
So when testing WeeWX the ```-r, --reuse = don't remove container(s) on successfully completed workflow(s) to maintain state between runs``` option can be useful.
Another option to speed it up would be to create a custom docker image.
This would increase the amount of WeeWX development infrastructure and knowledge to maintain...

- I had to disable mysql/mariadb in the host - due to port conflicts.

- I installed [act](https://github.com/nektos/act) using these [directions](https://github.com/nektos/act#bash-script).

- Invocation:
``` sudo /home/richbell/bin/act push -P ubuntu-latest=catthehacker/ubuntu:runner-latest ```
