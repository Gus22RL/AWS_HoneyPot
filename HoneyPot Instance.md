## Launch Instance 
Named 
![[Instance-1.png]]

Choose a Debian distrit  and type
![[Instance-2.png]]


Choose our Honey-vpc and Security group
![[Instance-3.png]]

We select 30 GiB to install and collect information.
![[Instance-4.png]
## Configuration HoneyPot

Once logged into the instance, we will perform an update and an upgrade.
Before install Git.

```
sudo apt install git
```

### Clone heralding repository

```
git clone https://github.com/johnnykv/heralding.git
```
#### Install system dependencies 
```
sudo apt-get install -y python3 python3-pip python3-venv docker.io
```

#### Build the Image 
```
sudo docker build -t heralding_modificado .
```
#### Run Docker
```
sudo docker run -d \
  -p 21:21 \
  -p 22:22 \
  -p 23:23 \
  -p 25:25 \
  -p 80:80 \
  -p 110:110 \
  -p 143:143 \
  -p 443:443 \
  -p 465:465 \
  -p 993:993 \
  -p 995:995 \
  -p 1080:1080 \
  -p 2222:2222 \
  -p 3306:3306 \
  -p 3389:3389 \
  -p 5432:5432 \
  -v /home/admin/honeypot-logs:/var/log/heralding \
  -v /home/admin/heralding-config:/etc/heralding \
  -v /home/admin/heralding-data:/var/lib/heralding \
  heralding
```
#### Change ssh port (optional)
SSH
```
sudo nano /etc/ssh/sshd_config
Port 64295 <--- (change)
```
Reload de service
```
sudo systemctl restart sshd
```
####  Edit Security group

Add al pots as we need.
![[SG-01.png]]

## Send logs to S3

Configure awscli
```
aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]: 
```

#### Sincronice directory-buket

```
aws s3 sync /honeypot-logs s3://honey-buket/
aws s3 ls s3://honey-buket/
```