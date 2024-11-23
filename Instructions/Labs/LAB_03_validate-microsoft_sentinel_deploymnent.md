---
lab:
  title: '演習 03: Sentinel のデプロイを検証する'
  module: 'Guided Project - Configure Microsoft Sentinel Data Collection rules, NRT Analytic rule and Automation'
---

>**注**: このラボはラボ 01 とラボ 02 に基づいて構築されています。 このラボを完了するには、[Azure サブスクリプション](https://azure.microsoft.com/free/?azure-portal=true)が必要です。 管理アクセス権を持つ。

## 一般的なガイドライン

- オブジェクトを作成するときは、異なる構成を必要とする要件がない限り、既定の設定を使用します。
- オブジェクトの作成、削除、または変更は、指定された要件を達成するためにのみ行います。 環境に対する不要な変更は、最終的なスコアに悪影響を与える可能性があります。
- 目標を達成するための複数の方法がある場合は、常に最小限の管理労力で済むアプローチを選択します。

Windows を実行する仮想マシンからセキュリティ イベントを受信するように Microsoft Sentinel を構成する必要があります。

## アーキテクチャ ダイアグラム

![DCR を使用した AMA 経由のWindows セキュリティ イベントのダイアグラム](../Media/apl-5001-lab-diagrams-lab03.png)

## スキルアップ タスク

次の要件を満たすために、Microsoft Sentinel のデプロイを検証する必要があります。

- VM1 という名前の仮想マシンからのみすべてのセキュリティ イベントを収集するように、AMA コネクタを使用して Windows セキュリティ イベントを構成します。
- 次のクエリに基づいてインシデントを生成するほぼリアルタイム (NRT) のクエリ ルールを作成します。

```KQL
SecurityEvent 
| where EventID == 4732
| where TargetAccount == "Builtin\\Administrators"
```

- NRT ルールによって生成されるインシデントの所有者ロールを Operator1 に割り当てるオートメーション ルールを作成します。

## 演習の手順

>**注**: 次のタスクでは、`Microsoft Sentinel` にアクセスするために、ラボ 01 で作成した `workspace` を選択します。

### タスク 1 - Microsoft Sentinel でデータ収集ルール (DCR) を構成する

AMA コネクタを使用して Windows セキュリティ イベントを構成します。 詳細については、[AMA コネクタを使用した Windows セキュリティ イベント](https://learn.microsoft.com/azure/sentinel/data-connectors/windows-security-events-via-ama)を参照してください。

 1. `Microsoft Sentinel` で、`Configuration` メニュー セクションに移動し、**[データ コネクタ]** を選択します。
 1. **AMA を使用した Windows セキュリティ イベント**を検索して選択します
 1. **[コネクタ ページを開く]** を選択します
 1. `Configuration` 領域で、**[+ データ収集ルールの作成]** を選びます
 1. `Basics` タブで、`Rule Name` を入力します
 1. `Resources` タブで、サブスクリプションと `Scope` 列の `RG1` リソース グループを展開します
 1. `VM1` を選んでから、**[次へ: 収集 >]** を選びます
 1. `Collect` タブの `All Security Events` を既定値のままにします。
 1. **[次へ: 確認および作成 >]** を選択してから、**[作成]** を選択します

### タスク 2 - ほぼリアルタイム (NRT) のクエリ検出を作成する

Microsoft Sentinel でほぼリアルタイム (NRT) の分析ルールを使用し、脅威を検出します。 詳細については、[Microsoft Sentinel の NRT 分析ルール](https://learn.microsoft.com/azure/sentinel/near-real-time-rules)を参照してください。

 1. `Microsoft Sentinel` で、`Configuration` メニュー セクションに移動し、**[分析]** を選択します
 1. **[+ 作成]** を選択し、 **[NRT クエリ ルール (プレビュー)]** を選択します
 1. ルールの `Name` を入力し、`Tactics and techniques` から **[特権エスカレーション]** を選択します。
 1. **[次へ: ルール ロジックの設定 >]** を選択します
 1. `Rule query` フォームに KQL クエリを入力します

    ```KQL
    SecurityEvent 
    | where EventID == 4732
    | where TargetAccount == "Builtin\\Administrators"
    ```

 1. **[次へ: インシデント設定 >]** を選択し、**[次へ: 自動応答 >]** を選択します
 1. **[次へ: 確認および作成]** を選択します
 1. 検証が完了したら **[保存]** を選択します

### タスク 3 - Microsoft Sentinel でオートメーションを構成する 

Microsoft Sentinel でオートメーションを構成します。 詳細については、[Microsoft Sentinel オートメーション ルールの作成と使用](https://learn.microsoft.com/azure/sentinel/create-manage-use-automation-rules)を参照してください。

 1. `Microsoft Sentinel` で、`Configuration` メニュー セクションに移動し、**[オートメーション]** を選択します
 1. **[+ 作成]**、[オートメーション ルール] の順に選択します
 1. `Automation rule name` を入力し、`Actions` から **[所有者の割り当て]** を選択します
 1. 所有者として **Operator1** を割り当てます。
 1. **[適用]** を選択します
