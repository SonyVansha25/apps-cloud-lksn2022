# apps-cloud-lksn2022
Web application for Cloud Computing LKSN 2022

# Backend Service
## Install Dependencies
### `npm install --prefix <your path>`

## Running Apps
### `npm run start-apps`

# Frontend Service
## Setting Environment Variables
### REACT_APP_BACKEND = http://your-ip:5000

## Install Dependencies
### `npm install --prefix <your path>`

## Available Scripts
### `npm run start`

## Run Web Apps
Open browser and access your ip+port, ex: localhost:3000

## Backend Userdata
```sh
#!/bin/bash
echo "step1 - install dependencies"
yum install -y gcc-c++ make git amazon-efs-utils
echo "step2 - install nodejs v16"
yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
yum install -y nodejs
echo "step3 - Clone github Backend"
mkdir /home/ec2-user/apps-cloud-lksn2022
git clone https://github.com/SonyVansha25/apps-cloud-lksn2022.git /home/ec2-user/apps-cloud-lksn2022
echo $(ls /home/ec2-user/apps-cloud-lksn2022)

echo "step4 - Create env"
touch /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_USER=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_PASS=adminsony\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "NODE_ENV=production\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_ACCESS_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_SECRET_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_BUCKET_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PORT=6379\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PASS=XXXn" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "LOG_PATH=/var/log\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "CACHE_PATH=/var/tmp\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env

echo "step5 - mount efs"
printf "180.10.X.X:/ /var/log nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvpor,_netdev 0 0\n" >> /etc/fstab
printf "180.10.X.X:/ /var/tmp nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport,_netdev 0 0\n" >> /etc/fstab

echo "step6 - npm install package"
npm install pm2 --prefix /home/ec2-user/apps-cloud-lksn2022/backend
npm install --prefix /home/ec2-user/apps-cloud-lksn2022/backend
npm run start-apps --prefix /home/ec2-user/apps-cloud-lksn2022/backend
echo "step7 - Done"
```


## frontend Userdata
```sh
#!/bin/bash
echo "step1 - install dependencies"
yum install -y gcc-c++ make git httpd amazon-efs-utils
echo "step2 - install nodejs v16"
yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
yum install -y nodejs

echo "step3 - Clone github Backend"
mkdir /home/ec2-user/apps-cloud-lksn2022
git clone https://github.com/SonyVansha25/apps-cloud-lksn2022.git /home/ec2-user/apps-cloud-lksn2022
echo $(ls /home/ec2-user/apps-cloud-lksn2022)
systemctl start httpd
systemctl enable httpd

echo "step4 - Create env"
touch /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "REACT_APP_BACKEND=http://elb-frontend-XXX.us-east-1.elb.amazonaws.com:5000\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "REDIS_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "REDIS_PORT=6379\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "REDIS_PASS=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "LOG_PATH=/var/log\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env
printf "CACHE_PATH=/var/log\n" >> /home/ec2-user/apps-cloud-lksn2022/frontend/.env

echo "step5 - Create Porxy"
touch /etc/httpd/conf.d/proxy.conf
printf "Listen 5000\n" >> /etc/httpd/conf.d/proxy.conf
printf "<VirtualHost *:5000>\n" >> /etc/httpd/conf.d/proxy.conf
printf 'Header set Access-Control-Allow-Origin "*"\n' >> /etc/httpd/conf.d/proxy.conf
printf 'Header set Access-Control-Allow-Methods "GET, PUT, POST, DELETE, PATCH"\n' >> /etc/httpd/conf.d/proxy.conf
printf "    ProxyPreserveHost On\n" >> /etc/httpd/conf.d/proxy.conf
printf "    ProxyPass / http://internal-elb-backend-XXX.us-east-1.elb.amazonaws.com/\n" >> /etc/httpd/conf.d/proxy.conf
printf "    ProxyPassReverse / http://internal-elb-backend-XXX.us-east-1.elb.amazonaws.com/\n" >> /etc/httpd/conf.d/proxy.conf
printf "</VirtualHost>\n" >> /etc/httpd/conf.d/proxy.conf
systemctl restart httpd
systemctl status httpd
echo $(cat /etc/httpd/conf.d/proxy.conf)

echo "step6 - mount efs"
printf "180.10.X.X:/ /var/log nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvpor,_netdev 0 0\n" >> /etc/fstab
printf "180.10.X.X:/ /var/tmp nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport,_netdev 0 0\n" >> /etc/fstab
mount -a

echo "step7 - npm install package"
npm install --prefix /home/ec2-user/apps-cloud-lksn2022/frontend
npm run start --prefix /home/ec2-user/apps-cloud-lksn2022/frontend
echo "step8 - Done"
```
