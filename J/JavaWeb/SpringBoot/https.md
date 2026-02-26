# 使用加密https协议

#### 加入配置信息

```
server.port=6150
server.ssl.key-store=classpath:keystore.p12  //resource文件夹下需有此证书
server.ssl.key-store-password=yourpassword  //对应证书内设置的密码
server.ssl.key-store-type=PKCS12  //证书的格式 PKCS12:包含公钥+私钥的容器 有密码保护
server.ssl.key-alias=...  //别名
```

**注** 加入上述配置后 就只能使用https协议连接服务器

服务器需持有keystore.p12 客户端需持有公钥

#### 生成自签名证书

##### 生成keystore.p12包含私钥、公钥、证书

```
//cmd
//keytool (JDK自带)
keytool -genkeypair
  -alias VL_Server  //别名
  -keyalg RSA
  -keysize 2048
  -storetype PKCS12
  -keystore keystore.p12
  -validity 3650  //生命周期
  -storepass 61504002
  -dname "CN=192.168.1.6, O=Server, L=Huge_Box, ST=Huge_Box, C=CN"  //例
```

##### 提取公钥

```
//cmd
keytool -exportcert
  -alias VL_Server
  -keystore keystore.p12
  -storetype PKCS12
  -storepass 61504002
  -file server.cer
  -rfc
```

