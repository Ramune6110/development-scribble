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


