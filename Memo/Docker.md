## 忘備録 - 1
Dockerの用意(ファイル:[Setup-Docker.sh](Setup-Docker.sh))
```sh
# パッケージリストの更新
sudo apt update
# 必要パッケージの入手
sudo apt install -y ca-certificates curl gnupg
# docker公式のGPGキーの保存先を作成
sudo install -m 0755 -d /etc/apt/keyrings
# 公式のGPGキーを保存 --> 権限を付与
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# 公式のDockerレポジトリを追加
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# パッケージリストの再更新
sudo apt update
# Dockerのインストール
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# Dockerをroot以外でも実行できるようにする
sudo usermod -aG docker $USER
# 変更の反映
su - ${USER}
# バージョンの確認
docker -v
# 動作確認(任意)
sudo docker run hello-world
```
#### 関連: Docker Hubからのコンテナの取得  
`docker image pull [オプション] [コンテナ名]`  
当てはめると: `docker image pull python`  
取得済みのコンテナを表示するには: `docker image ls`
#### Dockerイメージを使って取得済みのコンテナを起動
`docker container run [オプション] imagename`
例: `docker container run --name httpdcontainer -d -p 8080:80 httpd`
これは、httpdイメージを使ってdockerコンテナを起動しろということ。
引数について:
- `–name`はコンテナ名を指定するオプション。
- 今回は、`httpdcontainer`というコンテナ名。
- `-d`はコンテナをバックグラウンドで実行するオプション。
- `-p`はコンテナのポート番号と、サーバーのポート番号を紐づけるオプション。
- 今回はコンテナの80番ポートと、サーバーの8080番ポートを紐づけた。

これでhttpdのDockerコンテナが起動されたはずです。(多分)  
ブラウザで以下URLにアクセスすると、サンプルのhtmlファイル（「It works!」）が表示されます。
#### Dockerコンテナの停止
`docker container stop [オプション] containername`
例: `docker container stop httpdcontainer`
#### Dockerコンテナの再開
`docker container start [オプション] containername`
例: `docker container start httpdcontainer`
> [!NOTE]
> 初回起動時はrunですが、再開はstartです。
> 間違えますとエラーを吐きます。
