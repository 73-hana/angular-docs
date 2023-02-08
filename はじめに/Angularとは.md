# Angularとは何か？

AngularはTypeScriptをベースに作られた開発プラットフォーム。プラットフォームにはコンポーネントベースのフレームワークや、様々なライブラリコレクション、ツールが含まれている。

# Angularアプリケーション：基本事項

## コンポーネント

アプリを組み立てる構成要素をコンポーネントという。AngularのコンポーネントにはTypeScriptのタイプ（@Componentデコレーターと一緒に使用される）、HTMLテンプレート、スタイルが含まれている。

@Componentデコレーターには要素名になるCSSセレクタ、HTMLテンプレート（ハードコーディングまたはファイル）、CSSスタイルの3つの情報が含まれている。これらの情報をもとに、Angularはコンポーネントのレンダリングを行う。

```ts
// Angularの最小構成
import { Component } from "@angular/core";

@Component({
    selector: "hello-world",
    template: `
        <h2>Hello World</h2>
        <p>This is my first component!</p>
    `
})

export class HelloWorldComponent {
    // コンポーネントの振る舞いを制御するコードを記述する
}
```

上記のコンポーネントを利用する場合は下記のように記述する。

```html
<hello-world></hello-world>
```

## テンプレート

全てのコンポーネントはどのようにレンダリングされるべきかを明確にするため、HTMLテンプレートを持っている。テンプレートは直書きかファイル指定かを選べる。二重波括弧を使うことでDOM要素を動的に変更できる機能が備わっている。その場合の各ファイルの設定は以下の通りである。

```html
<!-- xxxx.html -->
<p>{{ message }}</p>
```

```ts
// xxxx.ts
import { Component } from "@angular/core";

@Component({
    selector: "hello-world-interpolation",
    templateUurl: "./xxxx.html",
})
export class HelloWorldInterpolationComponent {
    message = "hello, world";
}
```

Angularにはプロパティバインディング機能もある。その機能を使うことで、HTMLのプロパティや属性の値を設定したり、アプリのプレゼンテーション層にあたいを値を渡したりすることができる。プロパティバインディングを行うには鉤括弧を使う。イベントリスナの設定には丸括弧を用いる。イベントリスナが呼び出す関数はコンポーネントクラス内で定義する。

```html
<!-- xxxx.html -->
<button
    type="button"
    [disabled]="canClick"
    (click))="sayMessage()">
    Trigger alert message
</button>

<p
    [id]="sayHelloId"
    [style.color]="fontColor">
    You can set my color in the component!
</p>

<p>My color is {{ fontColor }}</p>
```

```js
// xxxx.ts
import { Component } from "@angular/core";

@Component({
    selector: "hello-world-bindings",
    templateUrl: "./xxxx.html",
})

export class HelloWorldBindingsComponent {
    fontColor = "blue";
    sayHelloId = 1;
    canClick = false;
    message = "Hello, World";

    sayMessage() {
        alert(this.message);
    }
}
```

DOM要素をさらに動的に処理するためのディレクティブとして`*ngIf`や`*ngFor`などが利用できる。また、カスタムディレクティブも作成可能。ディレクティブを利用することでプレゼンテーション層からロジックを分離できる。

```html
<!-- xxxx.html -->
<h2>Hello World ngIf!</h2>

<button type="button" (click)="onEditClick()">Make text editable!</button>

<div *ngIf="canEdit; else noEdit">
    <p>You can edit the following paragraph.</p>
</div>

<ng-template #noEdit>
    <p>The following paragraph is read only. Try clicking the button!</p>
</ng-template>

<p [contentEditable]="canEdit">{{ message }}</p>
```

```ts
// xxxx.ts
import { Component } from "@angular/core";

@Component({
    selector: "hello-world-ngif",
    templateUrl: "./hello-world-ngif.component.html"
})

export class HelloWorldNgIfComponent {
    message = "I'm read only";
    canEdit = false;
    
    onEditClick() {
        this.canEdit = !this.canEdit;
        if (this.canEdit) {
            this.message = "You can edit me!";
        } else {
            this.message = "I'm read only!";
        }
    }
}
```

## 依存性の注入

依存性注入を利用することで、TSクラスの依存性をより柔軟に宣言できる。ベストプラクティスとして推奨されている機能である。TSクラスをインスタンス化することなく、Angularコンポーネント内で利用することができる。

```ts
import { Injectable } from "@angular/core";

@Ingectable({ providedIn: "root" })
export class Logger {
    writeCount(count: number) {
        console.warn(count);
    }
}
```

```ts
import { Component } from "@angular/core";
import { Logger } from "../logger.service";

@Component({
    selector: "hello-world-di",
    templateUrl: "./hello-world-di.component.html"
})
export class HelloWorldDependencyInjectionComponent {
    count = 0;
    constructor(private logger: Logger) {}

    onLogMe() {
        this.logger.writeCount(this.count);
        this.count++;
    }
}
```

## Angularのコマンドラインインターフェース

- `ng build`でAngularアプリケーションをコンパイルする
- `ng serve`でビルド、提供を行う
- `ng generate`でファイルの生成を行う
- `ng test` でテストを実行する
- `ng e2e`でAngularアプリケーションをビルドし、E2Eテストを行う

## ライブラリ

- Angular Router: クライアントサイドのナビゲーションとルーティングを管理する。
- Angular Forms: フォームを構築しバリデーションも行う
- Angular HttpClient: 頑健なHTTPクライアント
- Angular PWA: PWAを構築するためのツール
- Angular Schematics: 大規模な開発を支援するツール
