# development-scribble
development-scribble

## 5.1 DFMEAテンプレート
| 番号 | 項目 | 説明 |
|------|------|------|
| 1 | **アイテム** | 分析対象の製品や部品のことを指します。例えば、車のエンジンやスマートフォンのバッテリーなど。これらのアイテムは、一つ以上の機能を持っています。 |
| 2 | **機能** | アイテムが持っている役割や動作のことです。例えば、エンジンの機能は車を動かすことであり、バッテリーの機能は電力を供給することです。 |
| 3 | **要求事項** | 機能が正常に働くために必要な条件や仕様のことです。例えば、エンジンが一定の馬力を出すことや、バッテリーが一定時間電力を供給できることが要求事項です。 |
| 4 | **故障モード** | アイテムが要求事項を満たさない可能性のある具体的な方法です。例えば、エンジンが動かなくなる、バッテリーが電力を供給できなくなることなどが故障モードです。 |
| 5 | **影響** | 故障モードが機能や顧客に与える潜在的な影響のことです。例えば、エンジンが故障すると車が動かなくなり、顧客が困ることなどです。 |
| 6 | **重大度 (S)** | 故障モードの影響がどれだけ深刻かを示す指標です。1から10のスケールで評価され、10が最も深刻なリスクを示します。例えば、エンジンが完全に動かなくなることは重大度が高い（10に近い）と評価されます。 |
| 7 | **クラス** | 特別な製品特性や高リスクの故障モードを示します。例えば、安全性に直結する部分や法律で規定されている特性などです。 |
| 8 | **原因** | 故障が発生する理由のことです。例えば、エンジンの部品が摩耗することや、バッテリーの化学反応が劣化することが原因になります。 |
| 9 | **予防管理** | 故障の原因を防ぐために設計された対策です。例えば、エンジンに定期的なメンテナンスを行うことや、バッテリーの温度管理を行うことです。 |
| 10 | **発生確率 (C)** | 故障が発生する可能性を示す指標です。1から10のスケールで評価され、10が最も発生確率が高いことを示します。例えば、古いエンジンは新しいエンジンよりも故障する確率が高いかもしれません。 |
| 11 | **検出管理** | 故障やその原因を検出するために設計された対策です。例えば、エンジンの異常を検出するためのセンサーを設置することなどです。 |
| 12 | **検出能力 (D)** | 故障を検出する方法の効果を示す指標です。1から10のスケールで評価され、10が最も検出能力が低いことを示します。例えば、エンジンに異常があっても検出できない場合は検出能力が低い（10に近い）と評価されます。 |
| 13 | **RPN** | リスク優先度番号（Risk Priority Number）のことです。重大度、発生確率、検出能力に基づいてプロセスのリスクを評価する指標です。RPNの計算式は、RPN = 重大度 (S) x 発生確率 (C) x 検出能力 (D)です。 |
| 14 | **アクション** | 故障モードの原因を排除または減少させるための推奨行動です。例えば、エンジンの部品を強化することや、バッテリーの製造プロセスを改善することです。 |
| 15 | **責任** | 推奨行動を実施する責任を持つ個人またはチーム／部門のことです。 |
| 16 | **目標完了日** | 推奨行動を完了する予定の日付です。 |
| 17 | **実際の完了日** | 推奨行動が実際に完了した日付です。 |
| 18 | **再評価: 重大度** | 是正措置後の故障モードの重大度を再評価することです。 |
| 19 | **再評価: 発生確率** | 是正措置後の発生確率を再評価することです。 |
| 20 | **再評価: 検出能力** | 是正措置後の検出能力を再評価することです。 |
| 21 | **再評価: RPN** | 是正措置後のリスク優先度番号を再計算することです。 |

## 5.2 DFMEAの入力

DFMEAを実施する際に、チームが考慮すべき入力リソースは以下の通りです：

1. **ブロック（境界）図**:
   - 製品のブロック図は、製品のコンポーネント間の物理的および論理的な関係を示します。ブロック図はDFMEAに含めるアイテムを決定するために使用できます。

