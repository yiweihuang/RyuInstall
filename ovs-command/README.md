# OVS Setup

## Setting up Open Vswitch experimental environment

We will use Ubuntu 14.04-lts as our OS, but you can also choose your own OS.

#### 1. Download Open Vswitch project from git or download it from official site directly
```
$ git clone https://github.com/openvswitch/ovs.git
$ wget http://openvswitch.org/releases/openvswitch-2.4.0.tar.gz
```
#### 2. Prepare your building environment
`$ apt-get install autoconf automake libtool`
#### 3. Building Open Vswitch
(if you **shut down** the pc, you have to **rerun** this step.)
```
$ ./boot.sh
$ ./configure
$ make && make install
$ /sbin/modprobe openvswitch
```
#### 4. Initialize the configuration database.
```
$ mkdir -p /usr/local/etc/openvswitch
$ ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema
```
#### 5. Run up the daemon
```
$ ovsdb-server --remote=punix:/usr/local/var/run/openvswitch/db.sock --remote=db:Open_vSwitch,Open_vSwitch,manager_options --private-key=db:Open_vSwitch,SSL,private_key --certificate=db:Open_vSwitch,SSL,certificate --bootstrap-ca-cert=db:Open_vSwitch,SSL,ca_cert --pidfile --detach
$ ovs-vsctl --no-wait init
$ ovs-vswitchd --pidfile --detach
```
#### 6. Now your environment is ready

## Creating a simple topology (composed of one switch, two hosts, and one controller)

#### 1. You need to set up *n* hosts that connect to the Open Vswitch

#### 2. The IP addresses of the two hosts are static. You should set up the IP addresses manually

`$ sudo ifconfig *interface* *ip*`
<br>

Example
<br>
`$ sudo ifconfig eth0 10.0.0.100`

#### 3. Set the default routing table on the two hosts to enable them to reach the switch
`$ route add –net *ip* netmask *ip* dev *interface*`
<br>

Example
<br>
`$ route add –net 10.0.0.0 netmask 255.0.0.0 dev eth0`

#### 4. Set Open Vswitch to be an OpenFlow switch. You can use the following commands
+ Prints a brief overview of the database contents
  <br>
  `$ ovs-vsctl show`

+ Creates  a  new  bridge named bridge
  <br>
  `$ ovs-vsctl add-br *bridge name*`
  <br>
  *Example*
  <br>
  `$ ovs-vsctl add-br br0`

+ Creates on bridge a new port named port that bonds together the network devices given as each iface
  <br>
  `$ ovs-vsctl add-port *bridge name* *interface*`
  <br>
  *Example*
  <br>
  `$ ovs-vsctl add-port br0 eth1 `

+ Sets the configured controller target or targets
  <br>
  `$ ovs-vsctl set-controller *bridge name* tcp:*ip*:*port*`
  <br>
  *Example*
  <br>
  `$ ovs-vsctl set-controller br0 tcp:192.168.3.4:6633`

#### 5. One each host, ping the other host to force the controller to learn and insert flow entries into the Open Vswitch

#### 6. Download and install the Ryu Controller
```
$ curl -sL https://raw.githubusercontent.com/sdnds-tw/ryuInstallHelper/master/ryuInstallHelper.sh | bash
$ ryu-manager simple_switch13.py
```
