## Installation

```
dotnet add package Isopoh.Cryptography.Argon2
```

or

```
Install-Package Isopoh.Cryptography.Argon2
```

#### Convert Password to byte array

```csharp
using System.Text;

var password = "password1";
byte[] passwordBytes = Encoding.UTF8.GetBytes(password);
```

#### Generate Salt

```csharp
using System.Security.Cryptography;

byte[] salt = new byte[16];
var Rng = RandomNumberGenerator.Create();
Rng.GetBytes(salt); // fills salt with a strong random sequence
```

#### Create Argon2 Configuration

```csharp
using Isopoh.Cryptography.Argon2;
using Isopoh.Cryptography.SecureArray;

var config = new Argon2Config
{
    Type = Argon2Type.DataIndependentAddressing,
    Version = Argon2Version.Nineteen,
    TimeCost = 10,
    MemoryCost = 32768,
    Lanes = 5,
    Threads = Environment.ProcessorCount, // higher than "Lanes" doesn't help (or hurt)
    Password = passwordBytes,
    Salt = salt, // >= 8 bytes if not null
    Secret = secret, // from somewhere
    AssociatedData = associatedData, // from somewhere
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
if (Argon2.Verify(hashString, passwordBytes, Threads))
{
    // verified
}

```

#### Addition

if you want to use all the avaliable threads

```csharp
int Threads = Environment.ProcessorCount;
```

---

_**See more:**_ <a href="https://www.nuget.org/packages/Isopoh.Cryptography.Argon2" target="_blank">Isopoh.Cryptography.Argon2</a>
