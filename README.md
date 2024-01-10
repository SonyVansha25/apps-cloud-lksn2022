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
yum install gcc-c++ make git -y
yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
yum install nodejs -y
echo "step2 - Create env in github Backend"
git clone https://github.com/SonyVansha25/apps-cloud-lksn2022.git
touch /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_USER=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_PASS=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "DB_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "NODE_ENV=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_ACCESS_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_SECRET_KEY=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "AWS_BUCKET_NAME=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_HOST=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PORT=6379\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "REDIS_PASS=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "LOG_PATH=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
printf "CACHE_PATH=XXX\n" >> /home/ec2-user/apps-cloud-lksn2022/backend/.env
echo "step3 - npm install package"
sudo npm install pm2 --prefix /home/ec2-user/apps-cloud-lksn2022/backend
echo "step4 - npm install backend"
npm install --prefix /home/ec2-user/apps-cloud-lksn2022/backend
npm run start-apps --prefix /home/ec2-user/apps-cloud-lksn2022/backend
```
