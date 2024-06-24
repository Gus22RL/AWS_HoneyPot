## Launch Instance 
Named 
![[Images/Instance-1.png]]

Choose a Debian distrit  and type
![[Images/Instance-2.png]]


Choose our Honey-vpc and Security group
![[Images/Instance-3.png]]

We select 30 GiB to install and collect information.
![[Images/Instance-4.png]]
## Configuration 
#### Download and install cowrie

Once logged into the instance, we will perform an update and an upgrade.
Before install Git.

```
$ sudoapt install git
```

We are going to follow the instalation https://cowrie.readthedocs.io/en/latest/INSTALL.html.
#### Install system dependencies 
```
$ sudo apt-get install git python3-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind virtualenv python3.11-venv
```
#### Create a user 
```
$ sudo adduser --disabled-password cowrie
$ sudo su - cowrie
```

#### Clone Repository
```
$ git clone http://github.com/cowrie/cowrie
$ cd cowrie 
```
#### Setup Virtuial Enviroment
```
$ sudo su - cowrie
$ python -m venv cowrie-env
$ source cowrie-env/bin/activate
	(cowrie-env) $ python -m pip install --upgrade pip
	(cowrie-env) $ python -m pip install --upgrade -r requirements.txt
```
#### Install configuration file
```
$ cp etc/cowrie.cfg.dist etc/cowrie.cfg
$ nano etc/cowrie.cfg
	Edit:
	[telnet]
	enabled = true"
```
#### Redirect ssh and telnet
ssh
```
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```
telnet
```
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 23 -j REDIRECT --to-port 2223
```

#### Starting Cowrie
```
$ bin/cowrie start
```
####  Edit Security group
Add a new rol to allow access Telnet
![[SG.png]]

