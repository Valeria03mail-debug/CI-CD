# Домашнее задание к занятию "`Защита хоста`" - `Гаврилова Валерия`

### Задание 1

```
sudo apt update
sudo apt install ecryptfs-utils -y
sudo useradd -m -s /bin/bash cryptouser
sudo passwd cryptouser
sudo pkill -u cryptouser
sudo ecryptfs-migrate-home -u cryptouser
sudo login
 # логин: cryptouser
 # пароль: ...
ls -la /home/cryptouser
```
после создания пользователя, но до шифрования:
![alt text](image-6.png)

```
echo "secret data" > /home/cryptouser/test.txt
ls -la /home/cryptouser
sudo ls -la /home/cryptouser
```
после шифрования, из-под root:

![alt text](image-7.png)

вход под cryptouser после шифрования:

![alt text](image-8.png)
---

### Задание 2

```
sudo apt update
sudo apt install cryptsetup -y
dd if=/dev/zero of=~/luks-container.img bs=1M count=100
sudo losetup -fP ~/luks-container.img
sudo losetup -l
```
Создание контейнера/раздела

![alt text](image-9.png)
![alt text](image-10.png)

```
sudo cryptsetup luksFormat /dev/loop33
```
Инициализация LUKS

![alt text](image-11.png)

```
sudo cryptsetup open /dev/loop33 myencrypted
ls -la /dev/mapper/myencrypted
sudo mkfs.ext4 /dev/mapper/myencrypted
```
Открытие и создание ФС

![alt text](image-12.png)

```
sudo mkdir /mnt/secret
sudo mount /dev/mapper/myencrypted /mnt/secret
df -h | grep secret
echo "Секретные данные" | sudo tee /mnt/secret/test.txt
sudo cat /mnt/secret/test.txt
ls -la /mnt/secret/
```
Монтирование и создание файлов

![alt text](image-13.png)

```
sudo umount /mnt/secret
sudo cryptsetup close myencrypted
sudo hexdump -C /dev/loop0 | head -20
```
Демонстрация шифрования 

![alt text](image-14.png)