1. تنظیمات اولیه و نصب Docker، docker-compose، Node.js، npm و yarn
ابتدا سیستم را به‌روزرسانی و ابزارهای لازم را نصب می‌کنیم:

#!/bin/bash

# 1. آپدیت مخازن و نصب Docker و Docker Compose
```sh 
sudo apt-get update
sudo apt-get install docker.io -y
```
# نصب Docker Compose از آخرین نسخه
VERSION=$(curl --silent https://api.github.com/repos/docker/compose/releases/latest | grep -Po '"tag_name": "\K.*\d')
DESTINATION=/usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/${VERSION}/docker-compose-$(uname -s)-$(uname -m) -o $DESTINATION
sudo chmod 755 $DESTINATION

# نصب Node.js، npm و yarn
sudo apt-get install npm -y
sudo npm install n -g
sudo n stable
sudo npm i -g yarn

# بررسی نصب‌ها
docker --version
docker-compose --version
node -v
npm -v
yarn -v
