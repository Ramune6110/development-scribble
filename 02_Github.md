# ０．GitとGithub
GitとGitHubは、バージョン管理とコード共有のために広く使われているツールですが、それぞれ異なる役割を持っています。以下に、それぞれの特徴と違いを初心者向けに詳しく解説します。

### Gitとは？

**Git**は、分散バージョン管理システムです。これは、プロジェクトのソースコードの変更履歴を追跡し、複数人で共同作業をする際に便利です。以下は、Gitの主な特徴です。

1. **ローカルでの管理**:
   - Gitはローカルコンピュータ上で動作します。つまり、全ての変更履歴やバージョンが自分のコンピュータに保存されます。

2. **コミット (Commit)**:
   - コードに変更を加えたら、その変更を「コミット」します。コミットはスナップショットのようなもので、プロジェクトの状態を記録します。

3. **ブランチ (Branch)**:
   - ブランチを使って、異なる機能や修正を並行して開発できます。ブランチを作ることで、元のコードに影響を与えずに新しい作業を進められます。

4. **マージ (Merge)**:
   - 作業が完了したら、ブランチを元のコード（通常は`main`ブランチ）にマージします。これにより、変更が統合されます。

### GitHubとは？

**GitHub**は、Gitをベースにしたクラウドサービスで、リモートリポジトリ（リモートで管理されるGitリポジトリ）をホストします。GitHubはコードを共有したり、共同作業を行うためのプラットフォームです。以下は、GitHubの主な特徴です。

1. **リモートリポジトリ**:
   - GitHubはリモートリポジトリをホストします。これにより、複数の開発者が同じプロジェクトで共同作業できます。

2. **ウェブインターフェース**:
   - GitHubはウェブブラウザからアクセスでき、リポジトリの管理やコードレビュー、プルリクエストの処理などを行うためのユーザーフレンドリーなインターフェースを提供します。

3. **プルリクエスト (Pull Request)**:
   - GitHubでは、プルリクエストを使って変更を提案し、レビューを受けることができます。これは、共同作業やコードレビューのプロセスを簡単にします。

4. **プロジェクト管理ツール**:
   - GitHubは、イシュー（問題・課題）の追跡やプロジェクトボードなどのプロジェクト管理ツールも提供しています。これにより、プロジェクトの進行状況を管理しやすくなります。

5. **アクション (Actions)**:
   - GitHub Actionsを使うと、CI/CD（継続的インテグレーションと継続的デリバリー）パイプラインを作成できます。これにより、コードのビルド、テスト、デプロイを自動化できます。

### GitとGitHubの違い

1. **ツール vs. サービス**:
   - **Git**はローカルで動作するバージョン管理ツールです。
   - **GitHub**はGitリポジトリをホストし、共同作業を支援するクラウドサービスです。

2. **インターフェース**:
   - **Git**はコマンドラインベースの操作が基本です（GUIツールもあります）。
   - **GitHub**はウェブブラウザからアクセスできるユーザーインターフェースを提供します。

3. **コラボレーション**:
   - **Git**はローカルでのバージョン管理が中心です。
   - **GitHub**はリモートリポジトリを使ったコラボレーションとコードレビューに強みがあります。

4. **機能**:
   - **Git**はバージョン管理に特化しています。
   - **GitHub**はバージョン管理に加えて、プロジェクト管理、CI/CD、コードレビューなどの機能も提供します。

### まとめ

- **Git**はバージョン管理システムであり、コードの変更履歴を管理するためのツールです。
- **GitHub**はGitリポジトリをホストし、共同作業を支援するクラウドサービスです。

GitHubを使うことで、Gitの強力なバージョン管理機能を利用しつつ、リモートでの共同作業やプロジェクト管理を容易に行えます。初心者はまずGitの基本的な使い方を学び、その後GitHubを使って他の開発者と共同作業を始めると良いでしょう。
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

## simulinkのCI/CD
SimulinkモデルをGitHubで管理し、GitHub Actionsを使ってそのモデルに対して自動化タスクを実行するには、Simulinkの実行環境であるWindowsを指定してワークフローを構成する必要があります。以下は、Simulinkモデルのビルドとテストを自動化するためのYAMLファイルの例です。

### Simulinkモデル用のGitHub Actionsワークフロー例

