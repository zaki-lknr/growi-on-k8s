# growi-on-k8s

[GROWI Wiki](https://github.com/weseek/growi) on Kubernetes

[weseek/growi-docker-compose: growi-docker-compose - The fastest way to boot All-in-One GROWI](https://github.com/weseek/growi-docker-compose)

解説記事: [Skaffoldを使ってGROWI on K8s用のElasticsearchとHackMDをビルド＆デプロイ - Qiita](https://qiita.com/zaki-lknr/items/58cbd858ceff40c42323)

## Elasticsearch for GROWI

GROWI用にElasticsearchの日本語プラグインを追加したイメージをビルドします。

[elasticsearch/k8s-deploy.yaml](elasticsearch/k8s-deploy.yaml)の下記イメージ設定を環境に合わせてください。

```yaml
    spec:
      containers:
      - image: zakihmkc/growi-elasticsearch:6.8.10
```

単体でデプロイするには`elasticsearch`ディレクトリ以下で`skaffold`を実行します。

## HackMD for GROWI

GROWI用の設定を追加したHackMDのイメージをビルドします。

Elasticsearch同様、[hackmd/k8s-deploy.yaml](hackmd/k8s-deploy.yaml)のイメージ設定を環境に合わせてください。

```yaml
    spec:
      containers:
      - image: zakihmkc/growi-hackmd:1.3.0
```

また、環境変数の設定でブラウザからアクセスする場合のGROWIのURLを設定してください。

```yaml
        env:
          # ...
          - name: GROWI_URI
            value: http://192.168.0.185:3000

```

単体でデプロイするには`hackmd`ディレクトリ以下で`skaffold`を実行します。

## GROWI

環境変数の設定で、ブラウザからアクセスする場合のHackMDのURLを`HACKMD_URI`に設定してください。

[manifests/growi-on-k8s.yaml](manifests/growi-on-k8s.yaml)

```yaml
        env:
          # ...
          - name: HACKMD_URI
            value: http://192.168.0.186:3000

```

Elasticsearch連携を行わない場合は`ELASTICSEARCH_URI`の定義を削除します。  
HackMD連携を行わない場合は`HACKMD_URI_FOR_SERVER`および`HACKMD_URI`の定義を削除します。

単体でデプロイするには`manifests/growi-on-k8s.yaml`を`apply`します。

## build / push / deploy

ルートディレクトリで以下を実行すれば全てデプロイされます。

`skaffold dev -t <tagname> -n <namespace>`
