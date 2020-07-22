# [Installation](https://docs.mongodb.com/manual/installation/)

## [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/)

### [Install on Linux](https://docs.mongodb.com/manual/administration/install-on-linux/)

#### [Install on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

**Import the public key used by the package management system.**

```
sudo apt-get install gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
```

**Create a list file for MongoDB**

Create the `/etc/apt/sources.list.d/mongodb-org-4.2.list` file for Ubuntu 18.04 (Bionic):

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

Create the `/etc/apt/sources.list.d/mongodb-org-4.2.list` file for Ubuntu 16.04 (Xenial):

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

**Reload local package database.**

```
sudo apt-get update
```

**Install the MongoDB packages**

To install the latest stable version, issue the following

```
sudo apt-get install -y mongodb-org
```

To install a specific release, you must specify each component package individually along with the version number, as in the following example:

```
sudo apt-get install -y mongodb-org=4.2.8 mongodb-org-server=4.2.8 mongodb-org-shell=4.2.8 mongodb-org-mongos=4.2.8 mongodb-org-tools=4.2.8
```

To prevent unintended upgrades, you can pin the package at the currently installed version:

```
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```

**Init System**

To run and manage your [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) process, you will be using your operating system’s built-in [init system](https://docs.mongodb.com/manual/reference/glossary/#term-init-system). Recent versions of Linux tend to use **systemd** (which uses the `systemctl` command), while older versions of Linux tend to use **System V init** (which uses the `service` command).

If you are unsure which init system your platform uses, run the following command:

```
ps --no-headers -o comm 1
```

```
sudo systemctl start mongod
sudo systemctl status mongod
sudo systemctl enable mongod
sudo systemctl stop mongod
sudo systemctl restart mongod
mongo
```

**Uninstall MongoDB Community Edition**

```
sudo service mongod stop
sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

