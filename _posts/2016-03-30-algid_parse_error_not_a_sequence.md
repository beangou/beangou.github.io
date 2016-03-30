---
layout: default
title: java rsa秘钥加密出错：algid parse error, not a sequence
---

1. java rsa秘钥加密出错：algid parse error, not a sequence

	<pre>
<code>
Exception in thread "main" java.security.spec.InvalidKeySpecException: java.security.InvalidKeyException: IOException : algid parse error, not a sequence
at sun.security.rsa.RSAKeyFactory.engineGeneratePrivate(Unknown Source)
at java.security.KeyFactory.generatePrivate(Unknown Source)
at base54.encrypt.RSAToy.main(RSAToy.java:36)
Caused by: java.security.InvalidKeyException: IOException : algid parse error, not a sequence
at sun.security.pkcs.PKCS8Key.decode(Unknown Source)
at sun.security.pkcs.PKCS8Key.decode(Unknown Source)
at sun.security.rsa.RSAPrivateCrtKeyImpl.<init>(Unknown Source)
at sun.security.rsa.RSAPrivateCrtKeyImpl.newKey(Unknown Source)
at sun.security.rsa.RSAKeyFactory.generatePrivate(Unknown Source)
</code>
</pre>

2. 根据以上错误信息，很可能是你的秘钥文件格式问题（PKCS#1这种格式），你可以按照以下方法将秘钥文件改为PKCS#8。
	<pre>
<code>
openssl pkcs8 -topk8 -inform PEM -outform PEM -in 你的秘钥文件.pem -out 输出秘钥文件.pem -nocrypt
</code>
</pre>

* 然后按照之前的方法，读取新的秘钥文件，应该就可以了。