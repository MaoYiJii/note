# Security

## 非對稱式加密

共用 interface
``` cs
public interface IHash
{
    string ComputeHash(object value);
}
```

共用基底
``` cs
/// <summary>
/// 非對稱式加密
/// </summary>
public abstract class HashSecurity : IHash
{
    protected abstract HashAlgorithm CreateAlgorithm();

    protected virtual string ComputeNullResult => null;
    
    protected virtual byte[] ValueToBytes(object value, Type type)
    {
        var serializer = new DataContractSerializer(type);
        using (var memoryStream = new MemoryStream())
        {
            serializer.WriteObject(memoryStream, value);
            return memoryStream.ToArray();
        }
    }
    protected virtual string BytesToString(byte[] bytes)
    {
        return Convert.ToBase64String(bytes);
    }
    protected virtual byte[] ComputeHashCore(byte[] bytes)
    {
        var algorithm = CreateAlgorithm();
        return algorithm.ComputeHash(bytes);
    }
    protected virtual string ComputeHash(object value, Type type)
    {
        return BytesToString(ComputeHashCore(ValueToBytes(value, type)));
    }
    public virtual string ComputeHash(object value)
    {
        if (value == null)
        {
            return ComputeNullResult;
        }
        return ComputeHash(value, value.GetType());
    }
}
```

### SHA1

``` cs
/// <summary>
/// SHA1 雜湊
/// </summary>
public class Sha1Security : HashSecurity
{
    protected override HashAlgorithm CreateAlgorithm() => new SHA1CryptoServiceProvider();
}
```

### MD5

``` cs
/// <summary>
/// MD5
/// </summary>
public class Md5Security : HashSecurity
{
    protected override HashAlgorithm CreateAlgorithm() => MD5.Create();

    protected override byte[] ValueToBytes(object value, Type type)
    {
        if (value is string str)
        {
            return Encoding.ASCII.GetBytes(str);
        }
        return base.ValueToBytes(value, type);
    }
    protected override string BytesToString(byte[] bytes)
    {
        return string.Join("", bytes.Select(x => x.ToString("x2")));
    }
}
```


## 對稱式加密

共用 interface
``` cs
public interface ISecurity
{
    /// <summary>
    /// 加密
    /// </summary>
    string Encrypt(string plaintext);

    /// <summary>
    /// 解密
    /// </summary>
    string Decrypt(string ciphertext);
}
```

共用基底
``` cs
/// <summary>
/// 對稱式加密
/// </summary>
public abstract class SymmetricSecurity : ISecurity
{
    protected abstract SymmetricAlgorithm CreateAlgorithm();
    protected abstract string Key { get; }
    protected abstract string IV { get; }

    protected virtual Encoding Encoding => Encoding.Default;
    protected virtual CipherMode Mode => CipherMode.CBC;
    protected virtual PaddingMode Padding => PaddingMode.PKCS7;

    /// <summary>
    /// 對 null 加密的結果
    /// </summary>
    protected virtual string EncryptNullResult => null;
    /// <summary>
    /// 對 <see cref="String.Empty"/> 加密的結果
    /// </summary>
    protected virtual string EncryptEmptyResult => string.Empty;
    /// <summary>
    /// 對 null 解密的結果
    /// </summary>
    protected virtual string DecryptNullResult => null;
    /// <summary>
    /// 對 <see cref="String.Empty"/> 解密的結果
    /// </summary>
    protected virtual string DecryptEmptyResult => string.Empty;

    /// <summary>
    /// 初始化演算法參數
    /// </summary>
    protected virtual void InitAlgorithm(SymmetricAlgorithm algorithm)
    {
        algorithm.Key = Encoding.GetBytes(Key);
        algorithm.IV = Encoding.GetBytes(IV);
        algorithm.Mode = Mode;
        algorithm.Padding = Padding;
    }

    /// <summary>
    /// 加密
    /// </summary>
    public virtual string Encrypt(string plaintext)
    {
        if (plaintext == null)
        {
            return this.EncryptNullResult;
        }
        if (plaintext == string.Empty)
        {
            return this.EncryptEmptyResult;
        }
        var dataBytes = Encoding.GetBytes(plaintext);
        using (var algorithm = CreateAlgorithm())
        {
            InitAlgorithm(algorithm);
            var encryptor = algorithm.CreateEncryptor();
            var encrypt = encryptor.TransformFinalBlock(dataBytes, 0, dataBytes.Length);
            return Convert.ToBase64String(encrypt);
        }
    }

    /// <summary>
    /// 解密
    /// </summary>
    public virtual string Decrypt(string ciphertext)
    {
        if (ciphertext == null)
        {
            return this.DecryptNullResult;
        }
        if (ciphertext == string.Empty)
        {
            return this.DecryptEmptyResult;
        }
        var dataBytes = Convert.FromBase64String(ciphertext);
        using (var algorithm = CreateAlgorithm())
        {
            InitAlgorithm(algorithm);
            var decryptor = algorithm.CreateDecryptor();
            var decrypt = decryptor.TransformFinalBlock(dataBytes, 0, dataBytes.Length);
            return Encoding.GetString(decrypt);
        }
    }
}
```

### AES

``` cs
/// <summary>
/// AES 加密
/// </summary>
public abstract class AesSecurity : SymmetricSecurity
{
    protected override SymmetricAlgorithm CreateAlgorithm() => Aes.Create();
}
```

### DES

``` cs
/// <summary>
/// DES 加密
/// </summary>
public abstract class DesSecurity : SymmetricSecurity
{
    protected override SymmetricAlgorithm CreateAlgorithm() => DES.Create();
}
```


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