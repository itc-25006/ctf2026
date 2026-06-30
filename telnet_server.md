# Telnetサーバ構築マニュアル

## 1. 目的

Telnetサーバをxinetd経由で構築する。

---

## 2. パッケージのインストール

```bash
sudo apt update
sudo apt install xinetd telnetd
```

---

## 3. xinetd設定

設定ファイルを編集する。

```bash
sudo vi /etc/xinetd.d/telnet
```

内容

```ini
service telnet
{
    disable     = no
    flags       = REUSE
    socket_type = stream
    wait        = no
    user        = root
    server      = /usr/sbin/telnetd
    log_on_failure += USERID
}
```

---

## 4. xinetdの再起動

```bash
sudo systemctl restart xinetd
```

状態を確認する。

```bash
sudo systemctl status xinetd
```

---

## 5. 動作確認

Telnetクライアントから接続する。

```bash
telnet サーバIPアドレス
```

ログイン画面が表示されれば構築完了。

