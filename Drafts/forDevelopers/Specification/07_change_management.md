# AnchorSpec - Specification
# 変更管理モデル

---

## Specification Lifecycle

AnchorSpecでは、Specification（Spec）をプロジェクトにおける承認済みの事実として扱う。   
Specは直接編集されない。   
すべての変更はChange Request（CR）として提案され、承認された変更のみが新たなSpecへ反映される。   
これにより、プロジェクトは変更履歴と意思決定理由を保持したまま進化できる。   

---

### Spec Immutability

Specは現在有効な承認済み状態を表す。   
一度承認されたSpecは不変（Immutable）として扱われる。   

直接編集を許可すると、   

- 何が変更されたのか
- なぜ変更されたのか
- いつ変更されたのか

が失われる可能性がある。   

そのため、Specへの変更は必ずCRを経由しなければならない。   
Specは常にプロジェクトの信頼できる基準点（Baseline）として維持される。   

---

### Change Request Lifecycle

Change Request（CR）はSpecに対する変更提案である。

CRはSpecとは独立して存在し、承認されるまではSpecへ影響を与えない。

CRは以下の状態を取る。

|状態       |説明                   |
|-----------|-----------------------|
|Draft	    |作成直後の提案          |
|Reviewed	|内容が検討された状態    |
|Approved	|採用が承認された状態     |
|Rejected	|却下された状態          |
|Archived	|履歴として保存された状態 |
   
CRは要求変更、設計変更、実装制約の追加、改善提案などを表現できる。   

---

### Promotion from CR to Spec

ApprovedとなったCRはSpecへ昇格できる。   

昇格時には、   

1. CRの内容をSpecへ反映する
2. 変更理由を記録する
3. 新しいSpecを生成する

過去のSpecは履歴として保持される。   
これにより、現在の仕様だけでなく、その仕様へ至った経緯も追跡できる。   

---

### FreezeSpec

実装を開始する前にSpecをFreezeする。   
FreezeSpecは実装および検証の基準となる。   
Freezeの目的は仕様ドリフトを防ぎ、実装・検証・レビューが同一の基準を参照できる状態を維持することである。   
Freeze後も変更提案は可能である。   
ただし、それらは新しいCRとして管理され、現在のFreeze対象へ直接反映されない。   
Freezeによって以下が保証される。   

- 安定した実装対象
- 安定した検証対象
- 再現可能な開発状態

---

### ThawSpec and Build Planning

FreezeSpecはそのままでは実装できない。   
実装を開始する際はSpecをThawし、実装可能な状態へ展開する。   
Thaw時には以下を実施する。   

- FreezeSpecを読み込む
- ImplContextを適用する
- 実装制約を解決する
- Build Planを生成する

ImplContextには以下が含まれる。   

1. 使用言語
2. フレームワーク
3. ランタイム
4. プラットフォーム
5. 非機能要件

Thawの結果としてBuild Planが生成される。   
Build PlanはSpecをどのように実装するかを定義した計画書である。   

---

### Build Loop

Build Planに従い実装を行う。   
実装主体は人間、AI、またはその協働である。   

実装中に新たな発見があった場合は、   

- CRを作成する
- Issueを記録する

のいずれかを行う。   

承認済みSpecを直接変更してはならない。   
Build Loopの目的はSpecを満たすビルドを生成することである。   

---

### Verify Loop

ビルド完了後はVerifyを実施する。   
Verifyは実装結果とSpecとの整合性を確認する工程である。   
Verifyは以下の観点から確認を行う。   

- Spec Check
- Impl Check
- Intent Check
- Coverage Check

Verifyの責務は問題の発見である。   
Verifyは修正を行わない。   

発見された問題はCRのIssueを生成する。   
問題が存在する場合はBuild Loopへ戻る。   

---

### Runtime / Behavior Confirmation

ビルド成功は動作成功を保証しない。   
コンパイルやビルドが成功していても、   

- 要求を満たしていない
- ユーザー体験が期待と異なる
- 運用上の問題が存在する

可能性がある。   

そのため、最終的な動作確認はプロジェクト責任者が実施する。   
Runtime Confirmationの責任はVerifyではなくプロジェクト側にある。   

---

### Issue Recording

問題、懸念事項、改善案が発見された場合は CR の Issue として記録する。   
Issue は新鮮なうちに記録することを推奨する。   

記録を後回しにすると、   

- 発生条件
- 判断理由
- 背景情報

が失われる可能性がある。   

Issue はバグだけを意味しない。   

- 技術的負債
- 改善提案
- 未解決事項
- 将来検討事項

も Issue として扱う。   

### Build Loop Completion

以下の条件を満たした場合、AnchorSpecのBuild Loopは完了とみなされる。   

1. Buildが成功している
2. Verifyが完了している
3. Runtime Confirmationが完了している
4. 発見された Issue が記録されている

Issue が残存していてもBuild Loopは完了できる。   
AnchorSpecの目的は既知の問題を管理可能な状態に保つことである。   
既知の問題を失わず、次の開発サイクルへ正しく引き継ぐことである。   

---

Previous : [スレッドモデル](./06_thread_model.md)

Next : [検証モデル](./08_verify_model.md)

Back to : [Index](./index.md)





