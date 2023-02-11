angular-docs

```bash
$ docker run -itd --name angularnode -w /app -v "$(pwd)/app:/app" -p 3000:3000 node

$ docker exec -it angularnode /bin/bash

$ docker restart angularnode
```

- ワークスペースとはプロジェクトの集まりである。プロジェクトはアプリケーションやライブラリになる前の、構成ファイルの集まりである。アプリケーションの基本単位はコンポーネントである。Angularのコンポーネント構成ファイルは３つである：コンポーネントクラスのTSファイル、テンプレートであるHTMLファイル、コンポーネントのためのプライベートスタイルシートであるCSSファイルである。

- 新規プロジェクトを開始するには`ng new [new-project-name]`コマンドを実行する。このコマンドで新規ワークスペースと新規アプリケーション、設定ファイルが生成される。ワークスペースディレクトリで`ng serve --host 0.0.0.0`を実行するとアプリケーションがビルドされ、ローカルサーバが起動する。Angularはファイルの変更を監視し、ホットリロードに対応している。

- 新規コンポーネントを作成するには`ng generate component [component-name]`を実行する。このコマンドでTS、HTML、CSSファイルが生成され、AppModuleの`@NgModule`の`declarations`に追加される。TSファイルのクラスでは、必ず`Component`シンボルをインポートし、クラスを`@Component`でアノテートする必要がある。アノテーション内で、コンポーネントのセレクタ名（`selector`）、テンプレートのファイルパス（`templateUrl`）、スタイルシートのファイルパス（`styleUrls`）を設定する。

コンポーネントにおいて、一方向バインディング（TSクラスのプロパティからHTMLテンプレート）を行うには二重波括弧を用いる。双方向バインディング（TSクラスのプロパティのデータをHTMLテンプレートに表示し、HTMLテンプレートでユーザの入力を取得しTSクラスのプロパティを書き換える）を用いるにはAppModuleの`@NgModule`の`imports`に`FormsModules`をインポートし、テンプレート内で`[(ngModel)]`を用いる必要がある。