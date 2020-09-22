# WordPress Wordmove
WordPressのローカル環境。

### WordPress
http://wordpress.test/

### MailHog
http://wordpress.test:8025

### WordMove

ローカルとリモートの同期。

Vagrantの起動前に環境変数を設定する。
```
.env
```

or

MovieFileを修正する。
```
wordmove\movefile.yml
```


#### 同期

リモートに反映
```
wordmove push --all
```

ローカルに反映
```
wordmove pull --all
```



## Vagrant
コメントアウト。

`vi .env`

```
## VAGRANT
VAGRANT_DEVCONTAINER_PATH=/vagrant/.devcontainer/
VAGRANT_HOST="wordpress.test"
```

Vagrant起動。

```
vagrant up
```

`.vscode/settings.json` に`docker.host`が設定されているので、それを参考に、SSHでログインできるように公開鍵を登録する。

```
cp ~/.ssh/id_ed25519.pub ./
vagrant ssh
cat /vagrant/id_ed25519.pub >> ~/.ssh/authorized_keys
rm /vagrant/id_ed25519.pub
exit
ssh vagrant@wordpress.test
```

`ssh-agent`に鍵を登録する。

```
ssh-add ~/.ssh/id_ed25519
```

拡張機能`Docker`で接続できるか確認。
