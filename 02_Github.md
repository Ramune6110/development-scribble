# １．新規リポジトリ作成
https://qiita.com/sodaihirai/items/caf8d39d314fa53db4db  
https://qiita.com/ucan-lab/items/e02f2d3a35f266631f24#_reference-7cbea668d512073f0df4

# ２．ブランチ保護ルール

GitHubでは、特定のブランチに対して編集やプッシュを完全に禁止することが可能です。これを行うためには、「ブランチ保護ルール」を設定します。以下はその手順です。

1. **リポジトリに移動**:
   - 編集を禁止したいブランチがあるリポジトリを開きます。

2. **設定 (Settings) に移動**:
   - リポジトリの上部にある「Settings」タブをクリックします。

3. **ブランチ (Branches) 設定に移動**:
   - 左側のメニューから「Branches」を選択します。

4. **ブランチ保護ルールを作成**:
   - 「Branch protection rules」のセクションで、「Add rule」をクリックします。

5. **ルールの設定**:
   - 「Branch name pattern」に編集を禁止したいブランチ名を入力します（例えば、`main`や`release`など）。
   - 「Protect this branch」を有効にします。

6. **編集禁止のオプションを設定**:
   - 「Require a pull request before merging」を有効にします。これにより、ブランチへの直接プッシュが禁止されます。
   - 「Include administrators」オプションを有効にすることで、管理者もこのルールに従う必要があります。
   - 他にも必要に応じて、「Require status checks to pass before merging」や「Require review from Code Owners」などのオプションを設定します。

7. **ルールを保存**:
   - 最後に「Create」または「Save changes」をクリックして、設定を保存します。

これにより、指定したブランチへの直接の編集やプッシュが禁止され、プルリクエストを通じてのみ変更を加えることができます。

## 設定オプション
GitHubのブランチ保護ルールには、編集やプッシュを制限するためのさまざまなオプションがあります。以下はその詳細です：

### 必須のルール
1. **Require a pull request before merging**: プルリクエストを経由しないとブランチに変更をマージできないようにします。

### 状態チェックのルール
2. **Require status checks to pass before merging**: 必須の状態チェックがパスしないとブランチにマージできないようにします。具体的なチェック（例えば、テストの成功など）を指定できます。
   - **Require branches to be up to date before merging**: ブランチが最新の状態であることを要求します。

### コードレビューのルール
3. **Require pull request reviews before merging**: プルリクエストがマージされる前にレビューが必要です。レビューの数を指定できます。
   - **Dismiss stale pull request approvals when new commits are pushed**: 新しいコミットがプッシュされた場合、既存のプルリクエストの承認を無効にします。
   - **Require review from Code Owners**: コードオーナーによるレビューを必須にします。

### 署名のルール
4. **Require signed commits**: すべてのコミットがGPGキーで署名されていることを要求します。

### マージの制限
5. **Require linear history**: マージコミットを避け、リニア（直線的な）履歴のみを許可します（リベースとマージ戦略のみを許可）。
6. **Include administrators**: 管理者にもブランチ保護ルールを適用します。これにより、管理者でも直接編集できなくなります。

### プッシュ制限のルール
7. **Restrict who can push to matching branches**: 特定のユーザーやチームのみがブランチにプッシュできるように制限します。

### 強制プッシュの制限
8. **Restrict force pushes**: 強制プッシュを禁止します。
9. **Restrict deletion**: ブランチの削除を制限します。

### 設定手順

1. リポジトリのトップページで、 **Settings** をクリックします。
2. 左サイドバーで **Branches** をクリックします。
3. **Branch protection rules** セクションで、 **Add rule** をクリックします。
4. ブランチ名のパターン（例：`main`）を入力し、上記のオプションを選択して必要なルールを設定します。
5. **Create** または **Save changes** をクリックしてルールを保存します。

このように設定することで、指定したブランチに対する編集を完全に禁止し、必要な手続きやレビューを経ることを強制できます。

＜参考文献＞  
https://zenn.dev/json_hardcoder/articles/f9b534377103a4  
https://qiita.com/KeisukeKudo/items/6404f51d1f4407661321  

# ３．Pull，Push
https://qiita.com/hiroaki-u/items/4e97f338ad18fca142b8  
https://qiita.com/nt-7/items/c5ea999a2638e03ee418

# ４．brnach切る
https://qiita.com/ysk91_engineer/items/f7676f2a7cde7251132b  
https://qiita.com/shuntaro_tamura/items/6c8bf792087fe5dc5103

# ５．tag
GitHubのタグ（tag）は、リポジトリ内の特定のコミットを特定の名前でマークするためのラベルです。タグを使うと、特定のバージョンのコードを簡単に参照したり、管理したりすることができます。例えば、ソフトウェアのリリースごとにタグを付けることが一般的です。

以下に、初心者向けにタグについて説明します：

### タグの目的と利点

1. **特定のポイントを記録**: タグを使うと、リポジトリの特定の時点を簡単に参照できます。これにより、重要なバージョン（例：v1.0、v2.0など）を簡単に見つけることができます。
2. **リリースの管理**: ソフトウェアの新しいバージョンをリリースするたびにタグを付けることで、そのバージョンの状態を保存し、後で簡単にアクセスできます。
3. **変更の追跡**: タグを使うと、どのコミットがどのバージョンに対応するのかを明確にできます。

### タグの作成方法

