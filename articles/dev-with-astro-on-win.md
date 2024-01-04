---
title: "WSLを捨て、\Windows上でAstroの環境構築をする"
emoji: "😩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows", "fnm", "pnpm", "github", "nodejs"]
published: true
---


[astro]: https://astro.build/  
[wsl2]: https://learn.microsoft.com/ja-jp/windows/wsl/compare-versions  
[nodejs]: https://nodejs.org/en  
[nvm-windows]: https://github.com/coreybutler/nvm-windows  
[rust]: https://www.rust-lang.org/ja  
[go]: https://go.dev/  
[fnm]: https://github.com/Schniz/fnm  
[pnpm]: https://pnpm.io/  
[npm]: https://www.npmjs.com/  
[yarn]: https://yarnpkg.com/  
[pnpm-vs-yarn]: https://pnpm.io/ja/benchmarks  
[yarn-pnp]: https://yarnpkg.com/features/pnp  
## 現状の課題

  https://github.com/7rs/pages  

  [WSL2][wsl2]で[Astro]を使ってウェブサイトを作っているのですが、時々フリーズします。一度フリーズすると再起動するまで頻発します。~~とりあえず[WSL][wsl2]が悪だと断定しました。~~ 仕方なくWindows上で開発することに。Unixで環境構築するのは非常に楽なのですが...  
  必要なものですが、[Astro][astro]は[Node.js][nodejs]で動くので、[Node.js][nodejs]の**バージョンマネージャー**が必要です。また、モジュールを管理する**パッケージマネージャー**も必要です。

  ### バージョンマネージャー  
  Windowsでは[NVM for Windows][nvm-windows]などが主流だと思いますが、私は[fnm][fnm]を使います。[fnm][fnm]は[Rust][rust]で書かれているので、高速です。[NVM for Windows][nvm-windows]のパフォーマンスは知りませんが、こちらも[Go][go]で書かれているので高速だと思います。  

  ### パッケージマネージャー  
  [npm][npm]や[yarn][yarn]などがありますが、私は[pnpm][pnpm]を使用します。パフォーマンスだけならば[Yarn PNP][yarn-pnp]が最速のようですが、[Yarn][yarn]と[pnpm][pnpm]では[大きな差は開いてないように見えます][pnpm-vs-yarn]。


[chocolatey]: https://chocolatey.org/  
[scoop]: https://scoop.sh/  
[winget]: https://learn.microsoft.com/ja-jp/windows/package-manager/winget/  
[winget-ui]: https://github.com/marticliment/WingetUI  
[cargo]: https://doc.rust-jp.rs/book-ja/ch01-03-hello-cargo.html  
## [fnm][fnm]のインストールについて  

  ### Windowsのパッケージマネージャー  

  [Chocolatey][chocolatey]や[Scoop][scoop]がありますが、私は[Winget][winget]を使います。非公式の[フロントエンド][winget-ui]もあります。この[WingetUI][winget-ui]は[Chocolatey][chocolatey]と[Scoop][scoop]を含む複数のパッケージマネージャーに対応しています。  

  ### Cargoを使うことにした  

  [winget][winget]で[fnm][fnm]をインストールしようと思いましたが、上手くいかなかったので[cargo][cargo]を使いました。なので、[Rust][rust]の環境を構築します。  


