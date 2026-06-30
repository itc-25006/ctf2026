# FTPサーバ（vsftpd）構築マニュアル

## 1. 目的

vsftpdをxinetd経由で動作させるFTPサーバを構築する。

---

## 2. パッケージのインストール

```bash
sudo apt update
sudo apt install vsftpd xinetd
```

---

## 3. vsftpdの設定

設定ファイルを編集する。

```bash
sudo vi /etc/vsftpd.conf
```

以下の設定を確認・変更する。

```ini
listen=NO
listen_ipv6=NO

anonymous_enable=NO
local_enable=YES
write_enable=YES

xferlog_enable=YES

pam_service_name=vsftpd
```

---

## 4. xinetdの設定

```bash
sudo vi /etc/xinetd.d/vsftpd
```

内容

```ini
service ftp
{
    disable = no
    socket_type = stream
    protocol = tcp
    wait = no
    user = root
    server = /usr/sbin/vsftpd
}
```

---

## 5. xinetdで起動

```bash
sudo systemctl stop vsftpd
sudo systemctl disable vsftpd

sudo systemctl enable xinetd
sudo systemctl restart xinetd
```

---

## 6. 動作確認

FTPクライアントから接続する。

```bash
ftp サーバIPアドレス
```

正常にログインできれば設定完了。