タグはコマンドラインから簡単に作成できます。以下に基本的な手順を説明します。

1. **リポジトリをクローン**:
   - まず、リポジトリをローカルにクローンします。
     ```bash
     git clone https://github.com/username/repository.git
     cd repository
     ```

2. **タグを作成**:
   - 特定のコミットにタグを付けます。例えば、現在の最新のコミットに`v1.0`というタグを付ける場合、以下のコマンドを使います。
     ```bash
     git tag v1.0
     ```

3. **タグをリモートにプッシュ**:
   - タグをリモートリポジトリにプッシュします。
     ```bash
     git push origin v1.0
     ```

### タグの使用例

例えば、あなたがバージョン1.0のリリースを行ったとしましょう。その時点でのコードに`v1.0`というタグを付けることで、その時点のコードを簡単に参照できます。

1. **特定のタグをチェックアウト**:
   - タグを付けた特定のバージョンのコードを取得するには、以下のようにします。
     ```bash
     git checkout v1.0
     ```

これにより、バージョン1.0のコードが取得されます。

### タグの種類

Gitには主に2種類のタグがあります：

1. **軽量タグ（Lightweight Tag）**: ただ単にコミットに名前を付けるだけのシンプルなタグ。
2. **注釈付きタグ（Annotated Tag）**: メタデータ（作成者、日付、メッセージなど）を含むタグ。通常、公式なリリースには注釈付きタグが使われます。

注釈付きタグの作成例：
```bash
git tag -a v1.0 -m "Initial release"
git push origin v1.0
```

これにより、`v1.0`というタグがリポジトリに作成され、メッセージ「Initial release」がそのタグに付けられます。

### 結論

タグは、特定の時点を記録し、ソフトウェアのバージョンを管理するための便利なツールです。特定のコミットに名前を付けることで、重要なバージョンを簡単に参照し、管理することができます。タグを使うことで、プロジェクトのバージョン管理がより効率的になります。

<参考文献>  
https://www.kagoya.jp/howto/rentalserver/webtrend/gittag/  
https://style.potepan.com/articles/31066.html

# ６．Github Actions
GitHub Actionsは、GitHubリポジトリ内で自動化されたタスク（ワークフロー）を実行するための機能です。これにより、コードのビルド、テスト、デプロイなどの作業を自動化できます。初心者でも理解できるように、GitHub Actionsの基本的な概念と使い方を簡単に説明します。

### GitHub Actionsの基本的な概念

1. **ワークフロー（Workflow）**:
   - ワークフローは、自動化された一連のステップです。GitHub Actionsで実行されるタスクの集まりを指します。これらのワークフローは、リポジトリ内の`.github/workflows`フォルダーにYAMLファイルとして定義されます。

2. **イベント（Event）**:
   - ワークフローは特定のイベントによってトリガーされます。例えば、コードがプッシュされたときやプルリクエストが作成されたときにワークフローを開始できます。

3. **ジョブ（Job）**:
   - ジョブはワークフロー内の単一のタスクセットです。ジョブは複数のステップで構成され、特定の環境（例えば、特定のオペレーティングシステム）で実行されます。

4. **ステップ（Step）**:
   - ステップはジョブ内の個々のアクションです。ステップはシェルコマンドを実行したり、再利用可能なアクションを呼び出したりします。

### GitHub Actionsの基本的な使い方

#### ステップ1: ワークフローの作成

まず、リポジトリにワークフローを作成します。

1. **リポジトリに移動**:
   - GitHubのウェブサイトで、自分のリポジトリに移動します。

2. **ワークフローファイルを作成**:
   - リポジトリのルートディレクトリにある`.github/workflows`フォルダーに、新しいYAMLファイルを作成します。例えば、`main.yml`という名前のファイルを作成します。

3. **ワークフローファイルの内容を定義**:
   - 以下は、シンプルなワークフローの例です。このワークフローは、コードがプッシュされたときに実行されます。

```yaml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```

#### ステップ2: ワークフローの説明

- **name: CI**:
  - ワークフローの名前です。任意の名前を付けることができます。

- **on: [push]**:
  - このワークフローがトリガーされるイベントを指定します。ここでは、コードがプッシュされたときにワークフローが実行されます。

- **jobs**:
  - ワークフロー内のジョブを定義します。ここでは、`build`というジョブを定義しています。

- **runs-on: ubuntu-latest**:
  - ジョブが実行される環境を指定します。ここでは、最新のUbuntu環境でジョブが実行されます。

- **steps**:
  - ジョブ内のステップを定義します。各ステップは個々のタスクです。

  - **Checkout code**:
    - コードをリポジトリからチェックアウトするステップです。`actions/checkout`アクションを使用しています。

  - **Set up Node.js**:
    - Node.jsのセットアップを行うステップです。`actions/setup-node`アクションを使用しています。

  - **Install dependencies**:
    - `npm install`コマンドを実行して依存関係をインストールするステップです。

  - **Run tests**:
    - `npm test`コマンドを実行してテストを実行するステップです。

### まとめ

GitHub Actionsを使用すると、リポジトリ内で様々なタスクを自動化できます。ワークフローを定義することで、コードのビルドやテスト、デプロイなどを簡単に自動化できます。上記のようなシンプルなワークフローから始めて、必要に応じて複雑なワークフローを作成していくことができます。

<参考文献>  
https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e  
