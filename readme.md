# bcrypt.net

Porting of bcrypt.codeplex.com with missing fixes, features and better .net support.

# Nuget 

Download using nuget or Paket (https://fsprojects.github.io/Paket/)

Package: https://www.nuget.org/packages/BCrypt.Net-Next/

## Compiling

You'll need at least VS2015 with the current SDK (check the global.json file for the current version);
You can grab the version required from https://github.com/aspnet/Tooling/blob/master/known-issues.md#missing-sdk 

The nuget packages can be built by running `buildfornuget.cmd`
or 

```
dotnet restore .\src
dotnet pack .\src\BCrypt.Net --configuration Release
```

## Tests

You can run the tests from the main folder by typing `dotnet test .\src\BCrypt.Net.Tests\`
It's ill advised to just run the tests like this as the `TestGenerateSaltWithMaxWorkFactor` takes a long time to complete. This may be altered in later versions to be ignored (manual run).

## Description

A .Net port of jBCrypt implemented in C#. It uses a variant of the Blowfish encryption algorithm’s keying schedule, and introduces a work factor, which allows you to determine how expensive the hash function will be, allowing the algorithm to be "future-proof".

## Details
This is, for all intents and purposes, a direct port of jBCrypt written by Damien Miller.  The main differences are the addition of some convenience methods and some mild re-factoring.  The easiest way to verify BCrypt.Net's parity with jBCrypt is to compare the unit tests.

For an overview of why BCrypt is important see How to Safely Store a Password.  In general it's a hashing algorithm that can be adjusted over time to require more CPU power to generate the hashes.  This, in essence, provides some protection against Moore's Law.  That is, as computers get faster, this algorithm can be adjusted to require more CPU power.  The more CPU power that's required to hash a given password, the more time a "hacker" must invest, per password.  Since the "work factor" is embedded in the resultant hash, the hashes generated by this algorithm are forward/backward-compatible.

## Why BCrypt

### From How to Safely Store a Password:

It uses a variant of the Blowfish encryption algorithm’s keying schedule, and introduces a work factor, which allows you to determine how expensive the hash function will be. Because of this, BCrypt can keep up with Moore’s law. As computers get faster you can increase the work factor and the hash will get slower.

### From Wikipedia:

Niels Provos and David Mazieres designed a crypt() scheme called BCrypt based on Blowfish, and presented it at USENIX in 1999. The printable form of these hashes starts with $2$ or $2a$, depending on which variant of the algorithm is used.

Blowfish is notable among block ciphers for its expensive key set-up phase. It starts off with subkeys in a standard state, then uses this state to perform a block encryption using part of the key, and uses the result of that encryption (really, a hashing) to replace some of the subkeys. Then it uses this modified state to encrypt another part of the key, and uses the result to replace more of the subkeys. It proceeds in this fashion, using a progressively modified state to hash the key and replace bits of state, until all subkeys have been set.

Provos and Mazieres took advantage of this, and actually took it further. They developed a new key set-up algorithm for Blowfish, dubbing the resulting cipher "Eksblowfish" ("expensive key schedule Blowfish"). The key set-up begins with a modified form of the standard Blowfish key set-up, in which both the salt and password are used to set all sub-keys. Then there is a configurable number of rounds in which the standard Blowfish keying algorithm is applied, using alternately the salt and the password as the key, each round starting with the sub key state from the previous round. This is not cryptographically significantly stronger than the standard Blowfish key schedule; it's just very slow.

The number of rounds of keying is a power of two, which is an input to the algorithm. The number is encoded in the textual hash.

# Releases

v2.0.0 -

Fresh release packaged for the majority of .net & containing safe-equals to reduce the risks from timing attacks https://en.wikipedia.org/wiki/Timing_attack
Technically the implementation details of BCrypt theoretically mitigate against a timing attacks, so this is simply implemented in the event that the salt is discovered.

