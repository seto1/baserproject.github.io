# 開発環境の構築

開発は、Vagrant 上に Docker コンテナを立ち上げる、Docker on Vagrant で行います。

　
## 構築手順

### 事前準備
[VirtualBox](https://www.virtualbox.org/) と [Vagrant](https://www.vagrantup.com/) をインストールします。

　
### ucmitz をクローンする
ucmitz をクローンし、`dev` ブランチに切り替えます。
```shell
git clone https://github.com/baserproject/ucmitz.git
git checkout dev
```

　
### Vagrant のプラグインのインストール
ucmitz プロジェクトのディレクトリに移動しプラグインのインストールコマンドを実行します。
```shell script
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-docker-compose
```

　
### Vagrantfile をコピーする
Vagrant の設定ファイルをコピーし、メモリ等、必要があれば設定を修正します。
```shell script
cp vagrant/Vagrantfile Vagrantfile
```
※ このファイルは自由に編集可能です。

　
### docker-compose をコピーする
```shell script
cp docker/docker-compose.yml.default docker/docker-compose.yml
```
※ このファイルは自由に編集可能です。

　
### Vagrant を実行する
Vagrant を実行すると、CentOS をインストールし、その上に、Docker や、docker-compose のインストールが始まります。
docker-compose も自動実行しますので、他に何もする必要がありません。以上で環境構築は終了です。
```shell script
vagrant up
```
ローカルの ucmitz のディレクトリは、/vagrant にマウントされています。  
マウントが正常にできていない場合は、[トラブルシューティング](https://baserproject.github.io/5/ucmitz/etc/troubleshooting#Vagrantのマウントが正常に行われない) を参照してください。

- アプリケーション：[https://localhost/](https://localhost/)
- phpMyAdmin：[http://localhost:8080/](http://localhost:8080/)
- phpPgAdmin：[http://localhost:10080/](http://localhost:10080/)
- MailCatcher：[http://localhost:1080/](http://localhost:1080/)

　
### 管理画面にログインする

ブラウザで、次のURLにアクセスして表示を確認します。
[https://localhost/baser/admin/baser-core/users/login](https://localhost/baser/admin/baser-core/users/login)
   
メールアドレスとパスワードを入力し管理画面にログインします。

- ユーザー名：admin@example.com
- パスワード：password

　
## コンテナへのログイン方法
まず、Vagrant にログインした後にコンテナにログインします。

```shell script
vagrant ssh
cd /vagrant/docker
docker exec -it bc5-php /bin/bash
```

## データベースのマイグレーションを実行
次のコマンドを実行してデータベースを初期化します。
```shell script
bin/cake migrations migrate --plugin BaserCore
bin/cake migrations seed --plugin BaserCore
```
詳しくは[データベースにおける注意点](https://baserproject.github.io/5/ucmitz/development/migration_point/database)を参照してください。

　
## データベース情報
| name | value |
|-----------|:------------|
| host | bc5-db |
| database | basercms |
| user | root |
| password | root |

　
