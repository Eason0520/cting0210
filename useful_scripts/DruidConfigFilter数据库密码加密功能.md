## 使用

### 执行命令加密数据库密码

```bash
java -cp druid-1.0.16.jar com.alibaba.druid.filter.config.ConfigTools you_password
```

输出

```text
privateKey:MIIBVgIBADANBgkqhkiG9w0BAQEFAASCAUAwggE8AgEAAkEA6+4avFnQKP+O7bu5YnxWoOZjv3no4aFV558HTPDoXs6EGD0HP7RzzhGPOKmpLQ1BbA5viSht+aDdaxXp6SvtMQIDAQABAkAeQt4fBo4SlCTrDUcMANLDtIlax/I87oqsONOg5M2JS0jNSbZuAXDv7/YEGEtMKuIESBZh7pvVG8FV531/fyOZAiEA+POkE+QwVbUfGyeugR6IGvnt4yeOwkC3bUoATScsN98CIQDynBXC8YngDNwZ62QPX+ONpqCel6g8NO9VKC+ETaS87wIhAKRouxZL38PqfqV/WlZ5ZGd0YS9gA360IK8zbOmHEkO/AiEAsES3iuvzQNYXFL3x9Tm2GzT1fkSx9wx+12BbJcVD7AECIQCD3Tv9S+AgRhQoNcuaSDNluVrL/B/wOmJRLqaOVJLQGg==
publicKey:MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAOvuGrxZ0Cj/ju27uWJ8VqDmY7956OGhVeefB0zw6F7OhBg9Bz+0c84RjzipqS0NQWwOb4kobfmg3WsV6ekr7TECAwEAAQ==
password:PNak4Yui0+2Ft6JSoKBsgNPl+A033rdLhFw+L0np1o+HDRrCo9VkCuiiXviEMYwUgpHZUFxb2FpE0YmSguuRww==
```

获得了私钥、公钥、以及用私钥加密后的密码。

### 配置数据源，提示Druid数据源需要对数据库密码进行解密

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
     init-method="init" destroy-method="close">
     <property name="url" value="jdbc:derby:memory:spring-test;create=true" />
     <property name="username" value="sa" />
     <property name="password" value="${password}" />
     <property name="filters" value="config" />
     <property name="connectionProperties" value="config.decrypt=true;config.decrypt.key=${publickey}" />
</bean>
```

## 加密实现

加密的入口在com.alibaba.druid.filter.config.ConfigTools的main方法：

```java
public static void main(String[] args) throws Exception {
       // 明文密码
       String password = args[0];
       // 生成密钥对
       String[] arr = genKeyPair(512);
       System.out.println("privateKey:" + arr[0]);
       System.out.println("publicKey:" + arr[1]);
       // 用私钥加密明文密码
       System.out.println("password:" + encrypt(arr[0], password));
}
```

### 密钥对的生成

```java
public static String[] genKeyPair(int keySize)
      throws NoSuchAlgorithmException, NoSuchProviderException {
   // 生成密钥对字节数组
   byte[][] keyPairBytes = genKeyPairBytes(keySize);
   String[] keyPairs = new String[2];
   
   // 用Bas64编码密钥对
   keyPairs[0] = Base64.byteArrayToBase64(keyPairBytes[0]);
   keyPairs[1] = Base64.byteArrayToBase64(keyPairBytes[1]);

   return keyPairs;
}
```

genKeyPairBytes：

```java
public static byte[][] genKeyPairBytes(int keySize)
      throws NoSuchAlgorithmException, NoSuchProviderException {
   byte[][] keyPairBytes = new byte[2][];
   // 用的JDK自带的安全工具
   // 密钥对生成器
   KeyPairGenerator gen = KeyPairGenerator.getInstance("RSA", "SunRsaSign");
   // 用keySize，随机数初始化密钥对生成器
   gen.initialize(keySize, new SecureRandom());
   // 生成密钥对
   KeyPair pair = gen.generateKeyPair();

   keyPairBytes[0] = pair.getPrivate().getEncoded();
   keyPairBytes[1] = pair.getPublic().getEncoded();

   return keyPairBytes;
}
```

至此，私钥和公钥就生成了。接下来看下加密过程：

### 加密

```java
public static String encrypt(String key, String plainText) throws Exception {
   // 如果私钥为空，使用默认的私钥
   if (key == null) {
      key = DEFAULT_PRIVATE_KEY_STRING;
   }

   byte[] keyBytes = Base64.base64ToByteArray(key);
   return encrypt(keyBytes, plainText);
}