[visual-studio]: https://visualstudio.microsoft.com/ja/  
## [Rust][Rust]のインストール  

  ### [Visual Studio][visual-studio]のインストール  

  [Rust][rust]でコンパイルする際には[Visual Studio][visual-studio]が必要です。要件が2017以上なので、現時点で最新の2022をインストールします。  

  ```sh: PowerShell
  >winget search "Visual Studio"

  名前                           ID                                    バージョン  ソース
  --------------------------------------------------------------------------------------
  Visual Studio Community 2022  XPDCFJDKLZJLP8                        Unknown    msstore
  Visual Studio Community 2022  Microsoft.VisualStudio.2022.Community 17.8.3     winget
  ```  
  検索すると、２つありますが、私は`Microsoft.VisualStudio.2022.Community`の方をインストールしました。明確な違いは不明ですが、恐らく上がMicrosoft Store版で、下がWinget版だと思います。

  そして、[Visual Studio][visual-studio]をインストールする際に**Visual Studio Installer**が立ち上がりますが、自動で閉じられた場合はもう一度開きます。**Visual Studio Community 2022**がインストール済みとして表示されている項目の右端にある「変更」を押下します。
  「**ワークロード**」のブラウズ画面で「**デスクトップとモバイル**」→「**C++によるデスクトップ開発**」を選択し、下の「**ダウンロードしながらインストール**」を押下します。すると、インストールが始まるはずです。  

  ### Rustup (Cargo)のインストール  

  **rustup**は１つしか無いです。[Visual Studio][visual-studio]より先にインストールしてもエラーは出ませんが、ここでは **[Visual Studio][visual-studio]を先にインストールすることを推奨します。** また、ビルドツールの設定が上手くいかない場合があるみたいです。

  - [Windows10+Rustupで環境構築](https://qiita.com/sunasaji/items/bdf6d6230820d431dbf7)  
  - [Rust で `link.exe` not found エラーが出た場合](https://hirake.link/rust-%E3%81%A7-link-exe-not-found-%E3%82%A8%E3%83%A9%E3%83%BC%E3%81%8C%E5%87%BA%E3%81%9F%E5%A0%B4%E5%90%88/)  


[code-name]: https://nodejs.org/en/about/previous-releases  
## [fnm][fnm]のインストール  

  https://github.com/Schniz/fnm  

  ```sh: PowerShell
  cargo install fnm
  ```
  こうして、[fnm][fnm]をインストールできます。  

  ```sh: PowerShell
  fnm install lts/hydrogen
  fnm use lts/hydrogen
  ```  
  [fnm][fnm]は **[コードネーム][code-name]でのインストール**が可能です。  
  また、以下の記事も参考にしてみて下さい。  

  - **Winget**  
    - [Qiita - WindowsでのNode.jsのバージョン管理](https://qiita.com/mkt1234/items/00ffcbc14931819a7dde)  
  - **Chocolatey**  
    - [Qiita - 📗 Node.jsバージョン管理ツール「fnm」のインストール方法と使い方](https://qiita.com/heppokofrontend/items/fe1c3bc41a0ae943c2ca)  
    - [Qiita - fnm (Fast Node Manager) のインストール方法と使い方](https://qiita.com/taqumo/items/b25d486e6ead9f38a13d)  
    - [Zenn - fnm（Fast Node Manager）の導入方法](https://zenn.dev/kazuma_r5/articles/cd5eaf3d8b5b9f)  
  - **Scoop**  
    - [Zenn - Windows での Node.js バージョン管理を fnm で](https://zenn.dev/yokoyamark/articles/ef7ff2d55f5158)  

  以下はコマンドの一覧です。  

  https://github.com/Schniz/fnm/blob/master/docs/commands.md  


## [pnpm][pnpm]のインストール  

  https://github.com/pnpm/pnpm

  ```sh: PowerShell
  winget install pnpm
  ```  
  設定は自動で行われます。**ターミナルを再起動**して設定を適用できます。  

  https://pnpm.io/ja/motivation  


[rsa]: https://ja.wikipedia.org/wiki/RSA%E6%9A%97%E5%8F%B7  
[ed25519]: https://ja.wikipedia.org/wiki/%E3%82%A8%E3%83%89%E3%83%AF%E3%83%BC%E3%82%BA%E6%9B%B2%E7%B7%9A%E3%83%87%E3%82%B8%E3%82%BF%E3%83%AB%E7%BD%B2%E5%90%8D%E3%82%A2%E3%83%AB%E3%82%B4%E3%83%AA%E3%82%BA%E3%83%A0#Ed25519  
## GitとGitHubの設定  

  ### Gitのインストール  

  :::message  
  既にインストールしている場合は不要です。
  :::  

  ```sh: PowerShell
  winget install git
  ```  

  Gitも[Winget][winget]でインストールできます。  

  ### メールアドレスの確認  

  :::message  
  既に設定済みの場合や、リポジトリのクローンをHTTPSで行う場合は不要です。
  :::  

  https://github.com/settings/emails  

  #### メールアドレス確認画面  
  ![emails](/images/articles/show-emails.png)  

  SSH接続にはメールアドレスが必要なのですが、**Primary email address**の説明の中で、`12345678+github@users.noreply.github.com`のようなメールアドレスがあります。これを使うのが一般的です。  

  ### SSH・GitHubの認証の設定  

  ```: ./ssh/config
  Host github github.com
    HostName github.com
    User git
    IdentityFile C:\Users\taiko\.ssh\github
    IgnoreUnknown UseKeychain
    AddKeysToAgent yes
    UseKeychain yes
  ```  
  後から作ってもエラーにはなりません。変更可能です。  `IgnoreUnknown`は`UseKeychain`よりも先に記述してください。`IdentityFile`が間違っているとエラーになります。このパスは後に指定します。  

  ### SSH鍵の作成

  ```sh: PowerShell
  > mkdir .ssh
  > cd .ssh
  # https://github.com/settings/emails で確認したメールアドレスを指定します
  > ssh-keygen -t ed25519 -C "12345678+github@users.noreply.github.com"
  Generating public/private ed25519 key pair.

  # IdentityFileで指定したパスと同じにする必要があります。
  # 何も入力しない場合は表示されているパスが使われます。
  Enter file in which to save the key (C:\Users\user/.ssh/id_ed25519)
  C:\Users\user/.ssh/github

  # 何も入力しない場合はパスフレーズが設定されません。
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
  ```  
  SSHのキーを作成します。[RSA][rsa]もありますが、[ed25519][ed25519]を使用します。  

  ### SSH鍵の登録  

  ```sh: PowerShell
  > cat github.pub
  ssh-ed25519 key 12345678+github@users.noreply.github.com
  ```  
  `cat`コマンドで出力された全文をコピーし、[SSH and GPG keys](https://github.com/settings/keys)の「New SSH Key」を押下し、**Title**には任意の名前、**Key**にはコピーした内容を貼り付けてください。最後に「Add SSH key」を押下してSSHを追加できます。  

  #### 鍵の追加画面  
  ![add-key](/images/articles/add-key.png)  

  #### SSH鍵の登録画面  
  ![add-ssh-key](/images/articles/add-ssh-key.png)  

  GitHub公式がドキュメントを公開しているので、こちらもご参照下さい。  
  https://docs.github.com/ja/authentication/connecting-to-github-with-ssh  

  ### GPG鍵の作成  

  :::message  
  既に設定済みの場合や、署名を求めない場合は不要です。
  :::  

  ```sh: PowerShell
  > winget search gnupg
  名前              ID             バージョン  ソース
  --------------------------------------------------
  GNU Privacy Guard GnuPG.GnuPG   2.4.3      winget
  Gpg4win           GnuPG.Gpg4win 4.2.0      winget
  ```  
  これも２つありますが、これらは異なるものです。`GnuPG.GnuPG`をインストールしてください。  

  ```sh: PowerShell
  > gpg --full-generate-key

  gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
  This is free software: you are free to change and redistribute it.
  There is NO WARRANTY, to the extent permitted by law.

  # 10を入力します
  ご希望の鍵の種類を選択してください:
     (1) RSA と RSA
     (2) DSA と Elgamal
     (3) DSA (署名のみ)
     (4) RSA (署名のみ)
     (9) ECC (署名と暗号化) *デフォルト
    (10) ECC (署名のみ)
    (14) カードに存在する鍵
  あなたの選択は? 10

  # 1を入力します
  ご希望の楕円曲線を選択してください:
     (1) Curve 25519 *デフォルト
     (4) NIST P-384
     (6) Brainpool P-256
  あなたの選択は? 1

  # 0を入力します
  # また、`1w`や`3m`のように期限を設定できます。
  鍵の有効期限を指定してください。
           0 = 鍵は無期限
        <n>  = 鍵は n 日間で期限切れ
        <n>w = 鍵は n 週間で期限切れ
        <n>m = 鍵は n か月間で期限切れ
        <n>y = 鍵は n 年間で期限切れ
  鍵の有効期間は? (0)0
  鍵は無期限です

  # yを入力します
  これで正しいですか? (y/N) y

  # Your nameはあなたの名前です。本名じゃなくても、GitHubのIDじゃなくてもいいです。
  # ただし、のちに設定する
  # `git config --global user.name "Your Name"`の値と一致する必要があります。
  # メールアドレスも同様です
  GnuPGはあなたの鍵を識別するためにユーザIDを構成する必要があります。
  本名: Your name
  電子メール・アドレス: 12345678+github@users.noreply.github.com
  コメント:
  次のユーザIDを選択しました:
      "Your name <12345678+github@users.noreply.github.com>"

  # 間違いやミスがあれば、修正可能です。ない場合はOを入力します。
  名前(N)、コメント(C)、電子メール(E)の変更、またはOK(O)か終了(Q)? O

  # ここで、文字化けしまくってるpinentryのウインドウが表示されますが、
  # とりあえずEnterを入力して、前に進まなかったら左のボタンを押下します。
  たくさんのランダム・バイトの生成が必要です。キーボードを打つ、マウスを動か
  す、ディスクにアクセスするなどの他の操作を素数生成の間に行うことで、乱数生
  成器に十分なエントロピーを供給する機会を与えることができます。
  gpg: C:\\Users\\user\\AppData\\Roaming\\gnupg\\trustdb.gpg: 信用データベースができました
  gpg: ディレクトリ'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d'が作成されました
  gpg: 失効証明書を 'C:\\Users\\user\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\thisisREVOKE.rev' に保管しました。
  公開鍵と秘密鍵を作成し、署名しました。

  pub   ed25519 2024-01-04 [SC]
        LONGLONGLONG
  uid   Your name <12345678+github@users.noreply.github.com>

  ```  
  こちらにも同じく[ed25519][ed25519]を使用します。pinentryが文字化けしている問題はよく分かりません。もしかすると、私の環境でのみ起こる現象かもしれません。文字が読み取れる場合はその指示に従ってください。  

  ```sh: PowerShell
  > gpg --list-secret-keys --keyid-format=long
  [keyboxd]
  ---------
  sec   ed25519/ABCDEFG 2024-01-04 [SC]
        LONGLONGLONG
  uid   [  究極  ] Your name <12345678+github@users.noreply.github.com>
  ```  

  ここで、`ABCDEFG`の場所に表示されている鍵の識別子をコピーしておいてください。  

  ### subkeyの作成  

  ```sh: PowerShell
  > gpg --edit-key ABCDEFG

  gpg (GnuPG) 2.4.3; Copyright (C) 2023 g10 Code GmbH
  This is free software: you are free to change and redistribute it.
  There is NO WARRANTY, to the extent permitted by law.
  秘密鍵が利用できます。
  sec  ed25519/ABCDEFG
       作成: 2024-01-04  有効期限: 無期限      利用法: SC
       信用: 究極        有効性: 究極
  [  究極  ] (1). Your name <12345678+github@users.noreply.github.com>

  # ここから対話モードになります
  # addkeyを入力します
  gpg> addkey

  # 10を入力します
  ご希望の鍵の種類を選択してください:
     (3) DSA (署名のみ)
     (4) RSA (署名のみ)
     (5) Elgamal (暗号化のみ)
     (6) RSA (暗号化のみ)
    (10) ECC (署名のみ)
    (12) ECC (暗号化のみ)
    (14) カードに存在する鍵
  あなたの選択は? 10

  # 1を入力します
  ご希望の楕円曲線を選択してください:
     (1) Curve 25519 *デフォルト
     (4) NIST P-384
     (6) Brainpool P-256
  あなたの選択は? 1

  # 0を入力します
  # 先ほどと同じで、任意の期限を設定できます
  鍵の有効期限を指定してください。
           0 = 鍵は無期限
        <n>  = 鍵は n 日間で期限切れ
        <n>w = 鍵は n 週間で期限切れ
        <n>m = 鍵は n か月間で期限切れ
        <n>y = 鍵は n 年間で期限切れ
  鍵の有効期間は? (0)0
  鍵は無期限です

  # yを入力します (この時はなぜか2回)
  これで正しいですか? (y/N) y
  本当に作成しますか? (y/N) y

  # ここでもpinentryがでるかもしれませんが、先ほどと同じです
  たくさんのランダム・バイトの生成が必要です。キーボードを打つ、マウスを動か
  す、ディスクにアクセスするなどの他の操作を素数生成の間に行うことで、乱数生
  成器に十分なエントロピーを供給する機会を与えることができます。
  sec  ed25519/ABCDEFG
       作成: 2024-01-04  有効期限: 無期限      利用法: SC
       信用: 究極        有効性: 究極
  ssb  ed25519/HIJKLMN
       作成: 2024-01-04  有効期限: 無期限      利用法: S
  [  究極  ] (1). Your name <12345678+github@users.noreply.github.com>

  # saveを入力します
  gpg> save
  ```  
  `HIJKLMN`の場所にある鍵の識別子をコピーしておいてください。  

  ### Git・GPG鍵の設定  

  ```sh: PowerShell
  > git config --global user.signingkey HIJKLMN
  > git config --global commit.gpgsign true
  > git config --global user.name "Your Name"
  > git config --global user.email "12345678+github@users.noreply.github.com"
  # 以下はWingetでインストールした場合のパスなので、絶対とは限りません
  > git config --global gpg.program "C:\Program Files (x86)\gnupg\bin\gpg.exe"
  ```  

  一応、subkeyを作成しなくても`HIJKLMN`の部分を先ほどの`ABCDEFG`に置き換えることで署名できます。  

  ### GPG鍵の登録  

  ```sh: PowerShell
  gpg --armor --export HIJKLMN
  ```  
  HIJKLMNの識別子は`git config --global user.signingkey`で指定した鍵と同じ鍵の識別子を指定します。出力された全文をコピーし、[SSH and GPG keys](https://github.com/settings/keys)の「New GPG Key」を押下し、**Title**には任意の名前、**Key**にはコピーした内容を貼り付けてください。最後に「Add GPG key」を押下してGPG鍵を追加できます。  

  #### 鍵の追加画面  
  ![add-key](/images/articles/add-key.png)  

  #### GPG鍵の登録画面  
  ![add-gpg-key](/images/articles/add-gpg-key.png)  

  こちらもGitHub公式がドキュメントを公開しているので、こちらもご参照下さい。  
  https://docs.github.com/ja/authentication/managing-commit-signature-verification/generating-a-new-gpg-key  


## お疲れ様でした  

  以上でセットアップは完了となります。  

  ```sh: PowerShell
  cd your-project
  pnpm install
  git pull origin idk
  ```  
  mainかmasterリポジトリの変更が取得できないことがあるので、`pull`して取得します。  

  ```sh: PowerShell
  pnpm run dev
  ```  
  あとは同じように開発できるはずです。  
