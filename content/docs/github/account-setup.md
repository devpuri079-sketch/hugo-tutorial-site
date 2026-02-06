---
title: GitHubの設定
weight: 3
---
## GitHubの設定
- Googleアカウントでサインイン
- メールアドレスは秘匿アドレスの使用推奨(Keep my email addresses private をOn)
![メール設定](image1.png)


## 認証設定（SSH認証）
Githubにpushするには認証設定が必要なので設定する（SSH認証の手順）



ssh-keyを作成する。以下コマンドをWindowsターミナルなどで実行
``` bash
ssh-keygen -t ed25519 -C "devpuri.079@gmail.com"


```
passフレーズの入力を求められるが空のままでもいい

.pubファイル（キーが記載されているファイル）の作成先が表示されるので確認して<br/>.pubファイルの中身を以下に貼り付けて**Add SSH key**ボタン押下
![SSH設定](image2.png)

Githubアカウントに登録しているメインのアドレスに認証コードメールが届くのでそれを表示されている画面に入力


gitbash等で認証確認 下記コマンドを入力し
パスフレーズ入力、空のままで設定してなければそのままエンター
```bash
ssh -T git@github.com
```
以下のように成功すればOK
```bash
The authenticity of host 'github.com (20.27.177.113)' can't be established.
ED25519 key fingerprint is: SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi devpuri079-sketch! You've successfully authenticated, but GitHub does not provide shell access.

```