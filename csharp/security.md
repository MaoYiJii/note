# Security

### SHA512 編碼

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
