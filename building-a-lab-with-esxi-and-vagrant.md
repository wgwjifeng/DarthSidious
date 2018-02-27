# Building a lab with VMware ESXi and Vagrant

## Lab plan

VPN access will be set up to connect straight into the network, but no domain user provided.

## Domain plan

Domain name etc

## Prepping

### Hardware requirements

* ESXi 6.5 compatible hardware \(can use 6.0 if incompatible\)
* Enough RAM
* A separate drive for installing ESXi \(rquires only 8 GB\) and one for the actual VMs \(500 GB+\).
* A USB drive to install ESXi with
* A separate computer to do management from, in ESXi 6.5

### Installing Vagrant

Install Vagrant and the vagrant VMware ESXi plugin.

This plugin requires ovftool from VMware. Download from VMware website.

[https://www.vmware.com/support/developer/ovf/](https://www.vmware.com/support/developer/ovf/)

[https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)

[josenk/vagrant-vmware-esxi: A Vagrant plugin that adds a vmware ESXi provider support.](https://github.com/josenk/vagrant-vmware-esxi)

```
vagrant plugin install vagrant-vmware-esxi
vagrant version
```

'How to enable ssh access on esxi'

#### Download operating systems in Vagrant

Using the following syntax download the required operating systems using Vagrant. Select `vmware_desktop` as provider when prompted. It is wise to choose boxes from the Vagrant cloud that doesn't have any configuration management built in; those are usually indicated by `nocm`.

    vagrant box add opentable/win-2008r2-enterprise-amd64-nocm
    vagrant box add opentable/win-2012r2-standard-amd64-nocm
    vagrant box add StefanScherer/windows_2016
    vagrant box add opentable/win-7-enterprise-amd64-nocm
    vagrant box add StefanScherer/windows_10`

[Vagrant box opentable/win-2008r2-enterprise-amd64-nocm - Vagrant Cloud](https://app.vagrantup.com/opentable/boxes/win-2008r2-enterprise-amd64-nocm)

[Vagrant box opentable/win-2012-standard-amd64-nocm - Vagrant Cloud](https://app.vagrantup.com/opentable/boxes/win-2012-standard-amd64-nocm)

[Vagrant box StefanScherer/windows\_2016 - Vagrant Cloud](https://app.vagrantup.com/StefanScherer/boxes/windows_2016)

[Vagrant box opentable/win-7-enterprise-amd64-nocm - Vagrant Cloud](https://app.vagrantup.com/opentable/boxes/win-7-enterprise-amd64-nocm)

[Vagrant box StefanScherer/windows\_10 - Vagrant Cloud](https://app.vagrantup.com/StefanScherer/boxes/windows_10)

## Installing ESXI

Download ESXI 6.5 image [https://my.vmware.com/en/group/vmware/evalcenter?p=free-esxi6](https://my.vmware.com/en/group/vmware/evalcenter?p=free-esxi6)

Use [Rufus ](http://rufus.akeo.ie/)to make a bootable USB key from the ESXI image.

Boot the lab machine from USB and install ESXi on the small drive as per instruction.

After installation, reboot the server. ESXi should now provide a DHCP-leased IP-address you can access from a web panel.

### Enabling ESXi shell and SSH

We want to enable SSH, so on the ESXi server itself, press F2

1. At the direct console of the ESXi host, press F2 and provide credentials when prompted.
2. Scroll to Troubleshooting Options and press Enter.
3. Choose Enable ESXi shell and Enable SSH and press Enter once on each of them
4. Press Esc until you return to the main direct console screen.

### Set static IP for the ESXi host

1. Press F2 on the ESXi console, provide credentials

2. Configure management network -&gt; IPV4 Configuration

3. Press space on `Set static ipv4 address`

4. Press Esc until you return to the main direct console screen.

### Adding a datastore to ESXi

Add the big drive, where the virtual machines will be stored as a datastore in ESXi. In the ESXi web client press `Storage`in the left side pane. Just follow the instructions after selecting `New datastore`from the menu, add a drive, give it a name like `VMs` and use the whole drive as one partition.

### Adding a network configuration to ESXi

Select Networking on the left side pane, click port group, ADD port group. Give it the name `Lab Network`, asign it to `VLAN 0`, assign it to `vSwitch0 `which is the default virtual switch.
