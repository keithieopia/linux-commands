# linux-guides: full disk encryption

<img align="right" alt="Key Icon" src="https://raw.githubusercontent.com/keithieopia/linux-guides/master/assets/key.png">

> secure, yet sane: Linux FDE for mere mortals

When you want to securely setup full disk encryption, yet still have
relatively snappy system performance, without spending a ridiculous time
digesting all the available material on the subject.


## In short:
```console
# cryptsetup --cipher aes-xts-plain64 --hash sha512 --key-size 256 --iter-time 5000 luksFormat /dev/sdX
```

## Rational

### Overview
|                    | default | recommended                         |
| ------------------ | ------- | ----------------------------------- |
| **cipher**         | aes     | aes<sup>[exceptions](#aes-ni)</sup> |
| **mode**           | xts     | xts                                 |
| **hash**           | sha256  | sha512                              |
| **key size**       | 256     | 256                                 |
| **iteration time** | 2000    | 5000                                |


### Cipher: AES
Three main ciphers are supported by LUKS: AES (Rijndael), Twofish, and Serpent.
AES provides an "adequate security margin" while Serpent and Twofish provide a
"high security margin".

Conceptually, it makes sense to select Twofish and Serpent over AES due to
cryptographic strength. However, none of the three have been shown to be broken
or fundamentally flawed and AES has been more heavily scrutinized for
cryptanalytic attacks due to it's standardization and subsequent widespread use.

AES also has a more important feature to consider: performance. Modern processors
have built-in support for AES, which doubles it's native speed and reduces CPU
consumption that would normally be used by the encryption.

Ultimately, ciphers are not the weakest point in FDE; physical security, evil maids,
weak passwords, and [$5 wrenches](https://xkcd.com/538/) are instead. In closing,
AES is specifically used to protect Top Secret classified material in the U.S.
— if it's good enough to protect nuclear secrets, it's good enough for you!


### Mode: XTS
There's a lot of misconceptions on what mode to use for encryption. Fortunately,
there is universal agreement on not to use ECB, thanks in part to
[Wikipedia's ECBed Tux](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Electronic_Codebook_.28ECB.29)

Usually XTS and CBC+ESSIV are the main recommendations. Both protect against
[different threats](http://security.stackexchange.com/questions/64151/xts-vs-aes-cbc-with-essiv-for-file-based-filesystem-encryption#64157)
the other doesn't protect against, but scenario that allows for the attack is
unrealistic in the scope of FDE.

XTS is usually recommended because it generally outperforms CVC+ESSIV under Linux.
Pundits of that recommendation usually point to Thomas Ptacek's article,
"[You Don't Want XTS](https://sockpuppet.org/blog/2014/04/30/you-dont-want-xts/)".

However, the careful reader, will find Ptacek specifically recommends XTS for
FDE, just not for singular file encryption. Ptacek wrote the piece to rebut a
recommendation to use XTS on Dropbox and other cloud-storage providers, which
would allow for traffic analysis and replay attacks which XTS is weak against.

### Hash: SHA512
Using SHA512 over SHA256 (the default), [helps reduce the attack surface](http://security.stackexchange.com/questions/40208/recommended-options-for-luks-cryptsetup)
of brute forcing the hash quickly with a specialized FPGA or ASIC. Similarly,
GPUs are inefficient with 64-bit integer operations, which SHA512 uses. These
attacks are congruous to using a specialized miner or GPU to speed up bitcoin
mining.

Whirlpool is another option, however older versions of libgcrypt used a
[flawed implementation that was later fixed](http://www.saout.de/pipermail/dm-crypt/2014-February/003956.html)
but broke compatibility. Older LUKS encrypted devices using Whirlpool must be
converted and newer LUKS devices are not supported in older versions.

Note that the LUKS specification always uses PBKDF2 by default on the given hash,
contrary to most unofficial documentation and advice. In fact, `cryptsetup` to
fail if a confused user specifies "pbkdf2-sha512" or similar as the hash. There
is no reason to differentiate SHA512 from PBKDF2/SHA512 within the scope of LUKS.

### Key Size: 256
LUKS only supports key sizes of 512 and 256 bits in XTS mode, as the key will be
split in half (a 256-bit key actually becomes 128-bit in use). Due to this, a
recommended 256 key size is notably the most controversial advice given so far.

Regardless, AES operating on 128-bits is still remarkably secure. It has been
mathematically estimated to [take until the year 2040 with all the planet's resources somehow mobilized](http://security.stackexchange.com/questions/6141/amount-of-simple-operations-that-is-safely-out-of-reach-for-all-humanity/6149#6149)
to break AES 128-bit encrypted data. NIST is slightly more conservative and has
certified AES 128-bit protected until the year 2030.

### Iteration Time: 5000
Note `cryptsetup` expects the iteration time to be in milliseconds, the
recommendation of 5000 means to keep hashing until 5 seconds has elapsed. The
actual number of iterations preformed is unique to each machine, depending on
it's specs.

After LUKS has finished, you should confirm the actual iteration count for both
the master key and each key slot is something reasonably secure (above 100,000 is
reasonable).

## When not to use AES
<a name="aes-ni"></a>
Use AES only when your processor supports [AES-NI](https://en.wikipedia.org/wiki/AES_instruction_set).
While most modern PC's support AES-NI, there are notable exceptions:

  * Raspberry Pi
  * Netbooks
  * Intel Adom processors
  * Older ARM processors (prior to 2011 and the ARMv8-A)
  * Old computers (prior to 2008, when AES-NI was invented)

If in question, run the below command to see if your processor supports it. If
nothing is returned, your processor does not support AES-NI:

```console
$ grep "aes" /proc/cpuinfo
```

### What to do if you don't have AES-NI
Should you not have AES-NI, use `serpent-xts-plain64` as Serpent under Linux is
[faster than AES](http://crypto.stackexchange.com/questions/15091/why-is-serpent-faster-than-aes-in-this-benchmark#15092)
when there is no hardware acceleration available.

Serpent also has a "high security margin" compared to AES which has an "adequate
security margin", according to NIST to selected Rijndael (now known as AES) as
the winner.

Twofish is also available for use, which also has a "high security margin".
However, Linux's implementation of Serpent is faster than Twofish. Beware of older
articles stating Serpent is slower than Twofish, as this use to not always be the
case.

**To be clear**: Use AES over Serpent, unless you don't have AES-NI

---

*This is part of the [linux-guides](https://github.com/keithieopia/linux-guides) series. For more information, such see the [README](https://github.com/keithieopia/linux-guides/blob/master/README.md).*
