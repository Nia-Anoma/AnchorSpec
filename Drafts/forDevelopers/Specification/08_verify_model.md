# AnchorSpec - Specification
# 検証モデル

AnchorSpecにおけるVerifyは、**実装や仕様を修正する主体ではない**。   
Verifyの責務は、Intent・Spec・Implの間に存在する不整合、欠落、逸脱を検出し、その結果を適切な経路へ返すことである。   

- VerifyはSpecを書き換えない。   
- Verifyは実装を修正しない。   
- Verifyは承認を行わない。   

Verifyはあくまで「検出」と「記録」のレイヤーであり、修正・承認・運用判断は別の責務として扱われる。  

---

## Verify Role

Verifyは、**AnchorSpecにおける検証主体**である。   
その役割は、仕様・実装・意図の整合性を確認し、既知の問題を発見可能な状態にすることである。   

Verifyは以下を責務とする。   

- SpecとImplの不整合を検出する
- IntentとSpecの逸脱を検出する
- 実装漏れや検証漏れを検出する
- 検出結果をIssue、Gap、またはReview対象として返す

Verifyは問題を検出するが、問題を解決する主体ではない。   
解決のための修正、承認、仕様変更は他の責務として扱われる。   

---

## Verify Inputs

Verifyは以下の情報を入力として参照する。   

- Intent
- Active Spec または FreezeされたSpec
- Impl または Build成果物
- ImplContext
- Build Plan
- 関連するIssue、CR、Review記録

Verifyは、単一の成果物だけを見るのではなく、必要に応じて複数の層を横断して確認する。   
たとえば、実装がSpecを満たしていても、そのSpec自体がIntentから逸脱している可能性がある。   
そのため、VerifyはSpecとImplの一致だけではなく、Intentとの整合性も確認対象に含める。   

---

## Verify Outputs

Verifyの出力はFindingである。   
Findingは、検出された問題、懸念、未解決事項、または確認結果を表す。   

Findingは以下のような状態を持つことができる。   

|状態	    |説明                                  |
|-----------|--------------------------------------|
|Pass	    |問題が検出されなかった                 |
|Warning	|重大ではないが記録すべき懸念がある       |
|Fail	    |修正または再検討が必要な問題が検出された |

VerifyはFindingを返すが、そのFindingをどのように扱うかは内容によって異なる。

- 実装上の不備や未整理の懸念はIssueとして記録される
- Spec変更が必要な内容はGapまたはCRへ返される
- Reviewや追加判断が必要な内容はReview対象として保持される

Verifyは出力先を直接確定する主体ではないが、少なくとも「どの種別の問題であるか」を識別可能でなければならない。   

---

## Verify Scope

Verifyは以下を検証対象とする。   

- Spec内部の矛盾、欠落、曖昧さ
- ImplがSpecを満たしているか
- SpecがIntentから逸脱していないか
- 要求・仕様・実装・検証の間に未カバー領域が存在しないか

一方で、Verifyは以下を保証しない。    

- ビルド成功後の最終的な動作保証
- ユーザー体験の妥当性
- ビジネス上の価値判断
- プロジェクト責任者による採用判断
- 法令・規制・契約への適合性そのもの

これらはRuntime Confirmation、Review、またはプロジェクト責任者の責務として扱われる。   

---

## Verify Types

AnchorSpecでは、Verifyを以下の観点に分類する。   

**Spec Check**

Spec Checkは、Specification内部の矛盾、欠落、曖昧さを検出する。   

たとえば、   

- 同一Feature内で矛盾した要求が存在する
- 必要な制約が明示されていない
- 実装不能な曖昧記述が残っている

といった状態はSpec Checkの対象となる。   

Spec Checkは実装前にも実施可能である。   

---

## Impl Check

Impl Checkは、実装がSpecを満たしているかを確認する。   

たとえば、   

- 実装がSpecに存在する要求を満たしていない
- 実装にSpec外の挙動が混入している
- Build成果物が想定した構成と一致しない

といった状態はImpl Checkの対象となる。   

