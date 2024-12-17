## Getting Start

Argon2 is a hash generator optimized to produce hashes suitable for credential storage, key derivation, or other situations requiring a cryptographically secure password hash. Argon2 was the winner of the 2015 <a target="_blank" href="https://www.password-hashing.net/">Password Hashing Competition.</a>

---

## Installation

```
dotnet add package Isopoh.Cryptography.Argon2
```

or

```
Install-Package Isopoh.Cryptography.Argon2
```

---

```csharp
using System.Text;
using System.Security.Cryptography;
using Isopoh.Cryptography.Argon2;
using Isopoh.Cryptography.SecureArray;
```

#### Convert Password to byte array

```csharp
var password = "password1";
byte[] passwordBytes = Encoding.UTF8.GetBytes(password);
```

#### Generate Salt

```csharp
byte[] salt = new byte[16];
var Rng = RandomNumberGenerator.Create();
Rng.GetBytes(salt); // fills salt with a strong random sequence
```

#### Create Argon2 Configuration

```csharp
int Threads = Environment.ProcessorCount; // use full threads

var config = new Argon2Config
{
    Type = Argon2Type.HybridAddressing,
    Version = Argon2Version.Nineteen,
    TimeCost = 10,
    MemoryCost = 32 * 1024, // 32MB
    Lanes = 5,
    Threads = Threads, // higher than "Lanes" doesn't help (or hurt)
    Password = passwordBytes,
    Salt = salt, // >= 8 bytes if not null
    Secret = Encoding.UTF8.GetBytes("your secret key"), // from somewhere
    HashLength = 20 // >= 4
};
var argon2A = new Argon2(config);
```

#### Start hashing Password

```csharp
string hashString;
using(SecureArray<byte> hashA = argon2A.Hash())
{
    hashString = config.EncodeString(hashA.Buffer);
}
Console.WriteLine(hashString);
```

#### Compare/Verify Password

```csharp
int Threads = 5;
var pwdConfig = new Argon2Config
{
    Password = Encoding.UTF8.GetBytes("password"),
    Threads = Threads,
    Secret = Encoding.UTF8.GetBytes("your secret key"),
};

SecureArray<byte>? hash = null;
bool isHashed = pwdConfig.DecodeString("hashed password", out hash);
if (isHashed && hash != null)
{
    var argon2ToVerify = new Argon2(pwdConfig);
    using (var hashVerify = argon2ToVerify.Hash())
    {
        if (Argon2.FixedTimeEquals(hash, hashVerify))
        {
            // verified
            // do something
        }
    }

    hash.Dispose();
}

```

#### Addition

if you want to use all the avaliable threads

```csharp
int Threads = Environment.ProcessorCount;
```

---

_**See more:**_ <a href="https://www.nuget.org/packages/Isopoh.Cryptography.Argon2" target="_blank">Nutget Package</a> | <a target="_blank" href="https://mheyman.github.io/Isopoh.Cryptography.Argon2/api/Isopoh.Cryptography.Argon2.Argon2Config.html">API Documentation</a>
