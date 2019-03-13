+++
title = "How to Install Turtl on Linux"
slug = "portfolio"
date = "2019-01-01"
+++

# What is Turtl?


Turtl is an open-source alternative to cloud-based storage services. With a focus on privacy, Turtl offers a place to store and access your passwords, bookmarks and pictures. Hosting your own Turtl server on a secure Linode allows you to monitor your own security.

The Turtl server is written in Common Lisp, and the low-level encryption is derived from the Stanford JavaScript Crypto Library. If encryption is important to you, read over the encryption specifics section of the [official documentation](https://turtlapp.com/docs/security/encryption-specifics/)



## Install Dependencies:

The Turtl server has to be built from source. Download all of the dependencies as well as git:

    apt install wget curl libtool subversion make automake git
### Libuv, RethinkDB, Clozure Common Lisp, QuickLisp:


#### Libuv

Download the Libuv package from the official repository:

    wget https://dist.libuv.org/dist/v1.13.0/libuv-v1.13.0.tar.gz
    tar -xvf libuv-v1.13.0.tar.gz

Build the package from source:

    cd libuv-v1.13.0
    sudo sh autogen.sh
    sudo ./configure
    sudo make
    sudo make install

After the package is built, run `sudo ldconfig` to maintain the shared libracy cache.

#### RethinkDB

[RethinkDB](https://rethinkdb.com/faq/) is a flexible JSON database. According to the Turtl [documentation](https://turtlapp.com/docs/server/), RethinkDB just needs to be installed; Turtl will take care of the rest.

RehinkDB has community-maintained packages on most distributions. On Ubuntu, you have to add the RethinkDB to your list of repositories:

    source /etc/lsb-release && echo "deb http://download.rethinkdb.com/apt $xenial main" | sudo tee /etc/apt/sources.list.d/rethinkdb.list
    wget -qO- https://download.rethinkdb.com/apt/pubkey.gpg | sudo apt-key add -

Navigate to your `sources.list` folder and add your version of Ubuntu:

    vi /etc/apt/sources.list.d/rethinkdb.list
    deb http://download.rethinkdb.com/apt xenial main

Update apt and install RethinkDB:

     sudo apt update
     sudo apt install rethinkdb

Navigate to `/etc/rethinkdb/` and rename `default.conf.sample` to `default.conf`

    sudo mv /etc/rethinkdb/default.conf.sample /etc/rethinkdb/default.conf

Restart the `rethinkdb.service` daemon:

    sudo systemctl restart rethinkdb.service

### Install Turtl

Clone Turtl from the Github repository:

    git clone https://github.com/turtl/api.git


Create a file called `launch.lisp` inside `/api` and copy the commands below:

    touch launch.lisp
    vi launch.lisp

    (pushnew "./" asdf:*central-registry* :test #'equal)
    (load "start")

Turtl does not ship with all of its dependencies. Instead, the Turtl community provides a list of dependencies. Clone these into `/home/turtl/quicklisp/local-projects`:

    echo "https://github.com/orthecreedence/cl-hash-util https://github.com/orthecreedence/cl-async https://github.com/orthecreedence/cffi https://github.com/orthecreedence/wookie https://github.com/orthecreedence/cl-rethinkdb https://github.com/orthecreedence/cl-libuv https://github.com/orthecreedence/drakma-async https://github.com/Inaimathi/cl-cwd.git" > dependencies.txt

    for repo in `cat dependencies.txt`; do `git clone $repo`; done

Edit the `/home/turtl/.ccl-init.lisp` to include:

    (cwd "/home/turtl/api")
    (load "/home/turtl/api/launch")

The first line tells Lisp to use the `cl-cwd` package that you cloned to change the current working directory to `/home/turtl/api`. You can change this to anything, but your naming conventions should be consistent. The second line loads your `launch.lisp`, loading `asdf` so that Turtl can run.

Create the default Turtl configuration file:

    cp /home/turtl/api/config/config.default.lisp /home/turtl/api/config/config.lisp

The `config.lisp` file is where the configurations for your server are stored. If you want to connect to your Linode from a Turtl desktop or mobile client, you need to add the Linode's public IP address to:


    (defvar *site-url* "http://1.0.0.0:8181"
       "The main URL the site will load from.")


Go to your home directory and run `ccl64`. This will automatically start the Turtl server.


You now have a functioning Turtl server. Add files, store passwords, and save bookmarks in your own private Turtl instance.



---
This tutorial was originally published here:
https://www.linode.com/docs/applications/cloud-storage/how-to-install-a-turtl-server-on-ubuntu/
