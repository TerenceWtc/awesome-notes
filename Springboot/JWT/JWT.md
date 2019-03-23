# JSON Web Token (JWT) 

## Introduction
- [Offical introduction](https://jwt.io/introduction/)

## Preparation
- spring boot project with maven
- dependency of `jjwt`
  ```
  <dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.0</version>
  </dependency>
  ```

## Strengthen token's safety
I use the solution of 'asymmetric cryptographic algorithm'(非对称加密算法).  
There's a pair of `public key` and `private key`.  
The privacy string, which encrypted by public key can only decrypt by the private key, and which encrypted by private key can only decrpty by the public key.  
In this case, I generated a pair of keys, gave the public key to others who need, and stored the private key myself.

## Generate the keys
```
public void generateKeys(String publicKeyFilename, String privateKeyFilename, String password) throws IOException, NoSuchAlgorithmException {
        KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        SecureRandom secureRandom = new SecureRandom(password.getBytes());
        keyPairGenerator.initialize(1024, secureRandom);
        KeyPair keyPair = keyPairGenerator.genKeyPair();
        byte[] publicKeyBytes = keyPair.getPublic().getEncoded();
        FileOutputStream fos = new FileOutputStream(publicKeyFilename);
        fos.write(publicKeyBytes);
        fos.close();
        byte[] privateKeyBytes = keyPair.getPrivate().getEncoded();
        fos = new FileOutputStream(privateKeyFilename);
        fos.write(privateKeyBytes);
        fos.close();
    }
```

## JWT
JWT token is consist of a `Header`, a `Payload` and a `Signature`.  

### Header
Header is a JSON object, contains the algorithm and type. 

### Payload
Payload is also a JSON object, contains the data you want to transfer.  
The data should not be privacy, because it can be easily decrypted.

### Signature
Signature prevents the other two parts being tampered.

## Generate JWT
I have a entity with three fileds to inplement the interface.
```
public interface IUserJwtInfo {

    String getId();

    String getUsername();

    String getName();
}
```

```
private static String generation(IUserJwtInfo userIwtInfo, PrivateKey privateKey, int expiration) {
        LocalDateTime localDateTime = LocalDateTime.now().plusSeconds(expiration);
        return Jwts.builder().setSubject(userIwtInfo.getId())
                .claim(JwtConstant.JWT_KEY_USERNAME, userIwtInfo.getUsername())
                .claim(JwtConstant.JWT_KEY_NAME, userIwtInfo.getName())
                .setExpiration(TimeConvertUtil.LocalDateTime2Date(localDateTime))
                .signWith(SignatureAlgorithm.RS256, privateKey).compact();
    }
```
So we can get the `username`, the `id` and the `name` from a token gerenated by the method.  
The `expiration` stands for how long the token will be expired.

---
TODO
- [ ] get info from token
- [ ] jwt exceptions
