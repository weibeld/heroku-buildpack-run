# Heroku Buildpack: Run

Run custom commands during the build process.


## Description

This buildpack allows to execute arbitrary commands on the build dyno during the build process by running one or more user-supplied Bash scripts.


## Basic Usage

Add the buildpack to your app:

~~~bash
heroku buildpacks:add https://github.com/weibeld/heroku-buildpack-run
~~~

Create the file `buildpack-run.sh` in the root directory of your app. For example:

~~~bash
# This is buildpack-run.sh
echo "Hello World"
~~~

Now push your app to Heroku as usual. The `buildpack-run.sh` script will be run by Bash during the build.

Note that it is **not** required to add a `#!/bin/bash` to your script (but if it's there, it doesn't do any harm).

## Usage with Another Filename

If you want to use a script with a different name than `buildpack-run.sh`, then you can specify this file in the Heroku app config variable `BUILDPACK_RUN`.

For example, if your script is called `script.sh`:

~~~bash
heroku config:set BUILDPACK_RUN=script.sh
~~~

The specified value must be a filename relative to the root directory of your app, so it could also be something like `bin/script.sh` or `./script.sh`.

Note that the `BUILDPACK_RUN` config variable takes precedence over the default `buildpack-run.sh` file. So, if you have both, a `BUILDPACK_RUN` variable and a `buildpack-run.sh` file, then only the file in `BUILDPACK_RUN` will be executed.

## Usage with Multiple Files

You can specify multiple files in `BUILDPACK_RUN` by separating them with colons:

~~~bash
heroku config:set BUILDPACK_RUN=script1.sh:script2.sh:script3.sh
~~~

The scripts will be executed sequentially in the specified order.

## Usage with Arguments

You can specify arguments to your scripts, just as you would on the command line.

For, example, you can do something like this:

~~~bash
heroku config:set BUILDPACK_RUN="script1.sh 1 2 3:script2.sh hey 'hello world'"
~~~

And this will execute `bash script1.sh 1 2 3` and then `bash script2.sh hey 'hello world'`.

## Build Environment Variables

The following variables holding information about the Heroku build environment are accessible to your scripts:

- `BUILD_DIR`: absolute path of your app's root directory on the build dyno
- `CACHE_DIR`: directory on the build dyno that persists between builds
- `ENV_DIR`: directory containing files for the app's Heroku config variables

The working directory for your scripts is `BUILD_DIR`, which is the root directory of your app.

## License

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.
