# Security

## SHA512 編碼

```cs
public string GenerateSHA512String(string inputString)
{
    SHA512 sha512 = SHA512.Create();
    byte[] bytes = Encoding.UTF8.GetBytes(inputString);
    byte[] hash = sha512.ComputeHash(bytes);
    return GetStringFromHash(hash);
}
private string GetStringFromHash(byte[] hash)
{
    StringBuilder result = new StringBuilder();
    for (int i = 0; i < hash.Length; i++)
    {
        result.Append(hash[i].ToString("X2"));
    }
    return result.ToString();
}
```

## AES 加解密
``` cs
public class AesSecurity : ISecurity
{
    protected string Key { get; }
    protected string IV { get; }
    public AesSecurity()
    {
        Key = ConfigurationManager.AppSettings["SecurityKey"];
        IV = ConfigurationManager.AppSettings["SecurityIV"];
    }

    /// <summary>
    /// AES 加密
    /// </summary>
    /// <param name="plaintext"></param>
    /// <returns></returns>
    public virtual string Encrypt(string plaintext)
    {
        var keyBytes = Encoding.UTF8.GetBytes(Key);
        var ivBytes = Encoding.UTF8.GetBytes(IV);
        var dataBytes = Encoding.UTF8.GetBytes(plaintext);
        using (var aes = Aes.Create())
        {
            aes.Key = keyBytes;
            aes.IV = ivBytes;
            aes.Mode = CipherMode.CBC;
            aes.Padding = PaddingMode.PKCS7;
            var encryptor = aes.CreateEncryptor();
            var encrypt = encryptor.TransformFinalBlock(dataBytes, 0, dataBytes.Length);
            return Convert.ToBase64String(encrypt);
        }
    }

    /// <summary>
    /// AES 解密
    /// </summary>
    /// <param name="ciphertext"></param>
    /// <returns></returns>
    public virtual string Decrypt(string ciphertext)
    {
        var keyBytes = Encoding.UTF8.GetBytes(Key);
        var ivBytes = Encoding.UTF8.GetBytes(IV);
        var dataBytes = Convert.FromBase64String(ciphertext);
        using (var aes = Aes.Create())
        {
            aes.Key = keyBytes;
            aes.IV = ivBytes;
            aes.Mode = CipherMode.CBC;
            aes.Padding = PaddingMode.PKCS7;
            var decryptor = aes.CreateDecryptor();
            var decrypt = decryptor.TransformFinalBlock(dataBytes, 0, dataBytes.Length);
            return Encoding.UTF8.GetString(decrypt);
        }
    }
}
```