2. **パラメータ（P）図**:
   - P-Diagramは、設計の機能に関連する物理現象を、入力、出力、制御、ノイズファクターをリストアップすることで説明する構造化されたツールです。

3. **品質履歴**:
   - 潜在的な故障モードを見つけ、新しい設計における予防措置の有効性を確認するために使用できます。

4. **図面、技術仕様**:
   - 機能と要件を決定するために使用できます。

5. **部品表（BOM）**:
   - 製品のコンポーネント/部品のリストです。

## 5.3 DFMEAを開発する手順

すべてが準備できたら、DFMEAチーム、テンプレート、およびサポート文書を用いて、以下の9つのステップに従ってDFMEAを実施します：

1. **製品要件の定義**:
   - 製品が満たすべき要件を明確に定義します。

2. **潜在的な故障モードのブレインストーミング**:
   - 製品やシステムに起こり得る故障モードを洗い出します。

3. **影響の分析**:
   - 各故障モードが製品やシステム、顧客に与える影響を分析します。

4. **潜在的な原因の特定**:
   - 各故障モードの原因を特定します。

5. **現在の予防措置の説明**:
   - 潜在的な原因に対して現在行われている予防措置を説明します。

6. **発生確率/検出の評価**:
   - 現在の状態における発生確率と検出の評価を行います。

7. **RPNの計算とリスクの評価**:
   - RPNを計算し、リスクを評価します。

8. **是正措置計画**:
   - 必要に応じて是正措置の計画を立てます。

9. **是正措置後のRPNの再評価**:
   - 是正措置を実施した後、RPNを再評価します。
     
https://www.iqasystem.com/news/dfmea-complete-guide/  
https://www.ansys.com/ja-jp/blog/what-is-dfmea

### DFMEAを活用した設計仕様の検討手順

1. **アイテムの特定**
    - 具体的な部品やサブシステムを特定します。例えば、通信モジュールやフェールセーフ機能など。

2. **機能の特定**
    - 各アイテムの具体的な機能をリストアップします。例えば、通信モジュールの機能は車両ネットワークとのデータ通信です。

3. **要求事項の特定**
    - 各機能が正常に動作するために必要な条件をリストアップします。例えば、通信モジュールの場合、一定のデータ転送速度や信頼性の確保などです。

4. **故障モードの特定**
    - 要求事項を満たさない可能性のある具体的な方法を考えます。例えば、通信モジュールがデータを正しく送受信できないことなどです。

5. **影響の特定**
    - 各故障モードが機能や顧客に与える影響を分析します。例えば、通信モジュールが故障すると他の車両システムとの連携が取れなくなり、安全性が低下する可能性があります。

6. **重大度の評価**
    - 各影響の重大度を1から10のスケールで評価します。例えば、通信モジュールの故障が重大な安全問題を引き起こす場合、重大度は高くなります（例: 8-10）。

7. **原因の特定**
    - 各故障モードが発生する原因を考えます。例えば、通信モジュールのハードウェア故障やソフトウェアバグなどです。

8. **予防管理の設計**
    - 故障原因を防ぐための設計仕様を考えます。例えば、通信モジュールに冗長性を持たせる、ソフトウェアのバグチェック機能を強化するなどです。

9. **発生確率の評価**
    - 各故障モードが発生する確率を1から10のスケールで評価します。例えば、ハードウェア故障の発生確率が低い場合、発生確率は低くなります（例: 2-3）。

10. **検出管理の設計**
    - 故障やその原因を検出するための方法を考えます。例えば、通信モジュールに異常検知機能を追加するなどです。

11. **検出能力の評価**
    - 検出方法の効果を1から10のスケールで評価します。例えば、異常検知機能が高い精度で故障を検出できる場合、検出能力は高くなります（例: 2-3）。

12. **RPNの計算**
    - 各故障モードのリスク優先度番号（RPN）を計算し、リスクを評価します。RPN = 重大度 (S) x 発生確率 (C) x 検出能力 (D)。

13. **是正措置の計画**
    - RPNに基づいて必要な是正措置を計画し、実行します。

14. **再評価**
    - 是正措置後に重大度、発生確率、検出能力を再評価し、RPNを再計算します。

