分两步
1. 获取密码 hash
2. 设计密码模式，利用 hashcat 碰撞密码
3. 如果成功，密码会保存在 excel 同名的 .hash.txt 后缀文件里，最后一个「:」 后面的就是密码。

直接上脚本，使用方式 `/bin/bash run.sh excel_file  ?d?d?d?d`。
参数1 为文件名，参数2 为 密码模式（示例中为 4位数字），参考 https://hashcat.net/wiki/doku.php?id=mask_attack#built-in_charsets

```run.sh
#!/bin/bash

# filename
file=${1}
# password mode
dict=${2:-?d?d?d?d}

# prepare tools
command -v hashcat || sudo apt install -y hashcat
test -f office2john.py || wget https://github.com/magnumripper/JohnTheRipper/raw/bleeding-jumbo/run/office2john.py

# get hash from excel
python office2john.py $file | awk -F: '{print $2}' > ${file}.hash

# crack，try every office version
for m in {9400,9700,9500,9600,25300,9710,9720,9810,9820,9800}; do
	hashcat --show -m $m -a 3 -o ${file}.hash.txt ${file}.hash ${dict} && break
done