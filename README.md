Heroku Buildpack: Run
=====================

Run custom commands during the build process.


Description
-----------

This buildpack allows to execute arbitrary commands on the build dyno during the build phase. To use it, you have to create the file `run.sh` containing shell commands (`bash`) in the root directory of your app. This file will then be sourced during the execution of the `compile` script of the buildpack. The working directory of the commands in `run.sh` will be the root directory of your app.

The buildpack is useful when one wants to find out certain things about the build process, for example, in order to write a custom buildpack.


Usage
-----

Simply do

~~~bash
# Create file 'run.sh' containing bash commands
echo 'echo "hello world"' >run.sh

heroku buildpack:set https://github.com/weibeld/heroku-buildpack-run.git
~~~

On the next `git push heroku master`, the Run buildpack will be used.

For more information on how to use custom buildpacks, see <https://devcenter.heroku.com/articles/third-party-buildpacks#using-a-custom-buildpack>.


Usage together with other buildpacks
------------------------------------

To use multiple buildpacks, you can use [heroku-buildpack-multi](
https://github.com/ddollar/heroku-buildpack-multi):

~~~bash
# Create file 'run.sh' containing bash commands
echo 'echo "hello world"' >run.sh

# Create file .buildpacks listing the buildpacks you want to use
cat <<EOF >.buildpacks
https://github.com/heroku/heroku-buildpack-ruby.git
https://github.com/weibeld/heroku-buildpack-run.git
EOF

heroku buildpack:set https://github.com/ddollar/heroku-buildpack-multi.git
~~~

On the next `git push heroku master`, all the buildpacks listed in `.buildpacks` will be used.


License
-------

Licensed under the MIT License. See [LICENSE.md](LICENSE.md) file.

