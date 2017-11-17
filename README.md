# kirby-vagrant
VagrantFile and provisioning for a local development environment

## What this is.

These files, in combination with [Virtualbox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads.html) will allow you
to run a local development environment that updates in realtime and provides
a good framework for a basic Kirby server running on Ubuntu.

## What this is not.

This is not a complete setup for running Kirby in production, although the
configuration could be easily extended for that.

At a minimum you'd want to:

* Enable SSL in nginx and redirect all web requests to the secure site.
* Run ufw to block all other ports.

## How to use

Just copy these files into the root of your vagrant project.

If you don't commit your /site/accounts directory and you have multiple
developers and would like to provide them all with login accounts, create them
in the panel or with [kirby cli](https://github.com/getkirby/cli) and copy them to /util/accounts.  They'll
get copied into place when people setup their systems with ```vagrant up```.

### Step by step:

1. Install Virtualbox
2. Install Vagrant
3. run ```vagrant up```

After some churning and blinking lights you should see "you can now load the test site"

At that point, the site should be available at http://192.168.73.13.

If you don't like that IP, change it in the Vagrantfile.  If you would rather use
a name, just make a DNS entry for local.$mycompany that points to 192.168.73.13 or
add an entry to your /etc/hosts.

You can use http://192.168.73.13/util/info.php to see the details about your system.

The PHP Debugger is already configured, it should 'just work' with most IDE's.

If you have an idea for an improvement, Pull Requests are gladly accepted.
