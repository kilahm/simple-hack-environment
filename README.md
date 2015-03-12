# Simple development environment for [Hack](www.hacklang.org)

For those who were unable to follow along during my talk at the [SeaPHP](http://www.meetup.com/seaphp/events/219969983/) meetup, I have assembled the Vagrant file that results from following the steps I discussed.

Please note that my talk and these instructions assume you are comfortable using the command line, have installed and know the basics of [Vagrant](https://www.vagrantup.com/), and prefer to use [vim](http://www.vim.org/) over [emacs](http://www.gnu.org/software/emacs/).

I have added all of the steps I took during the presentation to the inline shell provision script in the Vagrant file.  These steps include:

* Setting the ip address of the virtual machine to 192.168.100.100
* Increasing the memory available to the virtual machine to 2G
* Installing apache2, git and hhvm
* Creating a symbolic link from /var/www/hhvm to /vagrant/hhvm
* Modifying /etc/apache2/mods-enabled/hhvm_proxy_fcgi.conf to look for hack scripts in /var/www/hhvm instead of /var/www
* Creating a .hhconfig file in /var/www/hhvm so the type checker will work
* Creating a sample index.php file in /var/www/hhvm
* Increasing the http slow query threshold to 1 minute
* Installing composer globally

After you run `vagrant up` you should be able to point your browser to http://192.168.100.100/index.php to view the sample file.

I suggest you `vagrant ssh` into your box to actually write Hack code using vim, although there are [plugins](https://github.com/facebook/hhvm/wiki/Hack%20Editor%20Plugins) available for GUI editors as well.

I found installing [spf13-vim](http://vim.spf13.com/) increased the capabilities of vim to almost match those of many GUI text editors.

## Troubleshooting 500 level errors

Often when apache or hhvm are incorrectly configured the hhvm process will return a 500 error.  By default, there is practically no other information provided.  In this case, your first step is to check the log.  The default log location is `/var/log/hhvm/error.log`.

Another source of 500 level errors is a type checker failure.  The log file should include an indication for why the type checker failed.

# Caveats

This set up is a minimal development environment.  There are many settings to be tweaked for both [apache](http://httpd.apache.org/docs/2.4/configuring.html) and [hhvm](https://github.com/facebook/hhvm/wiki/INI-Settings).  There are also many caching and data persistence tools which are allmost always used for any project of event moderate size.  Care and research must be performed to ensure all of the parts work together.

I personally seperate my hhvm and apache processes in their own [Docker](https://www.docker.com/) containers, which allows me to tightly control the environment for apache and hhvm. Docker also allows me to ensure consistentcy between development, test, and production environments.
