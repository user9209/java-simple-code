# PBKDF2 in java 21 nativ

```
 /**
 * Get a key form a password
 * pbkdf2 params related to OWASP:
 * @see <a href="https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html">
 *     OWASP Password Storage Cheat Sheet</a>
 * @param password string should not be used
 * @param salt extra bytes additional to the password
 * @return key of 256 bit
 */
public static byte[] pbkdf2(String password, byte[] salt) {
    try {
        PBEKeySpec spec = new PBEKeySpec(password.toCharArray(), salt, 210_000, 256);
        SecretKeyFactory secretKeyFactory = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA512");
        return secretKeyFactory.generateSecret(spec).getEncoded();
    } catch (NoSuchAlgorithmException| InvalidKeySpecException e) {
        throw new RuntimeException(e);
    }
}
```
