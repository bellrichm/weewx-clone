# weewx-clone

This is a 'clone' of [WeeWX](https://github.com/weewx/weewx) to prototype testing WeeWX in a Github action.
**Do not use it for anything else!**

## WeeWX code updates

The following changes were madebto the WeeWX code base.

### bin/weedb/tests/setup_mysql.sh

Obviously this cannot be run interactively.
For the prototype I took a 'quick and dirty' approach.
This leverages the fact that running `mysql` as root does not require a password.
Fully configuring mysql/mariadb would get by the removal of `--protocol=tcp`.
Passing in the password as an optional parameter would be an option to eliminate the need to run interactive.

### bin/weeutil/tests/test_config.py -> test_weeutil_config.py

Renamed the file `bin/weeutil/tests/test_config.py` to `test_weeutil_config.py`.
With tests as part of the source tree and discovering tests, pytest requires unique file names or \__init__.py file in the directory.
For additional information see [test discovery](https://docs.pytest.org/en/7.2.x/explanation/goodpractices.html#tests-as-part-of-application-code)

### bin/weewdb/tests/test_weedb.py

When running under pytest received the following error
```AttributeError: 'Common' object has no attribute 'db_dict'```

## Miscellaneous notes

### Use of mariadb

### Running Github actions locally

