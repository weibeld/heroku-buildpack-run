Heroku Buildpack: Run
=====================

Run custom commands during the build process.


Description
-----------

This buildpack allows to execute arbitrary commands on the build dyno during the build process by sourcing one or more files with Bash commands.


Usage
-----

### Basic

Add the buildpack to your app:
~~~bash
heroku buildpacks:add https://github.com/weibeld/heroku-buildpack-run.git
~~~

Create the file `buildpack-run.sh` in the root directory of your app, for example:
~~~bash
echo "Hello World"
~~~

Now push your app to Heroku as usual. The commands in `buildpack-run.sh` will be executed during the build.


###Use of a different filename

If you want to use another filename than `buildpack-run.sh`, then you can specify this filename in the app config variable `BUILDPACK_RUN`. For example, if your file is called `my_file.sh`:

~~~bash
heroku config:set BUILDPACK_RUN=my_file.sh
~~~


### Use of multiple files

You can specify multiple files to `BUILDPACK_RUN` by separating them with colons. For example, if you want to source the files `my_file_1.sh` and `my_file_2.sh` in this order, then set your `BUILDPACK_RUN` as follows:

~~~bash
heroku config:set BUILDPACK_RUN="my_file_1.sh:my_file_2.sh"
~~~


Additional Information
----------------------

- The working directory of the shell in which your commands will be executed is the root directory of your app
- The paths of the files specified to `BUILDPACK_RUN` must be relative to the root directory of your app
- You can use the command `exit 1` in your files to abort the build
- The following build-specific shell variables are available to your command files:
    - `BUILD_DIR`: path to the root directory of your app on the build dyno
    - `ENV_DIR`: the directory holding all your app's config variables as files
    - `CACHE_DIR`: directory whose content persists across builds of your app


License
-------

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.

