---
layout:     post
title:      Setting up Node.js >= 4.x on Debian 7
category:
    - Debian
    - Node.js
    - Embedded
    - IoT
    - Beaglebone
date:       2015-11-15 23:00:00

---
I just received my Beaglebone Black, installed with Debian 7.9 "Wheezy".
This is a quick guide on how to set up most recent Node.js versions and gcc/g++ 4.8 for native bindings.

---

Unfortunately, Debian Wheezy provides only outdated versions of Node.js (0.12) and gcc/g++ (4.6).
Installing a recent Node.js version can be done either by building from source, or by retreiving the binaries.

Building from source take some times, as it requires to install dependencies (such as gcc). The benefits are that your package manager is aware of your installation, and the dependencies required for native bindings are already set up.  

If you're not concerned about directly installing binaries (without your package manager), and not interested in native bindings, go for the [direct install](#direct-install).
Otherwise, [build Node.js from source](#building-nodejs-from-source).


# Direct install
Nothing fancy here, just download the binaries and put them in the right folders.

```bash
$ wget https://Node.js.org/dist/v[VERSION]/node-[VERSION]-linux-armv7l.tar.gz
$ tar -xzvf node-[VERSION]-linux-armv7l.tar.gz
$ cd node-[VERSION]-linux-armv7l
$ cp ./bin/node /usr/local/bin/node
$ cp -r ./lib/node_modules /usr/local/lib/node_modules
$ ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm
$ cp -r ./include/node /usr/local/include
$ mkdir -p /usr/local/man/man1 && cp ./share/man/man1/node.1 /usr/local/man/man1/node.1
```

# Building Node.js from source

## Dependencies: gcc/g++ 4.8
Both gcc and g++ 4.8 are present in the Debian 8 "Jessie" respositories. Let's change Wheezy respositories into Jessie's for the installation.

```bash
$ sed -i 's/wheezy/jessie/g' /etc/apt/sources.list
$ apt-get update
```

We can now install gcc and g++ from Jessie repositories.

```bash
$ apt-get install gcc-4.8 g++-4.8
```

Change the sources.list back

```bash
$ sed -i 's/jessie/wheezy/g' /etc/apt/sources.list
$ apt-get update
```

In case of you need to switch between 4.7 and 4.8 versions, this can be done with the following commands.

```bash
# Add the alternatives
$ update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 20
$ update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
$ update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
$ update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50

# Switch between 4.6 and 4.8
$ update-alternatives --config gcc
$ update-alternatives --config g++
```

You can verify the current version of gcc/g++

```bash
gcc -v
```

## Build and install
Once the dependencies are set up, we can install node using [Node.js source](https://github.com/nodesource/distributions)
pretending we are running a Debian Jessie.

For Node.js v4.x:

```bash
curl -sL https://deb.nodesource.com/setup_4.x | bash -
apt-get install -y Node.js
```

For Node.js v5.x:

```bash
curl -sL https://deb.nodesource.com/setup_5.x | bash -
apt-get install -y Node.js
```