```yaml
name: Simulink CI

on: 
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: windows-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Set up MATLAB
      uses: matlab-actions/setup-matlab@v1
      with:
        release: R2022b  # 使用するMATLABのバージョンを指定

    - name: Run Simulink tests
      run: |
        matlab -batch "run('path/to/your/simulink_test_script.m')"
```

### 詳細な説明

1. **name: Simulink CI**:
   - ワークフローの名前です。Simulinkの継続的インテグレーション（CI）プロセスを表します。

2. **on**:
   - このワークフローがトリガーされるイベントを指定します。ここでは、`main`ブランチへのプッシュとプルリクエストがトリガーとなります。

3. **jobs**:
   - ワークフロー内のジョブを定義します。ここでは、`build`というジョブを定義しています。

4. **runs-on: windows-latest**:
   - ジョブが実行される環境を指定します。SimulinkはWindows上で実行されるため、`windows-latest`を使用します。

5. **steps**:
   - ジョブ内の各ステップを定義します。各ステップは個々のタスクです。

6. **Checkout repository**:
   - コードをリポジトリからチェックアウトするステップです。`actions/checkout`アクションを使用しています。

7. **Set up MATLAB**:
   - MATLAB環境をセットアップするステップです。`matlab-actions/setup-matlab`アクションを使用し、MATLABのバージョンを指定します。

8. **Run Simulink tests**:
   - Simulinkテストを実行するステップです。ここでは、MATLABのバッチモードを使用して、指定されたスクリプトを実行します。スクリプト内でSimulinkモデルのビルドやテストを行います。

### Simulinkテストスクリプトの例

以下は、Simulinkモデルのテストを行うMATLABスクリプトの例です。

```matlab
% path/to/your/simulink_test_script.m

% モデルを開く
open_system('your_model_name')

% モデルをビルド
set_param('your_model_name', 'SimulationCommand', 'update')

% テストの実行
% ここにテストスクリプトを記述
% 例: シミュレーションの実行と結果の検証
simOut = sim('your_model_name');
assert(~isempty(simOut), 'Simulation output is empty')

% モデルを閉じる
close_system('your_model_name', 0)
```

このワークフローとスクリプトにより、Simulinkモデルのビルドとテストを自動化できます。GitHub Actionsを利用することで、コードの品質を保ち、リリースプロセスを効率化できます。

<参考文献>  
https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e  
https://qiita.com/shun198/items/14cdba2d8e58ab96cf95  
https://www.kagoya.jp/howto/it-glossary/develop/githubactions/

# ７．Pull request
プルリクエスト（Pull Request、略してPR）は、GitHubの重要な機能の一つで、コードの変更を他の開発者にレビューしてもらい、プロジェクトに統合するためのプロセスです。初心者でも理解できるように、プルリクエストについて詳しく解説します。

### プルリクエストとは？

プルリクエストは、他の開発者に自分の変更をレビューしてもらい、その変更を公式なプロジェクトに取り入れてもらうためのリクエストです。これにより、コードレビューや協力的な開発が容易になります。

### プルリクエストの基本的な流れ

1. **フォーク（Fork）**:
   - 他人のリポジトリに対して変更を提案するには、まずそのリポジトリを自分のアカウントにコピーする（フォークする）必要があります。これにより、自分のコピーが作成されます。

2. **クローン（Clone）**:
   - 自分のフォークしたリポジトリをローカルマシンにクローンします。これにより、ローカルでコードの変更を行うことができます。
     ```bash
     git clone https://github.com/your-username/repository.git
     cd repository
     ```

3. **ブランチ（Branch）を作成**:
   - 新しい機能やバグ修正ごとに新しいブランチを作成します。ブランチを使うことで、元のコードに影響を与えずに作業を進められます。
     ```bash
     git checkout -b my-feature-branch
     ```

4. **変更を加えてコミット（Commit）**:
   - コードに変更を加え、変更をコミットします。
     ```bash
     git add .
     git commit -m "説明的なコミットメッセージ"
     ```

5. **リモートリポジトリにプッシュ（Push）**:
   - 変更を自分のGitHubリポジトリにプッシュします。
     ```bash
     git push origin my-feature-branch
     ```