public static String encrypt(byte[] keyBytes, String plainText)
      throws Exception {
   PKCS8EncodedKeySpec spec = new PKCS8EncodedKeySpec(keyBytes);
   KeyFactory factory = KeyFactory.getInstance("RSA", "SunRsaSign");
   PrivateKey privateKey = factory.generatePrivate(spec);
   // 使用JDK自带的加解密工具类Cipher加密
   Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
       try {
       //初始化指定MODE和privateKey
       cipher.init(Cipher.ENCRYPT_MODE, privateKey);
       } catch (InvalidKeyException e) {
           //For IBM JDK, 原因请看解密方法中的说明
           RSAPrivateKey rsaPrivateKey = (RSAPrivateKey) privateKey;
           RSAPublicKeySpec publicKeySpec = new RSAPublicKeySpec(rsaPrivateKey.getModulus(), rsaPrivateKey.getPrivateExponent());
           Key fakePublicKey = KeyFactory.getInstance("RSA").generatePublic(publicKeySpec);
           cipher = Cipher.getInstance("RSA");
           cipher.init(Cipher.ENCRYPT_MODE, fakePublicKey);
       }
   // 加密
   byte[] encryptedBytes = cipher.doFinal(plainText.getBytes("UTF-8"));
   String encryptedString = Base64.byteArrayToBase64(encryptedBytes);

   return encryptedString;
}
```

加密密码的过程就完成了。下面来看一下ConfigFilter怎么解密的。

## 解密实现

解密的入口在DruidAbstractDataSource的初始化方法init：

```java
public void init() throws SQLException {
        // 省略...
        for (Filter filter : filters) {
            // 调用filter的init方法
            filter.init(this);
        }
        // 省略...
}
```

看下ConfigFilter的init方法：

```java
public void init(DataSourceProxy dataSourceProxy) {
    if (!(dataSourceProxy instanceof DruidDataSource)) {
        LOG.error("ConfigLoader only support DruidDataSource");
    }

    DruidDataSource dataSource = (DruidDataSource) dataSourceProxy;
    Properties connectionProperties = dataSource.getConnectProperties();
    // 从本地或者网络加载配置，由config.file配置
    Properties configFileProperties = loadPropertyFromConfigFile(connectionProperties);

    // 判断是否需要解密，如果需要就进行解密行动
    boolean decrypt = isDecrypt(connectionProperties, configFileProperties);

    if (configFileProperties == null) {
        if (decrypt) {
            //解密
            decrypt(dataSource, null);
        }
        return;
    }

    if (decrypt) {
        decrypt(dataSource, configFileProperties);
    }

    try {
        DruidDataSourceFactory.config(dataSource, configFileProperties);
    } catch (SQLException e) {
        throw new IllegalArgumentException("Config DataSource error.", e);
    }
}
```

解密的源码：

```java
public void decrypt(DruidDataSource dataSource, Properties info) {

    try {
        // 从配置文件获取加密后的密码
        String encryptedPassword = null;
        if (info != null) {
            encryptedPassword = info.getProperty(DruidDataSourceFactory.PROP_PASSWORD);
        }

        if (encryptedPassword == null || encryptedPassword.length() == 0) {
            encryptedPassword = dataSource.getConnectProperties().getProperty(DruidDataSourceFactory.PROP_PASSWORD);
        }

        if (encryptedPassword == null || encryptedPassword.length() == 0) {
            encryptedPassword = dataSource.getPassword();
        }
        
        // 从配置文件或JVM启动参数中获取公钥
        PublicKey publicKey = getPublicKey(dataSource.getConnectProperties(), info);
        // 获取明文密码
        String passwordPlainText = ConfigTools.decrypt(publicKey, encryptedPassword);

        // 用明文密码覆盖原来的password
        if (info != null) {
            info.setProperty(DruidDataSourceFactory.PROP_PASSWORD, passwordPlainText);
        } else {
            dataSource.setPassword(passwordPlainText);
        }
    } catch (Exception e) {
        throw new IllegalArgumentException("Failed to decrypt.", e);
    }
}
```

ConfigTools.decrypt：

```java
public static String decrypt(PublicKey publicKey, String cipherText)
      throws Exception {
   // 还是用Cipher解密
   Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
   try {
      // 初始化模式为解密
      cipher.init(Cipher.DECRYPT_MODE, publicKey);
   } catch (InvalidKeyException e) {
           // 因为 IBM JDK 不支持私钥加密, 公钥解密, 所以要反转公私钥
           // 也就是说对于解密, 可以通过公钥的参数伪造一个私钥对象欺骗 IBM JDK
           RSAPublicKey rsaPublicKey = (RSAPublicKey) publicKey;
           RSAPrivateKeySpec spec = new RSAPrivateKeySpec(rsaPublicKey.getModulus(), rsaPublicKey.getPublicExponent());
           Key fakePrivateKey = KeyFactory.getInstance("RSA").generatePrivate(spec);
           cipher = Cipher.getInstance("RSA"); //It is a stateful object. so we need to get new one.
           cipher.init(Cipher.DECRYPT_MODE, fakePrivateKey);
   }
   
   if (cipherText == null || cipherText.length() == 0) {
      return cipherText;
   }

   byte[] cipherBytes = Base64.base64ToByteArray(cipherText);
   // 获得明文密码
   byte[] plainBytes = cipher.doFinal(cipherBytes);

   return new String(plainBytes);
}
```

## 总结

Druid加密数据库密码的流程是，先用ConfigTools加密密码，并生成公钥和私钥，并将公钥配置到配置文件；至于解密发生在数据源初始化的时候，会调用ConfigFilter的init方法，用公钥解密加密密码，获得明文后，覆盖原来的密文。