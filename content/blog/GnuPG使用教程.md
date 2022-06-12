---
title: GnuPG使用教程
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-05 20:55:25
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [Gnu PG基本操作](#org99fae5b)
    1.  [生成公/私钥](#org93134f5)
    2.  [更改密钥密码](#org1f0eac3)
    3.  [列出公/私钥](#org727b018)
    4.  [导出公/私钥](#org3966672)
    5.  [导入公/私钥](#org81de83f)
    6.  [验证公钥](#org76bf1ce)
    7.  [删除公钥/私钥](#orgd8946c4)
    8.  [废除密钥](#orgaa85092)
    9.  [exchange on keyservers](#org46449e8)
2.  [应用-加解密文件](#orgc92c582)
    1.  [公钥方式](#org0c7f3d4)
    2.  [对称密码方法](#org55024ed)
3.  [应用-数字签名](#org101dfb7)
    1.  [私钥创建,公钥验证](#org02dfb61)



<a id="org99fae5b"></a>

# Gnu PG基本操作

`brew install gpg`


<a id="org93134f5"></a>

## 生成公/私钥

    gpg --gen-key
    gpg (GnuPG) 2.3.1; Copyright (C) 2021 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    gpg: directory '/Users/crkmyth1cal/.gnupg' created
    gpg: keybox '/Users/crkmyth1cal/.gnupg/pubring.kbx' created
    Note: Use "gpg --full-generate-key" for a full featured key generation dialog.
    
    GnuPG needs to construct a user ID to identify your key.
    
    Real name: crkmyth1cal
    Email address: crkmyth1cal@protonmail.com
    You selected this USER-ID:
        "crkmyth1cal <crkmyth1cal@protonmail.com>"
    
    Change (N)ame, (E)mail, or (O)kay/(Q)uit? O
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    gpg: /Users/crkmyth1cal/.gnupg/trustdb.gpg: trustdb created
    gpg: key AA72032823A2ECCA marked as ultimately trusted
    gpg: directory '/Users/crkmyth1cal/.gnupg/openpgp-revocs.d' created
    gpg: revocation certificate stored as '/Users/crkmyth1cal/.gnupg/openpgp-revocs.d/CDD514503006241E57352861AA72032823A2ECCA.rev'
    public and secret key created and signed.
    
    pub   ed25519 2021-06-19 [SC] [expires: 2023-06-19]
          CDD514503006241E57352861AA72032823A2ECCA
    uid                      crkmyth1cal <crkmyth1cal@protonmail.com>
    sub   cv25519 2021-06-19 [E] [expires: 2023-06-19]


<a id="org1f0eac3"></a>

## 更改密钥密码

    gpg --change-passphrase crkmyth1cal
    gpg (GnuPG) 2.3.1; Copyright (C) 2021 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.


<a id="org727b018"></a>

## 列出公/私钥

    gpg --list-keys  # or -k
    /Users/crkmyth1cal/.gnupg/pubring.kbx
    -------------------------------------
    pub   ed25519 2021-06-19 [SC] [expires: 2023-06-19]
          CDD514503006241E57352861AA72032823A2ECCA
    uid           [ultimate] crkmyth1cal <crkmyth1cal@protonmail.com>
    sub   cv25519 2021-06-19 [E] [expires: 2023-06-19]
    
    gpg --list-secret-key # or -K
    /Users/crkmyth1cal/.gnupg/pubring.kbx
    -------------------------------------
    sec   ed25519 2021-06-19 [SC] [expires: 2023-06-19]
          CDD514503006241E57352861AA72032823A2ECCA
    uid           [ultimate] crkmyth1cal <crkmyth1cal@protonmail.com>
    ssb   cv25519 2021-06-19 [E] [expires: 2023-06-19]


<a id="org3966672"></a>

## 导出公/私钥

    gpg -a -o crkmyth1cal.gpg  --export crkmyth1cal@protonmail.com
    -----BEGIN PGP PUBLIC KEY BLOCK-----
    
    mDMEYM1ptRYJKwYBBAHaRw8BAQdAaHUGQrDArp3fZYv3MxlT6yxP9/CW5wAgdP7n
    Lfqz4MO0KGNya215dGgxY2FsIDxjcmtteXRoMWNhbEBwcm90b25tYWlsLmNvbT6I
    mgQTFgoAQhYhBM3VFFAwBiQeVzUoYapyAygjouzKBQJgzWm1AhsDBQkDwmcABQsJ
    CAcCAyICAQYVCgkICwIEFgIDAQIeBwIXgAAKCRCqcgMoI6LsynziAQCa+VwORih0
    H2Ycnx8vaM4hwr1iyySb3Bb5o0caHdeu3QEAzBAaIX6JnVPJnIskca6b+k+iJJsW
    B0xG/GTwp/G5Egi4OARgzWm1EgorBgEEAZdVAQUBAQdAmrrwVy7J5GgqpA9PfB63
    oTVQAKR/w6aARrqQ7VNybWYDAQgHiH4EGBYKACYWIQTN1RRQMAYkHlc1KGGqcgMo
    I6LsygUCYM1ptQIbDAUJA8JnAAAKCRCqcgMoI6LsyiSuAQD2PfSX8REDV34euf9D
    RoBg0uKhLTgQtgm2zeEcpm+0UAD/V4KzFUFrwMqHComhztuWLEWiCZdvRn1n+n4v
    vYFYggQ=
    =3qda
    -----END PGP PUBLIC KEY BLOCK-----
    
    gpg -ao crkmyth1cal@protonmail.com.private.key  --export-secret-keys crkmyth1cal@protonmail.com
    -----BEGIN PGP PRIVATE KEY BLOCK-----
    
    lIYEYM1ptRYJKwYBBAHaRw8BAQdAaHUGQrDArp3fZYv3MxlT6yxP9/CW5wAgdP7n
    Lfqz4MP+BwMClpMTsR0QrB7z+mUYYoXKHAVO6Sx8Qou3jDkh+13GjO8T0BKtvJxp
    4gg+ycTou2JKSF79LJPTVJKMxO+qVL9ZoXNKPMroumiraVtFKp2OOrQoY3JrbXl0
    aDFjYWwgPGNya215dGgxY2FsQHByb3Rvbm1haWwuY29tPoiaBBMWCgBCFiEEzdUU
    UDAGJB5XNShhqnIDKCOi7MoFAmDNabUCGwMFCQPCZwAFCwkIBwIDIgIBBhUKCQgL
    AgQWAgMBAh4HAheAAAoJEKpyAygjouzKfOIBAJr5XA5GKHQfZhyfHy9oziHCvWLL
    JJvcFvmjRxod167dAQDMEBohfomdU8mciyRxrpv6T6IkmxYHTEb8ZPCn8bkSCJyL
    BGDNabUSCisGAQQBl1UBBQEBB0CauvBXLsnkaCqkD098HrehNVAApH/DpoBGupDt
    U3JtZgMBCAf+BwMC3RGXbNzVlWDz4USVVz26VGm7Wo8VSovee7SFJ2YXwFbx0//u
    5luXDYhKK7iTrtjDQqMXk1LOnOdO1HM5nn0uD5N5eTgRKfiDBLZRYRXxaIh+BBgW
    CgAmFiEEzdUUUDAGJB5XNShhqnIDKCOi7MoFAmDNabUCGwwFCQPCZwAACgkQqnID
    KCOi7MokrgEA9j30l/ERA1d+Hrn/Q0aAYNLioS04ELYJts3hHKZvtFAA/1eCsxVB
    a8DKhwqJoc7blixFogmXb0Z9Z/p+L72BWIIE
    =hdmV
    -----END PGP PRIVATE KEY BLOCK-----


<a id="org81de83f"></a>

## 导入公/私钥

    gpg --import crkmyth1cal.gpg
    
    gpg --import crkmyth1cal.private.key
    gpg: key AA72032823A2ECCA: "crkmyth1cal <crkmyth1cal@protonmail.com>" not changed
    gpg: key AA72032823A2ECCA: secret key imported
    gpg: Total number processed: 1
    gpg:              unchanged: 1
    gpg:       secret keys read: 1
    gpg:  secret keys unchanged: 1


<a id="org76bf1ce"></a>

## 验证公钥

    gpg --edit-key crkmyth1cal
    gpg (GnuPG) 2.3.1; Copyright (C) 2021 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    Secret key is available.
    
    sec  ed25519/AA72032823A2ECCA
         created: 2021-06-19  expires: 2023-06-19  usage: SC
         trust: ultimate      validity: ultimate
    ssb  cv25519/603F4ACEF55D04B2
         created: 2021-06-19  expires: 2023-06-19  usage: E
    [ultimate] (1). crkmyth1cal <crkmyth1cal@protonmail.com>
    
    gpg> fpr  # fingerprint
    pub   ed25519/AA72032823A2ECCA 2021-06-19 crkmyth1cal <crkmyth1cal@protonmail.com>
     Primary key fingerprint: CDD5 1450 3006 241E 5735  2861 AA72 0328 23A2 ECCA
    
    gpg> sign
    "crkmyth1cal <crkmyth1cal@protonmail.com>" was already signed by key AA72032823A2ECCA
    Nothing to sign with key AA72032823A2ECCA
    
    gpg> quit


<a id="orgd8946c4"></a>

## 删除公钥/私钥

    gpg --delete-key crkmyth1cal@protonmail.com
    gpg --delete-secret-key crkmyth1cal@protonmail.com


<a id="orgaa85092"></a>

## 废除密钥

    gpg  --output revoke.asc  --gen-revoke crkmyth1cal@protonmail.com                                                                130 ↵
    
    sec  ed25519/AA72032823A2ECCA 2021-06-19 crkmyth1cal <crkmyth1cal@protonmail.com>
    
    Create a revocation certificate for this key? (y/N) y
    Please select the reason for the revocation:
      0 = No reason specified
      1 = Key has been compromised
      2 = Key is superseded
      3 = Key is no longer used
      Q = Cancel
    (Probably you want to select 1 here)
    Your decision? 3
    Enter an optional description; end it with an empty line:
    > I'm not used this key
    >
    Reason for revocation: Key is no longer used
    I'm not used this key
    Is this okay? (y/N) y
    ASCII armored output forced.
    Revocation certificate created.
    
    Please move it to a medium which you can hide away; if Mallory gets
    access to this certificate he can use it to make your key unusable.
    It is smart to print this certificate and store it away, just in case
    your media become unreadable.  But have some caution:  The print system of
    your machine might store the data and make it available to others!
    
    gpg --import revoke.asc
    gpg --send-keys  crkmyth1cal@protonmail.com


<a id="org46449e8"></a>

## exchange on keyservers

    gpg --refresh-keys  # update all keys from a keyserver
    gpg: refreshing 1 key from hkps://hkps.pool.sks-keyservers.net
    gpg: key 32B25331509A6D85: "crkmyth1cal <crkmyth1cal@protonmail.com>" not changed
    gpg: Total number processed: 1
    gpg:              unchanged: 1
    
    # 发送key ID到keyserver
    gpg --send-keys 9001430FA52D253633CB1B8D32B25331509A6D85                                                                           2 ↵
    gpg: sending key 32B25331509A6D85 to hkps://hkps.pool.sks-keyservers.net
    
    gpg --search-keys ethan hunter  # search key and import
    gpg: data source: https://hkps.pool.sks-keyservers.net:443
    (1)	Ethan Hunter <Ethanhunter@cock.li>
    	  2048 bit RSA key E6F7CD04BD975343, created: 2018-05-12, expires: 2020-05-12 (expired)
    Keys 1-1 of 1 for "ethan hunter".  Enter number(s), N)ext, or Q)uit > 1
    gpg: key E6F7CD04BD975343: public key "Ethan Hunter <Ethanhunter@cock.li>" imported
    gpg: Total number processed: 1
    gpg:               imported: 1
    
    # import keys from a keyserver
    gpg --receive-keys B9c0165f
    gpg: key C7BA956CB9C0165F: public key "ethan <askding@bugbank.cn>" imported
    gpg: Total number processed: 1
    gpg:               imported: 1


<a id="orgc92c582"></a>

# 应用-加解密文件


<a id="org0c7f3d4"></a>

## 公钥方式

    # encrypt
    gpg  -ao msg.txt.gpg  -r crkmyth1cal   -e msg.txt  # -r = --recipient ,many recipient use : -r askDing -r crkmyth1cal
    
     -----BEGIN PGP MESSAGE-----
    
    hF4DYD9KzvVdBLISAQdAsW4ri+FuwxVn0pE1/WGl0hsmKL+j5+hWiXRohZFeXGgw
    9PDAlAPE5icl3aKaOH2ZLVPaGLerGjRVZDopJqmAH812IPvlZHtZdRSPvBhb39dr
    1GkBCQIV1TBYqTyxBsdFBrkOmc9dVS2/720cTcwoEISGa6RRAvzj1wIFDaP9yW0g
    iA9TybkoHWaM0Gfa2Zb9d3I2FGfV+wnusWr1zl8HUGxQ+HwxaJi0PvRM1T1LZHla
    +qOU2zzdLNNlXP0=
    =E0w0
    -----END PGP MESSAGE-----
    
    # decrypt
    gpg -o msg.txt -d msg.txt.gpg
    gpg: encrypted with cv25519 key, ID 603F4ACEF55D04B2, created 2021-06-19
          "crkmyth1cal <crkmyth1cal@protonmail.com>"


<a id="org55024ed"></a>

## 对称密码方法

    #encrypt
    gpg -ao msg.txt.symmetric.gpg  -c msg.txt  # -c as --symmetric
    -----BEGIN PGP MESSAGE-----
    
    jA0ECQMCDFhZquLMGzHz0loBJfOjQIg8gbP4LwMYHQ1dJzPmEjwPRR9WcT0OffXq
    Xqjk0ku3bUCXhKcx4FTmapleTSDJUBqHRNmBf94F2cbnSt+JUJZNpyPkY447wDne
    SYkxccKlP67k+Ro=
    =6Q3U
    -----END PGP MESSAGE-----
    
    # decrypt
    gpg -o msg.txt  -d msg.txt.symmetric.gpg
    gpg: AES256.CFB encrypted data
    gpg: encrypted with 1 passphrase


<a id="org101dfb7"></a>

# 应用-数字签名


<a id="org02dfb61"></a>

## 私钥创建,公钥验证

    # sign
    gpg -ao msg.txt.sign -s msg.txt  # -s as --sign
    -----BEGIN PGP MESSAGE-----
    
    owGbwMvMwCVmtCnYMGBWbivjGp0k9tzidL2SipKEs02PSjIyixWAKFEhObE4VSE/
    TSE1L7mosqBEIS0zJ5Wro5SFQYyLQVZMkWUCozP/Ul1VM+PT0r0w01iZQGYwcHEK
    wEQWf2Vk2J26+Yxx+Y4vEWt5l2w8+HDx97CNWmeVDZfnrN7XlPemvp+R4YTTtZ2H
    xGVayz76q6yb1eRmfiuWbW/wHB3fzS0niydJsQIA
    =GoIj
    -----END PGP MESSAGE-----
    
    # verify
     gpg --verify msg.txt.sign
    gpg: Signature made Sat Jun 19 13:38:42 2021 CST
    gpg:                using EDDSA key 9001430FA52D253633CB1B8D32B25331509A6D85
    gpg: Good signature from "crkmyth1cal <crkmyth1cal@protonmail.com>" [ultimate]
    
    # verify and restore
    gpg -o msg.txt -d msg.txt.sign
    gpg: Signature made Sat Jun 19 13:38:42 2021 CST
    gpg:                using EDDSA key 9001430FA52D253633CB1B8D32B25331509A6D85
    gpg: Good signature from "crkmyth1cal <crkmyth1cal@protonmail.com>" [ultimate]

