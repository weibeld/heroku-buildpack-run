Heroku Buildpack: Run
=====================

Run custom commands during the build process.


Description
-----------

This buildpack allows to execute arbitrary commands on the build dyno during the build process by sourcing a file containig Bash commands.

Just create the file `buildpack-run.sh` in the root directory of your application and write in this file the commands that you want to execute. This file is then *sourced* by the `compile` script of this buildpack. That is, the commands in `buildpack-run.sh` are executed on the build dyno as they would be part of the `compile` script.

Available build-specific variables are `BUILD_DIR`, `CACHE_DIR`, and `ENV_DIR`.

The initial working directory is the root directory of your application.

For aborting the build at any point, you can use the `exit` command with a non-zero exit code, e.g. `exit 1`.

Usage
-----

Add the buildpack to your app:
~~~bash
heroku buildpacks:add https://github.com/weibeld/heroku-buildpack-run.git
~~~

Create the file `buildpack-run.sh` in the root directory of your app:
~~~bash
echo "Hello World"
~~~

Now push your app to Heroku as usual, and the commands in `buildpack-run.sh` will be executed during the build.

**Note:** the working directory of the shell in which your commands will be executed is the root directory of your app.


###Use of a different file name

If you want to use another file name than `buildpack-run.sh`, then simply create this file and specify its name in the config variable `BUILDPACK_RUN`:
~~~bash
heroku config:set BUILDPACK_RUN=<filename>
~~~




Usage
-----

Simply do:

~~~bash
# Create file 'buildpack-run.sh' containing bash commands
echo 'echo "hello world"' >buildpack-run.sh

heroku buildpacks:set https://github.com/weibeld/heroku-buildpack-run.git
~~~

The buildpack is now set and will be used on the next `git push heroku master`.

For more information on how to use custom buildpacks, see <https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack>.


Usage together with other buildpacks
------------------------------------

You can use multiple buildpacks at the same time as described on [https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app](
https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app):

~~~bash
heroku buildpacks:add --index 1 heroku/ruby
heroku buildpacks:add --index 2 https://github.com/weibeld/heroku-buildpack-run.git
~~~

You can always check which buildpacks you have currently added to your app with:

~~~bash
heroku buildpacks
~~~


License
-------

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.

