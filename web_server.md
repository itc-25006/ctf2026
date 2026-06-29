# Webサーバ（Apache2）構築マニュアル

## 1. 目的

Apache2をインストールし、VirtualHostを利用したWebサーバを構築するとともに、ベーシック認証によるアクセス制限を設定する。

---

## 2. システム更新

```bash
sudo apt update
sudo apt upgrade
```

---

## 3. Apache2のインストール

Apache2をインストールする。

```bash
sudo apt install apache2
```

サービスを起動し、有効化する。

```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```

サービスの状態を確認する。

```bash
sudo systemctl status apache2
```

---

## 4. 公開ディレクトリの作成

公開ディレクトリを作成する。

```bash
sudo mkdir -p /var/www/sample
```

テストページを作成する。

```bash
echo "<h1>Web Server Test</h1>" | sudo tee /var/www/sample/index.html
```

所有者とアクセス権を設定する。

```bash
sudo chown -R www-data:www-data /var/www/sample
sudo chmod -R 755 /var/www/sample
```

---

## 5. VirtualHostの設定

設定ファイルを作成する。

```bash
sudo vi /etc/apache2/sites-available/sample.conf
```

設定内容

```apache
<VirtualHost *:80>
    ServerName sample.local

    DocumentRoot /var/www/sample

    <Directory /var/www/sample>
        AllowOverride AuthConfig
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/sample_error.log
    CustomLog ${APACHE_LOG_DIR}/sample_access.log combined
</VirtualHost>
```

デフォルトサイトを無効化し、作成したサイトを有効化する。

```bash
sudo a2dissite 000-default.conf
sudo a2ensite sample.conf
```

設定ファイルの文法を確認する。

```bash
sudo apache2ctl configtest
```

正常であれば

```text
Syntax OK
```

と表示される。

Apacheを再読み込みする。

```bash
sudo systemctl reload apache2
```

---

## 6. ベーシック認証の設定

### apache2-utilsのインストール

### パスワードファイルの作成

```bash
sudo htpasswd -c /etc/apache2/.htpasswd ubuntu
```

### .htaccessの作成

```bash
sudo vi /var/www/sample/.htaccess
```

内容

```apache
AuthType Basic
AuthName "Private Area"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```

Apacheを再起動する。

```bash
sudo systemctl restart apache2
```
---

### 7. 動作確認

ブラウザから

```text
http://サーバIPアドレス
```

または

```text
http://sample.local
```

へアクセスする。

以下を確認する。

1. ベーシック認証画面が表示される。
2. ユーザー名 `user1` と設定したパスワードを入力する。
3. 次の画面が表示される。

```text
Web Server Test
```

---

## 11. 完成

以上で、Apache2を利用したVirtualHost対応Webサーバおよびベーシック認証の設定は完了である。

