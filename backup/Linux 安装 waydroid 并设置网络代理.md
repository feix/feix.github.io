## 1. 安装
Waydroid要求内核中包含binder模块，Ubuntu 版本已经满足。按照[官网安装说明](https://docs.waydro.id/usage/install-on-desktops), https://github.com/waydroid/waydroid
```
sudo apt install curl ca-certificates -y
curl https://repo.waydro.id | sudo bash
sudo apt install waydroid -y
```

## 2. 启动，常用操作
```
sudo waydroid init
sudo waydroid container start
sudo systemctl restart waydroid-container.service
waydroid prop set persist.waydroid.multi_windows true
# 启动 
waydroid session start
# 启动 UI
waydroid show-full-ui
# 查看状态
waydroid status
# 进入adb shell
waydroid shell
```

## 3. 注册为 Google 设备
[waydroid_script](https://github.com/casualsnek/waydroid_script)
```
sudo waydroid init -s GAPPS -f
git clone https://github.com/casualsnek/waydroid_script
cd waydroid_script
sudo python3 -m pip install -r requirements.txt
sudo python3 main.py certified
```
复制获取的 ID，进入[设备注册页面](https://www.google.com/android/uncertified/?pli=1)，登录谷歌账户并输入前面生成的ID，设置完需要重启 `sudo systemctl restart waydroid-container.service`

安装 libhoudini，支持 arm 架构 apk
```
# cd waydroid_script
sudo python3 main.py install libhoudini
waydroid app install /path/to/apk
```

## 4. 网络代理配置
参考 waydroid/waydroid/issues/870
```
adb shell settings put global http_proxy "ip:port"  
cert_hash=$(openssl x509 -subject_hash_old -in ssl-proxying-certificate.pem | head -1)
sudo mkdir -p /var/lib/waydroid/overlay/system/etc/security/cacerts/
sudo cp ssl-proxying-certificate.pem /var/lib/waydroid/overlay/system/etc/security/cacerts/${cert_hash}.0
```
设置完成后需要重启 `sudo systemctl restart waydroid-container.service`

