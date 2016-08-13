# Software Setup for the Rosie Repository

The instructions below assume you're installing this software on a Raspberry Pi 3 running Debian.
If your hardware or OS is different, be sure to inspect the instructions below to note OS differences, 
which would likely manifest either in the package manager used or the platform architecture (ARM is assumed below).

## Installing InfluxDB

There are no prepackaged binaries for InfluxDB, Telegraf and Grafana that work with ARM-based devices, 
so you'll need to build them yourself.

### Install Node.js and NPM

Raspbian already has node 0.10.29, but not npm. Let's grab a newer version of both.

Start with fetching the latest package list from your manager.

```
sudo apt-get update
```

Install some essential build tools we need, then node and npm
```
sudo apt-get install build-essential
sudo apr-get install nodejs
sudo apt-get install npm
```

Now create a symlink to your new version of node

```
sudo ln -s /usr/bin/nodejs/usr/bin/node
```

### Install Go

To build InfluxDB (as well as Telegraf and Grafana), you will need [Go](https://golang.org/).

Start by installing bison and the Go Version Manager (gvm)

```
sudo apt-get install bison
wget https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer
chmod +x gvm-installer
./gvm-installer
```

Versions of Go installed will be saved to `~/.gvm/scripts/gvm`.

You want Go 1.6, but due to a bug in the 1.6 installer, will need to install version 1.4.3 first.

```
gvm install go1.4.3
gvm use go1.4.3
export GOROOT_BOOTSTRAP=$GOROOT
```

Now, install Go 1.6.2

```
gvm install go1.6.2
gvm use go1.6.2 --default
```

### Install InfluxDB

Now to the good stuff. Install a couple of additional dependencies before grabbing the Go source.

```
sudo apt-get install ruby-dev
sudo gem install fpm
```

Download and build InfluxDB from source

```
go get github.com/influxdata/influxdb
cd $GOPATH/src/github.com/influxdata/influxdb/
./build.py --package --version=0.13.0 --arch=armhf
```

Setting the `arch` flag to `armhf` is critical for installing InfluxDB on ARM devices like the Pi.

When you execute the Python build script, you might get a (non-fatal) error message. You can ignore this.

Finally, install InfluxDB. Note that the exact name of your `deb` package might differ from the one below. check the `build` directory to get the exact name.

```
sudo dpkg -i build/influxdb_0.13.0~4254ad3_armhf.deb
```

To see if things worked, run the `influxd` command and then `influx`. You should jump into the InfluxDB cli once the latter command is issued.

Now that InfluxDB is up, lets jump to Telegraf...

## Installing Telegraf

## Installing Grafana

## Auto-starting services

Once you've installed everything, you'll no-doubt want to set up your services to auto-start on a reboot. 
This will save you time each time you power-cycle your devices. 
What's more, having your services managed by `systemd` will allow you to restart and shut them down without jumping through hoops.

### Auto-starting InfluxDB

### Auto-starting Grafana

### Auto-starting Telegraf