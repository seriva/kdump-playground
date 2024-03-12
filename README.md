# Kernel dump testing

A few simple steps and scripts to force a kernelpanic or kernelpanic on softlockup to explore kernel dump debugging with the help of some kernel modules. Only tested on Ubuntu20.04 and uses Vagrant+Hyper-V/VirtualBox to quickly spin up a VM for testing. You can also use any other generic Ubuntu20.04 install.

Launch VM:

```shell
cd vagrant
vagrant up --provider hyperv
#or
vagrant up --provider virtualbox
```

Enable ```kernel.softlockup_panic``` and ```kernel.panic```:

```shell
sudo vim /etc/sysctl.d/panic.conf
### Add line: kernel.softlockup_panic = 1 and kernel.panic = 20 then save.
sudo sysctl -p /etc/sysctl.d/panic.conf
sudo reboot now
```

Install and configure [kernel crash dump](https://ubuntu.com/server/docs/kernel-crash-dump):

```shell
sudo apt-get install linux-crashdump
```

Install prerequisites to build kernel modules:

```shell
sudo apt-get update
sudo apt-get install build-essential
```

Copy files in the ```softlockup``` or ```kernelpanic``` directory to the VM and build kernel module:

```shell
make
```

Install the kernelmodule:

```shell
sudo insmod softlockup.ko
# Or
sudo insmod kernelpanic.ko
```

Now wait between 30 to 60 seconds for the machine to reboot, kernel dump can now be found inside ```/var/crash```

More information about kernel dump debugging [here](https://www.youtube.com/watch?v=6l0ulgv1OJ4)
