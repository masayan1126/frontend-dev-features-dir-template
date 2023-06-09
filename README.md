## 機能ベースのディレクトリ構成
脱atomicデザイン
- 技術のはやりすたりは速いもので、もうatomicデザインが古いといわれている
- 技術ではなく、各機能ベースで
- 技術というのは、関数/クラス/コンポーネントなどの区別で、機能というのは文字通り、「認証」であるとか、「注文履歴の表示」などの個別具体的な区別を指す
- わりと、ドメイン駆動に近しいものがある

## 実装例
- 映画館の予約システム（※あくまでイメージです）
- ディレクトリやファイルはサンプルとして配置していますが、実装は書いていません

## 前提
- 技術ベース（関数、クラス、コンポーネント・・・）ではなく、各機能ベースでディレクトリを分ける
  - 機能ベースなので、その機能に関連する関数やクラス、テストなどを同ディレクトリに配置することになる
  - 技術ベースよりも、比較的容易に配置するディレクトリを決定でき、なおかつ人によって差が生じにくいというのが最大のメリット
  - たいてい、機能ベースで開発していくやり方が主流なので、むしろこのやり方が適しているといえる
  - 特に自分以外のエンジニアが作成した機能の場合、技術ベースで分けていると、関連するコードの探索に時間がかかりやすい
  - エディターで対象のファイルを開く際に、機能ベースにしていると比較的近い距離に関連ファイルが見えるので、見通しがよい。技術ベースだと、かなり離れた距離のファイル同士を開いたりするので、思考の邪魔になる
- 特定の機能に依存するかどうかで配置する場所を決定する
  - 依存する場合はfeatures、しない場合はそれ以外の適切なディレクトリとなる

## 各ディレクトリの役割
### components
- 特定の機能に依存しないコンポーネントを配置
- 依存しているカスタムフックや関数、型定義ファイル、定数ファイル、テストなども配置

### configs
- 機能に依存しないアプリケーション全体の設定ファイルを配置
- 主に、外部ライブラリの設定ファイル等を配置

### constants
- 機能に依存しない定数ファイルを配置

### functions
- 特定の機能に依存しない関数を配置
- 依存している定数ファイルや型定義ファイルなども配置する

### classes
- 特定の機能に依存しない関数を配置
- 依存している定数ファイルや型定義ファイル、テストなども配置する


### hooks
- 特定の機能に依存しないカスタムフックを配置
- stateやnext/routerなどのルーティング（Next.jsであれば）を使用する必要のあるファイルを配置するイメージ
- 依存している関数や定数ファイルやクラスなども配置する

### pages
- URLに対応するコンポーネントを配置
- features、componentsからコンポーネントを呼び出して使用する
- Next.jsなら、デフォルトで存在するディレクトリなので、それをそのまま使用する


### features
- 特定の機能に依存するコンポーネントやカスタムフック、関数や型定義ファイル、定数ファイル、css、テストなどを配置する
- 特定のコンテキストでしか使えなかったり、ドメイン知識が入っていたりするものが対象となるイメージ

### types
- 機能に依存しない型定義ファイルを配置する

### store
- 機能に依存しないグローバルステートのファイルを配置する

### schemas
- Zodなどを使用している場合、機能に依存しないスキーマファイルを配置する


## 各ディレクトリの依存関係
- 機能ベースのディレクトリ構成を採用していれば、割と自然に以下のようになるはず
- 機能に依存しているファイルから依存していないファイルの呼び出しは可能だが、その逆は不可

| 分類             | ディレクトリ                               |
| ---------------- | ------------------------------------------ |
| 機能に依存する   | pages、features                            |
| 機能に依存しない | pages、features以外のcomponents、hooksなど |


| 依存の方向            | 可否 |
| --------------------- | ---- |
| pages →   components  | 〇   |
| pages → pages         | ×    |
| features → components | 〇   |
| components → features | ×    |
