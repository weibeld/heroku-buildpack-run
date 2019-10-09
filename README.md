# Heroku Buildpack: Run

Run custom commands during the build process.

## Description

This buildpack allows to specify arbitrary commands that will be run in sequence during the build process.

If any command exits with a non-zero code, the build is aborted.

## Enable

Add the buildpack in addition to any other buildpacks:

~~~bash
heroku buildpacks:add https://github.com/weibeld/heroku-buildpack-run
~~~

Set the buildpack as the only buildpack:

~~~bash
heroku buildpacks:set https://github.com/weibeld/heroku-buildpack-run
~~~

## Configure

The commands to run are specified in the `BUILDPACK_RUN` [config variable](https://devcenter.heroku.com/articles/config-vars) by using colons as delimiters.

A command can be anything from a user-provided shell script or binary to a native UNIX command. Commands may have arguments as well.

### Example

~~~bash
heroku config:set BUILDPACK_RUN='./script.sh:ls -l:bin/myexec foo bar'
git push heroku master
~~~

This causes the buildpack to run the following commands in sequence:

1. `./script.sh`
2. `ls -l`
3. `bin/myexec foo bar`

Notes:

- `script.sh` is a user-provided shell script
- `ls -l` refers to the native `ls` command on the build dyno
- `bin/myexec` is a user-provided binary 

### Default value

If the `BUILDPACK_RUN` config variable is unset, then `./buildpack-run.sh` is used as the default command.

So, if you want to run only a shell script, you can name it `buildpack-run.sh` and omit setting the `BUILDPACK_RUN` config variable.

## Build Environment Variables

The following special environment variables are available to your commands during the build:

- `BUILD_DIR`: absolute path of your app's root directory on the build dyno
- `CACHE_DIR`: directory on the build dyno that persists between builds
- `ENV_DIR`: directory containing the values of all config variables in files

The working directory for your scripts is `BUILD_DIR`, which is the root directory of your app.

## License

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.
