Heroku Buildpack: Run
=====================

Run custom commands during the build process.


Description
-----------

This buildpack allows to execute arbitrary commands on the build dyno during the build process.

To use it, you have to create the file `buildpack-run.sh` in the root directory of your app. This file can contain arbitrary Bash commands. If you then activate the buildpack (see below), `buildpack-run.sh` will be sourced during the build. The working directory will be the root directory of your application.

This buildpack is useful for finding out information about the build process.


Usage
-----

Simply do

~~~bash
# Create file 'buildpack-run.sh' containing bash commands
echo 'echo "hello world"' >buildpack-run.sh

heroku buildpack:set https://github.com/weibeld/heroku-buildpack-run.git
~~~

On the next `git push heroku master`, the `heroku-buildpack-run` buildpack will be used.

For more information on how to use custom buildpacks, see <https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack>.


Usage together with other buildpacks
------------------------------------

To use multiple buildpacks, you can use [heroku-buildpack-multi](
https://github.com/ddollar/heroku-buildpack-multi):

~~~bash
# Create file 'buildpack-run.sh' containing bash commands
echo 'echo "hello world"' >buildpack-run.sh

# Create file .buildpacks listing the buildpacks you want to use
cat <<EOF >.buildpacks
https://github.com/heroku/heroku-buildpack-ruby.git
https://github.com/weibeld/heroku-buildpack-run.git
EOF

heroku buildpacks:set https://github.com/ddollar/heroku-buildpack-multi.git
~~~

On the next `git push heroku master`, all the buildpacks listed in `.buildpacks` will be used.


License
-------

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.

