## CodePipeline によるパイプラインの構築および自動デプロイの実行

CodeDeploy の設定が済んだところで、CodePipeline/CodeBuild/CodeDeploy を使用したパイプラインを作成していきます。

今回作成するパイプラインは以下図の左側の部分です。

![構成図](https://cacoo.com/diagrams/Cl9H3qce7UsvLRVO-521EE.png)

では、早速作成していきましょう。

マネジメントコンソールのトップ画面より「CodePipeline」をクリックします。

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2017/05/devops-hands-on-15.png)

以下のような画面が表示されるので「パイプラインの作成」をクリックします。

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2018/10/3a19eb848252813419c880402c423c1e.png)

パイプラインのセットアップが開始するので順番に値を入力していきます。

| 入力項目               | 値                                              |
| ---------------------- | ----------------------------------------------- |
| パイプライン名         | `hands-on-pipeline`                             |
| サービスロール         | 既存のサービスロール                            |
| ロール名               | `hands-on-environment-CodePipeline-ServiceRole` |
| アーティファクトストア | デフォルトの場所                                |

次はソースプロバイダのセットアップが始まるので以下の表のように入力後、「次のステップ」をクリックします。

| 入力項目           | 値                           |
| ------------------ | ---------------------------- |
| ソースプロバイダ   | GitHub                       |
| リポジトリ         | フォークしておいたリポジトリ |
| ブランチ           | master                       |
| 変更検出オプション | GitHub ウェブフック (推奨)   |

ビルドプロバイダのセットアップが始まるので以下の表のように入力後、「ビルドプロジェクトの保存」をクリックしてから「次のステップ」をクリックします。

ビルドステージの設定画面になるので、ビルドプロバイダで`CodeBuild`を選択し、`Create Project`をクリック。

CodeBuild のプロジェクトの作成画面が表示されるので CodeBuild の設定項目を入力していきます。

| 入力項目                                                                                     | 値                                                               |
| -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| プロジェクト名                                                                               | hands-on-project                                                 |
| 環境イメージ                                                                                 | マネージド型イメージ                                             |
| OS                                                                                           | Ubuntu                                                           |
| ランタイム                                                                                   | Node.js                                                          |
| ランタイムバージョン                                                                         | aws/codebuild/nodejs:10.1.0                                      |
| Image version                                                                                | このランタイムバージョンには常に最新のイメージを使用してください |
| 特権付与                                                                                     | チェックしない                                                   |
| サービスロール                                                                               | 既存のサービスロール                                             |
| ロール名                                                                                     | `hands-on-environment-CodeBuild-ServiceRole`                     |
| AWS CodeBuild にこのサービスロールの編集を許可し、このビルドプロジェクトでの使用を可能にする | チェックしない                                                   |
| ビルド仕様                                                                                   | buildspec ファイルを使用する                                     |

最後にデプロイステージの設定を行います

| 入力項目           | 値                      |
| ------------------ | ----------------------- |
| デプロイプロバイダ | AWS CodeDeploy          |
| アプリケーション名 | `hands-on-app`          |
| デプロイグループ名 | `hands-on-deploy-group` |

最後に確認画面が表示されるので、内容を確認後、「パイプラインの作成」をクリックします。

すると、パイプラインが自動で開始されます。

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2018/10/FireShot-Capture-14-AWS-Developer-Tools_-https___ap-northeast-1.console.aws_.png)
