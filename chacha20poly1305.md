# ChaCha20Poly1305 java 21 native

```
byte[] key = new byte[32];
byte[] plaintext = new byte[1024];
byte[] iv = new byte[12];
SecureRandom.getInstanceStrong().nextBytes(iv);

Cipher cipher = Cipher.getInstance("ChaCha20-Poly1305");
cipher.init(Cipher.ENCRYPT_MODE, new SecretKeySpec(key, "ChaCha20"), new IvParameterSpec(iv), SecureRandom.getInstanceStrong());

byte[] ciphertext = cipher.doFinal(plaintext);
```
