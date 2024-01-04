---
title: "WindowsでAstroの環境を作る"
emoji: "😸"
type: "tech"
topics: []
published: false
---

## 現状の課題

Astroフレームワークでウェブ開発をしているのですが、時々フリーズします。一度フリーズすると再起動するまで頻発します。とりあえずWSLが悪だと断定しました。Unixで環境構築するのは非常に楽ではありますが、開発中にフリーズするのは鬱陶しいです。なので仕方なくWindows上で開発することにしました。WindowsのパッケージマネージャーはchocolateyやScoopがありますが、私はWingetを使います。
- [WingetUI](https://github.com/marticliment/WingetUI)  
公式では無いですが、フロントエンドもあります。お試しあれ。  

まず、AstroはNode.jsで動くので、Node.jsのバージョンマネージャーが欲しいです。Windowsでは[NVM for Windows](https://github.com/coreybutler/nvm-windows)などが主流だと思いますが、私は[fnm](https://github.com/Schniz/fnm)を使います。

wingetでfnmをインストールしようと思っていたのですが、上手くいかなかったのでcargoを使うことにしました。

## Visual Studio

Rustのコンパイル時にVisual Studioが必要になります。2017以上が必要となっているので、現時点で最新の2022版をインストールします。

| a                            | a                                     |         |         |
| ---------------------------- | ------------------------------------- | ------- | ------- |
| Visual Studio Community 2022 | XPDCFJDKLZJLP8                        | Unknown | msstore |
| Visual Studio Community 2022 | Microsoft.VisualStudio.2022.Community | 17.8.3  | winget  |

`winget search "Visual Studio"`と検索すると、２つ同じ項目がありますが、私は`Microsoft.VisualStudio.2022.Community`の方をインストールしました。明確な違いがは分かりませんが、恐らく上がMicrosoft Store版で、下がWinget版だと思います。

Visual Studioをインストールする際にVisual Studio Installerが立ち上がると思うのですが、自動で閉じられた場合はもう一度開きます。そしてVisual Studio Community 2022がインストール済みとして表示されていると思いますが、右の変更をクリックします。
ワークロードのブラウジング画面が開くと思うのですが、「デスクトップとモバイル」 -> 「C++によるデスクトップ開発」を選択し、下の「ダウンロードしながらインストール」をクリックします。そうすると、インストールが始まるはずです。

## Rustup & Cargo

| a                                    | b               | c      |                            |
| ------------------------------------ | --------------- | ------ | -------------------------- |
| Rustup: the Rust toolchain installer | Rustlang.Rustup | 1.26.0 | ProductCode: rustup winget |

`winget search "rustup"`と一応確認はしましたが、１つしかないようです。rustupのインストールはVisual Studioより先にインストールしてもエラーは出ませんが、ここではVisual Studioを先にインストールすることを推奨します。また、ビルドツールの設定が上手くいかない場合があるみたいです。

Windows10+Rustupで環境構築
https://qiita.com/sunasaji/items/bdf6d6230820d431dbf7

Rust で `link.exe` not found エラーが出た場合
https://hirake.link/rust-%E3%81%A7-link-exe-not-found-%E3%82%A8%E3%83%A9%E3%83%BC%E3%81%8C%E5%87%BA%E3%81%9F%E5%A0%B4%E5%90%88/

## fnm

```sh
cargo install fnm
```

こうして、fnmをインストールできます。

fnmはコードネームでのインストールが可能です。コードネームについては以下をご参照ください。
https://nodejs.org/en/about/previous-releases
https://github.com/Schniz/fnm/blob/master/docs/commands.md

```
fnm install lts/hydrogen
fnm use lts/hydrogen
```


## pnpm

私はパッケージマネージャーにpnpmを使っています。こちらもwingetでインストールします。

```sh
winget install pnpm
```

## Git

リポジトリのクローンにGitが必要になりますが、これもwingetでインストールできます。

```sh
winget install git
```

## SSH

私はGitHubとの接続をSSHで行っているため、設定をします

https://github.com/settings/emails

Primary Addressの項目の説明の中で
12345678+github@users.noreply.github.com
のようなメールアドレスがあります。これを使うのが一般的です。

SSHのキーを作成します。RSAもありますが、ed25519を使用します

ssh-keygen -t ed25519 -C "12345678+github@users.noreply.github.com"

メールアドレスには先ほど確認したものを使います。

```config
Host github github.com
  HostName github.com
  User git
  IdentityFile C:\Users\taiko\.ssh\github
  IgnoreUnknown UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
```

```sh
> mkdir .ssh
> cd .ssh
> ssh-keygen -t ed25519 -C "12345678+github@users.noreply.github.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\user/.ssh/id_ed25519): C:\Users\user/.ssh/github
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\user/.ssh/github
Your public key has been saved in C:\Users\user/.ssh/github.pub
The key fingerprint is:
SHA256:  12345678+github@users.noreply.github.com
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
> cat github.pub
ssh-ed25519 key 12345678+github@users.noreply.github.com
```

## GNUPGのインストール

```sh
> winget search gnupg
名前              ID            バージョン ソース
--------------------------------------------------
GNU Privacy Guard GnuPG.GnuPG   2.4.3      winget
Gpg4win           GnuPG.Gpg4win 4.2.0      winget

> winget install GnuPG.GnuPG
```

## GPG鍵の作成

```sh
> gpg --full-generate-key
gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

ご希望の鍵の種類を選択してください:
   (1) RSA と RSA
   (2) DSA と Elgamal
   (3) DSA (署名のみ)
   (4) RSA (署名のみ)
   (9) ECC (署名と暗号化) *デフォルト
  (10) ECC (署名のみ)
  (14) カードに存在する鍵
あなたの選択は? 10

ご希望の楕円曲線を選択してください:
   (1) Curve 25519 *デフォルト
   (4) NIST P-384
   (6) Brainpool P-256
あなたの選択は? 1

鍵の有効期限を指定してください。
         0 = 鍵は無期限
      <n>  = 鍵は n 日間で期限切れ
      <n>w = 鍵は n 週間で期限切れ
      <n>m = 鍵は n か月間で期限切れ
      <n>y = 鍵は n 年間で期限切れ
鍵の有効期間は? (0)0
鍵は無期限です

これで正しいですか? (y/N) y

GnuPGはあなたの鍵を識別するためにユーザIDを構成する必要があります。

本名: Your name
電子メール・アドレス: 12345678+github@users.noreply.github.com
コメント:
次のユーザIDを選択しました:
    "Your name <12345678+github@users.noreply.github.com>"

名前(N)、コメント(C)、電子メール(E)の変更、またはOK(O)か終了(Q)? O

たくさんのランダム・バイトの生成が必要です。キーボードを打つ、マウスを動か
す、ディスクにアクセスするなどの他の操作を素数生成の間に行うことで、乱数生
成器に十分なエントロピーを供給する機会を与えることができます。
gpg: C:\\Users\\user\\AppData\\Roaming\\gnupg\\trustdb.gpg: 信用データベースができました
gpg: ディレクトリ'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d'が作成されました
gpg: 失効証明書を 'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\thisisREVOKE.rev' に保管しました。
公開鍵と秘密鍵を作成し、署名しました。

pub   ed25519 2024-01-04 [SC]
      LONGLONGLONG
uid                      Your name <12345678+github@users.noreply.github.com>

>

PS C:\Users\taiko\w>  gpg --list-secret-keys --keyid-format=long
[keyboxd]
---------
sec   ed25519/ABCDEFG 2024-01-04 [SC]
      LONGLONGLONG
uid                 [  究極  ] Your name <12345678+github@users.noreply.github.com>

```

## Subkeyの作成

```sh
PS C:\Users\taiko> gpg --edit-key ABCDEFG
gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

秘密鍵が利用できます。

sec  ed25519/ABCDEFG
     作成: 2024-01-04  有効期限: 無期限      利用法: SC
     信用: 究極        有効性: 究極
[  究極  ] (1). Your name <12345678+github@users.noreply.github.com>

gpg> addkey

ご希望の鍵の種類を選択してください:
   (3) DSA (署名のみ)
   (4) RSA (署名のみ)
   (5) Elgamal (暗号化のみ)
   (6) RSA (暗号化のみ)
  (10) ECC (署名のみ)
  (12) ECC (暗号化のみ)
  (14) カードに存在する鍵
あなたの選択は? 10

ご希望の楕円曲線を選択してください:
   (1) Curve 25519 *デフォルト
   (4) NIST P-384
   (6) Brainpool P-256
あなたの選択は? 1

鍵の有効期限を指定してください。
         0 = 鍵は無期限
      <n>  = 鍵は n 日間で期限切れ
      <n>w = 鍵は n 週間で期限切れ
      <n>m = 鍵は n か月間で期限切れ
      <n>y = 鍵は n 年間で期限切れ
鍵の有効期間は? (0)0
鍵は無期限です

これで正しいですか? (y/N) y
本当に作成しますか? (y/N) y

たくさんのランダム・バイトの生成が必要です。キーボードを打つ、マウスを動か
す、ディスクにアクセスするなどの他の操作を素数生成の間に行うことで、乱数生
成器に十分なエントロピーを供給する機会を与えることができます。

sec  ed25519/ABCDEFG
     作成: 2024-01-04  有効期限: 無期限      利用法: SC
     信用: 究極        有効性: 究極
ssb  ed25519/HIJKLMN
     作成: 2024-01-04  有効期限: 無期限      利用法: S
[  究極  ] (1). Your name <12345678+github@users.noreply.github.com>

gpg> save

```

## Gitconfig  

```sh
> git config --global user.signingkey HIJKLMN
> git config --global commit.gpgsign true
> git config --global user.name "Your Name"
> git config --global user.email "12345678+github@users.noreply.github.com"
> git config --global gpg.program "C:\Program Files (x86)\gnupg\bin\gpg.exe"
```



git clone https://github.com/7rs/pages.git

https://ryoichi0102.hatenablog.com/entry/github-verified-signature-in-commit-list-with-gpg-by-gnupg-windows
https://qiita.com/spiegel-im-spiegel/items/8c60e63e7d00c5805427
https://qiita.com/suthio/items/2760e4cff0e185fe2db9
https://qiita.com/hollyhock0518/items/a3fee20951cd92c87ed9
https://zenn.dev/7rs/articles/a6a7612a2af44f#gpg%E3%81%AE%E7%9B%B8%E9%81%95%E7%82%B9-1