Impl Checkは、実装完了後のBuild Loopにおいて中心的な役割を持つ。   

---

## Intent Check

Intent Checkは、SpecまたはImplがIntentから逸脱していないかを確認する。   

たとえば、   

- Specは整合しているが、元の目的から外れた方向へ最適化されている
- 実装はSpecを満たしているが、Intent上重要な制約を損なっている

といった状態はIntent Checkの対象となる。   

Intent Checkは、局所的な整合性だけでは見抜けない方向性の逸脱を検出するために存在する。   

---

## Coverage Check

Coverage Checkは、Intent・Spec・Impl・Verifyの間に未カバー領域が存在しないかを確認する。   

たとえば、   

- Intentに存在する要求がSpecへ反映されていない
- Specに存在する要求に対する実装が存在しない
- 実装済み機能に対する検証が行われていない

といった状態はCoverage Checkの対象となる。   

Coverage Checkの目的は、整合しているように見えるが実際には穴が残っている状態を防ぐことである。   

---

## Verify Routing

Verifyで発見されたFindingは、その内容に応じて適切な経路へ返される。   

---

1. Issueへ返す場合   

以下のようなFindingはIssueとして記録される。   

- 実装上の不備
- 未確定の懸念
- 技術的負債
- 将来改善の候補
- 直ちにSpec変更を要しない問題

Issueは、問題を失わず保持するための記録単位である。   

---

2. GapまたはCRへ返す場合

以下のようなFindingはGapまたはCRへ返される。   

- Specそのものの変更が必要である
- Intentとの整合を保つために仕様修正が必要である
- 現在のSpecでは問題を解消できない

   Gapは不足や矛盾の整理に用いられ、必要に応じてCRへ接続される。
   CRは承認を経てSpec変更を行うための正式な変更単位である。

---

3. Review対象として返す場合

以下のようなFindingはReview対象として保持される。   

- 実装ミスか仕様不足かがまだ確定していない
- 優先度や採用可否の判断が必要である
- 仕様変更に進めるべきか追加議論が必要である

Reviewは、Findingを直ちにSpec変更や実装修正へ結び付けず、一度判断を保留するための場として扱われる。   

---

## Verify Non-Authority

Verifyは検証主体であるが、以下の権限を持たない。   

- Specを直接変更する権限
- Implを直接修正する権限
- CRを承認する権限
- Runtime Confirmationを代替する権限
- プロジェクトの最終採用判断を行う権限

Verifyが持つのは、問題を検出し、その結果を記録または返却する権限のみである。   

---

## Verify and Runtime Confirmation

Verifyは、Intent・Spec・Implの整合性を確認する。   
しかし、ビルド成功や整合性確認は、そのまま実運用上の成功を意味しない。   

実際の動作、利用環境での妥当性、ユーザー体験、業務要件への適合などは、Runtime Confirmationによって確認される。   

そのため、VerifyとRuntime Confirmationは明確に分離される。   

- Verifyは整合性を確認する
- Runtime Confirmationは動作と運用上の妥当性を確認する

Runtime Confirmationの責任はプロジェクト責任者側にあり、Verifyはこれを代替しない。   

---

## Verify Loop Integration

VerifyはBuild Loopの一部として実行される。   
Build完了後、VerifyによってFindingが検出された場合、内容に応じて次の経路へ接続される。   

- 実装修正が必要であればBuild Loopへ戻る
- Spec変更が必要であればGapまたはCRへ進む
- 未確定の懸念であればIssueまたはReviewへ送る

このとき、Verifyは修正そのものを行わない。   
Verifyはあくまで問題の所在を明らかにし、適切な次工程へ返すためのレイヤーである。   

AnchorSpecにおいてVerifyは、品質を自動的に保証する魔法ではない。   
Verifyの役割は、既知の問題を検出し、失われない形で次の判断へ接続することである。   

---

Previous : [変更管理モデル](./07_change_management.md)

Next : [Freezeモデル](./09_freeze_model.md)

Back to : [Index](./index.md)