6. **プルリクエストを作成**:
   - GitHubのウェブサイトに戻り、フォークしたリポジトリのページに行きます。そこで、「Compare & pull request」ボタンをクリックしてプルリクエストを作成します。
   - プルリクエストには、変更内容や変更理由を説明するメッセージを含めます。これにより、レビューアが変更の意図を理解しやすくなります。

### プルリクエストのレビューとマージ

1. **レビュー**:
   - プルリクエストが作成されると、他の開発者がコードをレビューします。レビューアはコメントを追加したり、変更を求めたりすることができます。

2. **修正**:
   - レビューアのフィードバックに基づいて、変更が必要な場合はコードを修正します。修正後、再度コミットしてプッシュすると、プルリクエストに自動的に反映されます。

3. **マージ**:
   - 変更が承認されると、プルリクエストをマージして公式なリポジトリに統合します。マージを行うと、変更が元のリポジトリに反映されます。

### プルリクエストのメリット

1. **コードレビュー**:
   - 他の開発者にコードをレビューしてもらうことで、バグや改善点を早期に発見できます。

2. **協力的な開発**:
   - 複数の開発者が同時に異なる機能や修正を進めることができ、共同作業が効率的になります。

3. **履歴の管理**:
   - 変更の履歴が明確になり、後で変更内容を追跡するのが容易になります。

### まとめ

プルリクエストは、GitHubでの共同開発を円滑に進めるための重要な機能です。ブランチを作成し、変更を加え、プルリクエストを通じて他の開発者に変更をレビューしてもらい、最終的に公式なリポジトリに統合する流れを理解することで、効率的かつ効果的にプロジェクトを進めることができます。初心者の方は、まずは小さな変更から始めて、プルリクエストの流れに慣れると良いでしょう。

# ８．gitロック
GitやGitHub自体には、特定のブランチ内の特定のファイルやフォルダに対する編集や変更を直接制限する機能はありません。しかし、これを実現するためのいくつかの方法があります。

### 1. **GitHub ActionsとCI/CDパイプラインの利用**

特定のファイルやフォルダへの変更を防ぐために、GitHub Actionsや他のCI/CDパイプラインツールを使って、プルリクエストやコミットがそのファイルやフォルダに変更を加えた場合にビルドを失敗させる方法があります。

以下は、GitHub Actionsを使った例です。

#### 例: `.github/workflows/restrict-files.yml`ファイルを作成

```yaml
name: Restrict File Changes

on: 
  pull_request:
    paths:
      - 'restricted-folder/**'
      - 'restricted-file.txt'

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
    - name: Check for restricted file changes
      run: |
        echo "Changes to restricted files are not allowed."
        exit 1
```

このワークフローは、指定したファイルやフォルダに変更が加えられた場合にビルドを失敗させます。

### 2. **Gitフックの利用**

ローカルリポジトリでプリコミットフックやプッシュフックを使用して、特定のファイルやフォルダへの変更を拒否することができます。

#### 例: プリコミットフック

```bash
#!/bin/sh
# .git/hooks/pre-commit

# List of restricted files or directories
restricted_files=("restricted-folder/" "restricted-file.txt")

for file in "${restricted_files[@]}"; do
  if git diff --cached --name-only | grep -q "^$file"; then
    echo "Error: Changes to $file are not allowed."
    exit 1
  fi
done
```

このスクリプトを`.git/hooks/pre-commit`に保存し、実行権限を与えることで、指定したファイルやフォルダに変更が加えられた場合にコミットを拒否します。

### 3. **ブランチ保護ルールとコードオーナーシップ**

GitHubのブランチ保護ルールとコードオーナーファイル（`CODEOWNERS`）を組み合わせて使用することで、特定のファイルやフォルダに対する変更を制限できます。

#### 例: `CODEOWNERS`ファイルの設定

```
# CODEOWNERS
/restricted-folder/ @username
/restricted-file.txt @username
```

この設定により、`restricted-folder`フォルダや`restricted-file.txt`ファイルへの変更は指定されたユーザーの承認を必要とします。

### まとめ

これらの方法を組み合わせて使用することで、GitやGitHubで特定のブランチ内の特定のファイルやフォルダに対する編集や変更を制限することができます。直接的な機能はありませんが、CI/CDツールやGitフックを活用することで、実質的に制限を実現できます。
