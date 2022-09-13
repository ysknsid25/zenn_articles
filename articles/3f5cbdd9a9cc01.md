---
title: "G.I.Gで学んだコマンド"
emoji: "📌"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# VM Instance

## 利用可能なリージョンとゾーンの一覧

```
gcloud compute zones list
```

## VM インスタンスのデフォルトゾーンを設定する

```
gcloud config set compute/zone us-central1-b
```

## VM インスタンスを作成する

```
gcloud compute instances create "my-vm-2" \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-10" \
--subnet "default"
```

# Cloud Storage

## バケットを作成する

リージョンを環境変数にセットする

```
export LOCATION=ASIA
```

バケットを作成。
バケットはグローバルに一意でなくてはならないので、プロジェクト ID を付与する。
プロジェクト ID は環境変数$DEVSHELL_PROJECT_ID から取得できる。

```
gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID
```

## 別のバケットからファイルをコピーする

```
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png
```

```
gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

取得したファイルのアクセス権限を全てのユーザーに変更する。

```
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

# Kubernetes

2 つの node を持つ Kubernetes Engine を作成する。

```
gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

## 稼働している Pod を取得する

```
kubectl get pods
```

## 作成した nginx コンテナを外部に公開する

```
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

## サービスの一覧を確認する

```
kubectl get services
```

# App Engine

## 有効なアカウントの一覧を取得する

```
gcloud auth list
```

## プロジェクトの一覧を取得する

```
gcloud config list project
```

## App Engine のプロジェクトを作成する

```
gcloud app create --project=$DEVSHELL_PROJECT_ID
```

## Deployment Manager を使ってデプロイを行う

初回

```
gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml
```

更新時

```
gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml
```
