# Heroku Buildpack: Run

Run custom commands during the build process.

## Description

This buildpack allows specifying custom scripts or other commands that will be run during the build process. If any command exits with a non-zero status code, the build is aborted.

## Usage

### Using the `buildpack-run.sh` script

By default, the buildpack looks for a script named `buildpack-run.sh` in the root directory of your app. So, you can do the following:

```bash
echo -e '#!/bin/bash\necho "Hello World"' >buildpack-run.sh
chmod +x buildpack-run.sh
git add buildpack-run.sh && git commit
git push heroku master
```

The above example will print `Hello World` during the build.

### Using the `BUILDPACK_RUN` config variable

Alternatively to using `buildpack-run.sh`, you can specify custom commands in the `BUILDPACK_RUN` config variable:

```bash
heroku config:set BUILDPACK_RUN='echo "Hello World"
```

The above example will print `Hello World` during the build.

You can also specify multiple commands by separating them with colons:

```bash
heroku config:set BUILDPACK_RUN='echo "Hello World":uname -a:./myscript.sh foo bar
```

In the above example, the buildpack will execute the following commands in sequence:

1. `echo "Hello World"`
1. `uname -a`
1. `./myscript.sh foo bar`

## Loading config variables

By default, the app's [config variables](https://devcenter.heroku.com/articles/config-vars) are only available as files in a specific directory in the build environment (see [`ENV_DIR`](#default-environment-variables) below). You can load all these config variables as environment variables by setting the `BUILDPACK_RUN_LOAD_CONFIG` config variable:

```bash
heroku config:set BUILDPACK_RUN_LOAD_CONFIG=1
```

Now, all the app's config variables will be available to your commands as environment variables.

If you want to prevent certain config variables from being loaded as environment variables (for example, to prevent overwriting native environment variables), you can specify them in the `BUILDPACK_RUN_LOAD_CONFIG_SKIP` config variable (separated by colons):

```bash
heroku config:set BUILDPACK_RUN_LOAD_CONFIG_SKIP=FOO:BAR:BAZ
```

In the above example, the config variables named `FOO`, `BAR`, and `BAZ` will _not_ be loaded as environment variables. Note that this may be especially useful for config variables like `PATH` that you might _not_ want to overwrite in the build environment.

## Default environment variables

The following special environment variables are always available to your commands:

- `BUILD_DIR`: your app's root directory in the build environment 
- `CACHE_DIR`: directory that persists between builds and can be used as a cache
- `ENV_DIR`: directory containing the app's config variables as files

## License

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.
