1. تنظیمات اولیه و نصب Docker، docker-compose، Node.js، npm و yarn
ابتدا سیستم را به‌روزرسانی و ابزارهای لازم را نصب می‌کنیم:

#!/bin/bash

# 1. آپدیت مخازن و نصب Docker و Docker Compose
```sh 
sudo apt-get update
sudo apt-get install docker.io -y
```
# نصب Docker Compose از آخرین نسخه
```sh 
VERSION=$(curl --silent https://api.github.com/repos/docker/compose/releases/latest | grep -Po '"tag_name": "\K.*\d')
DESTINATION=/usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/${VERSION}/docker-compose-$(uname -s)-$(uname -m) -o $DESTINATION
sudo chmod 755 $DESTINATION
```

# نصب Node.js، npm و yarn
```sh 
sudo apt-get install npm -y
sudo npm install n -g
sudo n stable
sudo npm i -g yarn
```

# بررسی نصب‌ها
```sh 
docker --version
docker-compose --version
node -v
npm -v
yarn -v
```

2. کلون کردن مخزن GitHub و بیلد پروژه
 # کلون کردن پروژه از GitHub و نصب وابستگی‌ها
```sh 
git clone https://github.com/CATProtocol/cat-token-box
cd cat-token-box
```
# نصب وابستگی‌های پروژه و بیلد
```sh 
sudo yarn install
sudo yarn build
```

3. اجرای Docker Container
# تغییر به دایرکتوری مربوط به tracker و تنظیمات دسترسی
```sh 
cd ./packages/tracker/
sudo chmod 777 docker/data
sudo chmod 777 docker/pgdata
```

# بالا آوردن Fractal Node با استفاده از Docker Compose
```sh 
sudo docker-compose up -d
```

# ساختن Docker image برای tracker و اجرای آن
```sh 
cd ../../
sudo docker build -t tracker:latest .
sudo docker run -d \
    --name tracker \
    --add-host="host.docker.internal:host-gateway" \
    -e DATABASE_HOST="host.docker.internal" \
    -e RPC_HOST="host.docker.internal" \
    -p 3000:3000 \
    tracker:latest
```
4. ایجاد کیف پول
# تنظیمات کیف پول در فایل config.json
```sh 
cd packages/cli
cat <<EOT > config.json
{
  "network": "fractal-mainnet",
  "tracker": "http://127.0.0.1:3000",
  "dataDir": ".",
  "maxFeeRate": 100,
  "rpc": {
      "url": "http://127.0.0.1:8332",
      "username": "bitcoin",
      "password": "opcatAwesome"
  }
}
EOT
```

# ساختن کیف پول جدید
```sh 
sudo yarn cli wallet create
```
5. اسکریپت برای تکرار Mint
در مرحله‌ی آخر، یک اسکریپت برای تکرار عملیات mint می‌سازیم:
# ایجاد اسکریپت برای تکرار mint
```sh 
cd packages/cli
cat <<EOT > script.sh
#!/bin/bash

command="sudo yarn cli mint -i 45ee725c2c5993b3e4d308842d87e973bf1951f5f7a804b21e4dd964ecd12d6b_0 5"

while true; do
    \$command

    if [ \$? -ne 0 ]; then
        echo "命令执行失败，退出循环"
        exit 1
    fi

    sleep 1
done
EOT
```

# افزودن دسترسی به اسکریپت
```sh 
chmod +x script.sh
```

# اجرای اسکریپت
```sh 
./script.sh
```



