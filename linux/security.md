# 安全相关

## 证书

### 证书内容

证书即是用来证明自己的文件，其中包含了证书签发者信息`Issuer`，过期时间`Validity`，公钥信息`Public Key Algorithm`以及签名算法。

使用`openssl x509 -noout -text -in ca.cer`可以查看证书内容：

```shell
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            76:66:6a:0a:29:61:70:cd:98:1e:6e:ba:f9:a4:80:8c:3f
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=CN, O=Huawei, CN=Huawei Equipment CA
        Validity
            Not Before: Aug 31 12:13:14 2017 GMT
            Not After : Oct 12 12:13:14 2041 GMT
        Subject: C=CN, O=Huawei, CN=Huawei Cloud CA
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:ab:c9:34:26:53:8c:c8:fc:9c:29:9f:58:4d:81:
                    f5:73:0e:2c:75:80:66:45:e4:d2:10:18:e2:96:f2:
                    e8:db:7e:95:5a:aa:70:1a:77:50:12:6a:08:d4:a7:
                    e0:bc:e4:81:c1:5e:da:ae:3a:68:15:2d:96:29:35:
                    93:8f:52:f1:ea:39:ce:65:71:d9:e5:96:4c:a1:a2:
                    a1:f2:67:71:a9:2f:0d:54:78:0a:8d:f3:04:32:53:
                    38:fc:71:c2:e4:91:d1:f4:5b:f0:ed:ca:35:a6:26:
                    4d:fe:b6:cd:88:89:d2:c9:e8:91:6f:a2:0c:ef:b5:
                    36:a4:06:cb:45:84:2d:9f:0d:e2:eb:47:cc:c3:82:
                    e2:06:01:f4:0f:bd:42:61:40:ac:30:7f:e9:bc:71:
                    11:89:b2:00:6d:ad:c6:6f:07:35:00:67:e6:10:af:
                    10:32:c4:12:e4:9b:55:61:db:8a:1e:05:e1:fb:72:
                    79:b8:68:d2:81:c8:78:a2:1f:34:ae:3c:be:87:8e:
                    78:13:c5:41:ef:4b:d2:d0:54:f0:43:a6:34:3d:39:
                    a6:ca:a9:85:18:68:8f:2b:6a:9a:b6:54:63:f6:27:
                    a9:0a:53:5a:97:b2:91:e2:35:31:b5:56:d1:86:79:
                    1d:16:d2:0b:a9:a0:a3:2b:e8:36:e8:b2:6b:e5:99:
                    d9:fd
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Authority Key Identifier: 
                keyid:2A:F8:10:59:27:80:35:1F:A7:7C:BA:3B:9F:2A:E4:4A:AA:9B:92:EA

            X509v3 Certificate Policies: 
                Policy: X509v3 Any Policy
                  CPS: http://support.huawei.com/support/pki

            X509v3 Basic Constraints: 
                CA:TRUE, pathlen:0
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Subject Key Identifier: 
                F4:92:E7:DC:E8:30:3D:EA:46:91:FC:50:E3:18:38:2F:C5:15:51:2B
    Signature Algorithm: sha256WithRSAEncryption
         9c:7d:f3:5e:c7:b2:54:8c:93:15:ff:0d:cb:36:19:7f:27:18:
         86:d7:97:bd:bf:ef:5a:65:f4:77:fd:95:63:98:72:a8:77:5b:
         c8:33:ec:7c:44:0d:8a:e0:a6:5e:7e:9c:8c:3c:8c:ac:62:dc:
         f6:c1:7e:84:97:42:76:93:63:3d:03:39:eb:0d:c2:41:f1:ec:
         39:fd:8d:be:ae:b2:8b:dc:c2:3c:5c:65:b0:7f:ec:67:9d:2e:
         b8:a2:0a:b1:bf:45:7d:02:a1:29:64:14:5a:36:57:48:e4:51:
         ae:ac:32:89:e6:68:0f:84:e6:4a:63:aa:05:f2:9a:f7:dd:5b:
         50:bb:ab:e0:7f:ea:b0:7f:21:26:4f:f9:16:14:b1:1a:a8:59:
         11:d7:58:3c:60:68:1e:e1:e6:6a:0a:3f:0d:11:f3:c8:f0:7e:
         b9:25:ee:6d:20:93:51:88:09:cd:50:33:87:f9:08:30:b4:b4:
         88:c1:c5:fa:f6:58:11:f8:ca:99:12:77:3c:c5:c0:7e:b0:3b:
         f6:cf:84:ea:87:d5:c9:70:ed:42:f0:b2:76:3f:a2:54:81:12:
         a9:dc:6b:22:28:13:af:9b:2a:5c:6b:dd:34:b5:34:b3:b0:e2:
         9e:c9:a3:02:77:4f:90:5a:12:77:6e:db:60:68:0e:8b:62:54:
         69:da:4c:37:4f:d6:44:99:15:91:57:6d:73:91:28:bd:6f:be:
         4d:5b:e3:d7:d6:79:4a:92:b0:de:59:7c:5b:fb:ed:f7:8e:9d:
         3a:99:86:a2:c2:f6:95:d8:58:5d:1b:84:7e:9c:42:06:3c:da:
         95:1e:a9:0b:ae:ce:76:11:ef:03:33:04:86:94:0d:f7:3f:46:
         54:c1:e4:28:ec:cf:82:c2:02:97:97:08:2f:9b:17:3d:19:d1:
         f9:18:7e:3e:b8:8c:af:5d:83:9b:86:fa:44:25:3b:78:82:72:
         b4:a4:85:0c:d3:c1:95:d7:3a:2d:68:3d:b4:84:05:59:42:c5:
         bf:b9:7f:80:76:76:85:79:2c:ec:19:3c:4a:e6:c4:19:0e:59:
         10:fa:d2:33:92:e2:c4:1f:b3:a0:30:cd:71:fd:25:c9:73:02:
         41:7e:e4:59:a2:f4:06:33:79:9b:a2:81:d9:dd:12:6e:1c:1b:
         2b:aa:d3:8c:6d:03:04:b1:d1:34:29:a7:24:ed:c9:6b:0e:b3:
         5c:10:f7:06:20:38:6e:8c:6c:99:b5:a8:ca:09:c4:94:a5:eb:
         e9:b9:27:c2:26:1f:65:47:dd:25:10:70:ee:df:23:77:e0:be:
         83:e1:e7:bb:5f:69:ff:2b:bf:77:0e:8f:a1:18:db:1c:cd:9e:
         24:e9:51:a0:f1:c3:80:a3
```

