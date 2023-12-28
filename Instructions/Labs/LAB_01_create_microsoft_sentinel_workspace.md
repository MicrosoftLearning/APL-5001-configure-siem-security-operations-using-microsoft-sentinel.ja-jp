---
lab:
  title: '演習 01: Microsoft Sentinel をデプロイする'
  module: Guided Project - Create and configure a Microsoft Sentinel workspace
---

>**注**: このラボを完了するには、[Azure サブスクリプション](https://azure.microsoft.com/en-us/free/?azure-portal=true)が必要です。 管理アクセス権を持つ。

## 一般的なガイドライン

- オブジェクトを作成するときは、異なる構成を必要とする要件がない限り、既定の設定を使用します。
- オブジェクトの作成、削除、または変更は、指定された要件を達成するためにのみ行います。 環境に対する不要な変更は、最終的なスコアに悪影響を与える可能性があります。
- 目標を達成するための複数の方法がある場合は、常に最小限の管理労力で済むアプローチを選択します。

現在、企業環境の既存のセキュリティ態勢を評価しています。 将来および進行中のサイバー攻撃の特定に役立つセキュリティ情報イベント管理 (SIEM) のソリューションの設定に関する支援が必要です。

## アーキテクチャ ダイアグラム

![Log Analytics ワークスペースを含むダイアグラム。](../Media/apl-5001-lab-diagrams-01.png)

## スキルアップ タスク

Microsoft Sentinel ワークスペースをデプロイする必要があります。 このソリューションでは、次の要件を満たす必要があります。

- Sentinel データが米国西部 Azure リージョンに保存されていることを確認します。
- すべての Sentinel 分析ログが 180 日間保持されていることを確認します。
- Operator1 にロールを割り当てて、Operator1 がインシデントを管理し、Sentinel プレイブックを実行できるようにします。 ソリューションでは、最小限の特権の原則を満たす必要があります。

## 演習の手順

### タスク 1 - Log Analytics ワークスペースを作成する

リージョン オプションを含む Log Analytics ワークスペースを作成します。 詳細については、[Microsoft Sentinel のオンボーディング](https://learn.microsoft.com/azure/sentinel/quickstart-onboard)を参照してください。

  1. Azure portal で、`Microsoft Sentinel` を検索して選択します。
  1. **[+ 作成]** を選択します。
  1. **[新しいワークスペースの作成]** を選択します。
  1. リソース グループとして `RG2` を選びます
  1. 新しい Log Analytics ワークスペースの有効な名前を入力します。
  1. ワークスペースのリージョンとして `West US` を選択します。
  1. **[確認および作成]** を新しいワークスペースを検証します。
  1. **[作成]** を選択して、ワークスペースをデプロイします。

### タスク 2 - Microsoft Sentinel をワークスペースにデプロイする

Microsoft Sentinel をワークスペースにデプロイします。

  1. `workspace` のデプロイが完了したら、**[最新の情報に更新]** を選択して新しい `workspace` ファイルを表示します。
  1. Sentinel に追加する `workspace` (タスク 1 で作成) を選択します。
  1. **[追加]** を選択します。

### タスク 3 - Microsoft Sentinel ロールをユーザーに割り当てる

Microsoft Sentinel ロールをユーザーに割り当てます。 詳細については、[Microsoft Sentinel で作業するためのロールとアクセス許可](https://learn.microsoft.com/azure/sentinel/roles)を参照してください

  1. リソース グループ RG2 に移動します
  1. **[アクセス制御 (IAM)]** を選択します。
  1. **[追加]**、`Add role assignment` の順に選択します。
  1. 検索バーで、`Microsoft Sentinel Contributor` ロールを検索して選択します。
  1. **[次へ]** を選択します。
  1. `User, group, or service principal` オプションを選択します。
  1. **[+ メンバーの選択]** を選択します。
  1. ラボの手順 `(operator1-XXXXXXXXX@LODSPRODMCA.onmicrosoft.com)` で割り当てられた `Operator1` を検索します。
  1. `user icon` を選択します。
  1. **[選択]** を選びます。
  1. [確認と割り当て] を選択します
  1. [確認と割り当て] を選択します

### タスク 4 - データ保持を構成する

データ保持を構成します。詳細については、[データ保持](https://learn.microsoft.com/azure/azure-monitor/logs/data-retention-archive)を参照してください。

  1. タスク 1 の手順 5 で作成した `Log Analytics workspace` 移動します。
  1. **[使用量と推定コスト]** を選択します。
  1. **[データ保持]** を選択します。
  1. データ保持期間を **180 日**に変更します。
  1. **[OK]** を選択します。

>**注**: 追加の演習を行う場合は、「[Microsoft Sentinel ワークスペースの作成と管理](https://learn.microsoft.com/training/modules/create-manage-azure-sentinel-workspaces/)」モジュールを完了してください。