### 私钥内容

私钥是用来加密数据的，包含了所选用的**素数**，可使用`openssl rsa -noout -text -in private.key`命令查看。

```shell
Private-Key: (2048 bit)
modulus:
    00:b7:58:3b:99:f6:23:9f:94:1e:7c:92:57:26:b4:
    68:0d:68:49:51:ad:00:33:54:3b:c3:f5:36:fa:05:
    93:9a:a8:bb:55:a4:5a:02:48:22:50:28:18:51:b5:
    00:be:58:28:fe:79:5c:3d:e2:57:e0:d7:2a:9e:38:
    cf:77:40:a3:d8:d5:a2:bb:6b:c1:2f:83:b4:fa:12:
    06:34:be:f1:55:08:95:0f:9b:62:b5:41:7e:5a:70:
    63:dd:94:12:35:50:b9:86:f2:4a:5a:71:91:dc:d4:
    0a:5c:8b:1d:c6:70:56:bb:05:14:a3:da:9b:73:b5:
    0d:8d:8c:d9:09:3d:b7:10:41:1d:45:b8:e8:ee:13:
    4d:84:22:7c:1a:ca:74:2f:5e:01:57:db:88:16:9a:
    d5:d2:69:d4:be:63:c0:0c:52:56:5f:c0:a5:d2:9a:
    f9:b9:17:6f:8d:83:f1:f2:b4:c2:da:ed:5f:b7:56:
    0e:2d:8f:55:7f:b7:4e:a0:8b:fa:40:fb:ad:77:7e:
    80:ae:36:51:17:42:5e:7f:7e:b8:ec:0a:ef:0f:67:
    09:f5:94:39:be:3a:d2:7e:f1:07:19:d9:8e:98:71:
    1b:36:5e:80:f4:1d:c4:26:54:f7:1e:7a:ef:bb:4a:
    ea:bc:8f:a5:46:87:08:32:56:39:bd:e1:36:43:dd:
    5a:a7
publicExponent: 65537 (0x10001)
privateExponent:
    22:6b:26:ed:35:3e:e4:c3:3a:a1:ea:1b:00:42:17:
    d2:84:84:57:17:e2:14:c0:b8:63:86:12:27:07:8a:
    f9:1a:b9:e8:4e:ed:32:c6:75:e2:94:54:54:6a:65:
    54:56:5a:83:c3:26:a4:db:6b:e1:e2:a2:b6:87:35:
    b3:4e:68:33:d9:8f:2e:1e:01:6b:39:0f:6d:6e:9a:
    c9:a6:1a:c9:fa:cd:78:db:93:c1:31:25:56:37:09:
    18:5c:a6:ba:69:88:1a:a4:3b:7f:15:18:eb:da:c6:
    83:2e:fe:8c:f5:23:e3:02:06:28:e8:6e:0c:5f:ad:
    c6:a7:ed:4a:85:d2:6a:f3:6c:cd:e7:88:be:6c:b1:
    dd:66:17:94:4c:46:e9:73:89:ba:a7:f8:26:35:76:
    e1:c2:59:50:a4:72:59:fe:12:2f:4b:95:42:75:9a:
    ff:dd:9e:fe:f6:4f:c3:59:15:64:91:6d:f0:0c:eb:
    a4:21:f2:d0:76:e6:3d:8b:69:c8:26:05:1e:a3:d9:
    6c:ff:e2:0b:7f:b3:98:da:0e:c7:f1:ef:18:60:7b:
    ff:33:73:6f:99:77:b1:32:43:ae:32:db:8e:d6:d8:
    78:50:20:ab:d4:df:2c:98:70:39:a1:fc:83:79:e8:
    41:06:d3:3e:f8:45:d1:fd:73:28:4c:4c:57:4e:6e:
    f9
prime1:
    00:e8:a6:18:8d:c5:f0:68:84:2f:b7:6b:ed:08:93:
    e3:f0:e3:8a:5b:50:ad:c2:be:03:3a:e2:01:6d:4e:
    37:8e:95:75:57:18:d2:df:9d:6b:47:ca:45:d7:17:
    14:19:a1:0c:f0:3e:93:c7:e1:37:77:b9:9c:59:f6:
    7c:46:1a:b0:1e:24:83:10:2f:cd:df:51:21:72:14:
    a9:3e:36:f4:34:45:39:18:f2:cf:00:17:d1:1a:e1:
    73:0f:98:ef:0c:58:a0:f5:59:2c:ee:03:c8:7b:3c:
    56:21:7b:80:06:c8:ba:be:55:7d:d6:d2:4a:87:c2:
    b6:9c:0a:81:1b:76:95:27:35
prime2:
    00:c9:bf:44:a9:af:d0:d2:6f:ea:ac:a9:9f:d3:11:
    ed:50:cf:2b:b9:83:a9:2d:25:ba:9f:35:6d:1a:12:
    ce:0e:d2:24:c2:7a:b7:32:46:f0:a8:6d:42:00:70:
    5d:aa:36:17:e6:1b:ca:bf:f6:0a:5b:9c:94:20:5f:
    5e:39:de:d8:47:40:a6:aa:a2:60:e3:e5:c3:0e:92:
    8a:e9:4d:2e:0d:f3:dd:d2:b8:94:b0:16:77:37:2c:
    c1:6d:be:d4:4a:7d:85:60:e9:e5:15:97:b8:bf:22:
    8f:39:78:bc:51:0f:8f:88:b5:3e:49:c4:20:c4:e3:
    7f:4c:58:cf:4f:4e:fb:89:eb
exponent1:
    00:ad:59:15:72:8d:ca:54:16:2a:77:c1:33:1c:f1:
    15:26:a8:64:af:25:ae:53:2c:c1:bb:5e:2b:32:06:
    d5:de:a7:fe:86:a7:38:78:e6:12:c9:ab:2d:de:d6:
    93:63:85:5c:de:a5:c1:f7:6d:18:a7:5c:e5:e3:22:
    85:f6:b6:6c:04:09:b0:9e:9f:00:07:25:ac:86:14:
    5f:06:bc:6b:b9:86:da:c4:7c:27:41:2a:7d:e4:49:
    98:32:47:01:d7:71:32:7d:9d:b4:9c:a4:b4:fd:1e:
    38:0f:3a:b6:6a:57:a6:cd:ab:65:d8:6b:58:9d:4f:
    b8:41:0c:92:a6:a3:f4:ff:01
exponent2:
    2a:1a:55:8d:08:19:0f:61:b5:da:f4:c7:5a:f8:39:
    cb:8c:80:b6:85:88:8f:d8:78:da:f2:fe:62:db:9b:
    cf:32:01:e3:e8:52:45:16:d2:f0:83:64:ab:cc:c7:
    6f:57:3c:a3:94:17:3a:fa:70:ec:ef:7f:66:46:20:
    d6:f8:ef:af:e7:09:21:41:23:f1:4b:a8:ea:68:a0:
    be:d1:91:93:ed:d9:d4:39:1b:26:a8:79:96:b5:2b:
    64:e8:d7:4a:f8:69:16:71:99:92:69:08:70:33:19:
    2a:c8:4c:9c:aa:1f:ad:4b:0d:c5:bd:3d:8d:79:60:
    be:a1:26:1a:8a:04:2a:a3
coefficient:
    38:fc:e5:85:f3:aa:a5:28:49:73:f4:bd:ba:83:ee:
    87:ce:3b:76:1f:75:5f:2e:1f:49:13:90:f6:78:d5:
    8b:77:a4:c3:ca:92:39:80:96:7f:f3:50:39:9f:1a:
    71:3f:3e:fa:3a:17:3a:53:75:78:65:88:bc:d2:04:
    3b:aa:ac:fb:86:6e:50:e5:42:54:bb:df:c0:65:6e:
    b4:73:13:f5:21:73:78:db:46:d0:bf:b0:43:6b:8d:
    9f:81:fc:fb:7c:b5:00:cb:34:4e:9d:1b:e4:d6:12:
    7a:72:a4:35:91:06:4a:61:ac:2d:59:f3:98:32:67:
    e1:b9:e3:40:bb:25:90:be
```

### 常用命令

**证书校验：**`openssl verify -CAfile cacert cert1 cert2`

**生成密钥：**`openssl genrsa -out RSA.pem`

**密钥格式转换：**`openssl pkcs8 -topk8 -in key_encrypt.pem -passin pass:123456 -out key_pkcs8.pem -nocrypt`

**私钥加密：**`openssl rsa -in RSA.pem -des3 -passout pass:123456 -out E_RSA.pem`

**私钥解密：**`openssl rsa -in E_RSA.pem -passin pass:123456 -out P_RSA.pem`

**查看密钥参数**：`openssl rsa -in key_encrypt.pem -des -passin pass:123456 -text -noout`

## openssl

### 连接s_client

**使用指定协议连接：**`openssl s_client -debug -state -showcerts -connect 100.95.166.21:27211 -tls1_2 -CAfile ca.cer -cert dra.pem -key dra.key`