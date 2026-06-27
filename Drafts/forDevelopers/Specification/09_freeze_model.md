# AnchorSpec - Specification
# Freezeモデル

---

## Reference Model

Referenceは、AnchorSpecを理解し運用するための参照層である。   

- Referenceは、新しい仕様を定義しない。
- Referenceは、新しい運用手順を定義しない。

Referenceの目的は、SpecificationおよびOperationに定義された内容を横断的に整理し、   
利用者が適切な判断を行えるよう支援することである。   

---

## Purpose

Referenceは以下を目的とする。   

- AnchorSpec全体の構造を理解しやすくする
- 関連する概念同士の関係を整理する
- 判断に必要な情報を横断的に提供する
- 利用者が迷わず適切な情報へ到達できるよう支援する

Referenceは仕様そのものではなく、仕様を理解するための案内役である。   

---

## Reference Scope

Referenceには以下のような情報を含めることができる。   

- Concept Map
- Responsibility Matrix
- Routing Table
- Relationship Diagram
- Chapter Map
- Terminology
- FAQ
- Examples

これらはSpecificationを補助するための参照情報であり、独立した規則として扱われない。   

---

## Concept Map

Concept Mapは、AnchorSpecを構成する概念同士の関係を視覚的または論理的に整理する。   

例えば、   

- IntentとSpecの関係
- SpecとCRの関係
- VerifyとIssueの関係
- FreezeとThawの関係

などを整理するために利用される。   

Concept Mapは構造理解を支援するための資料であり、新しい仕様を追加するものではない。   

---

## Responsibility Matrix

Responsibility Matrixは、各オブジェクトまたは各レイヤーの責務を整理する。   

例えば、   

- Verifyは何を担当するのか
- Change Managementは何を担当するのか
- Runtime Confirmationは誰の責務なのか

などを一覧として整理できる。   

Responsibility Matrixは責務を理解するための資料であり、新たな責任範囲を定義するものではない。   

---

## Routing Reference

Routing Referenceは、検出結果や変更要求がどの経路を通るのかを示す。   

例えば、   

- Findingはどこへ返されるか
- CRはどのようにSpecへ昇格するか
- Build LoopはどのようにVerifyへ接続されるか

などの関係を整理する。   

Routing Referenceは処理の流れを説明するものであり、運用手順そのものではない。   

---

## Chapter Map

Chapter Mapは、各章が扱う責務を整理する。   

これにより、   

- どの概念がどの章で定義されているか
- 詳細を確認するためにどこを参照すべきか

を容易に理解できる。   

---

## Examples

Referenceには理解を補助するための例を掲載できる。   

Examplesは、   

- Specificationの利用例
- Operationの利用例
- Verifyの判断例
- Change Managementの利用例

などを示すことができる。   

Examplesは説明を目的とするものであり、仕様そのものではない。   

---

## Non-Authority

Referenceは以下を行わない。   

- Specificationを定義しない
- Operationを定義しない
- Governanceを定義しない
- プロジェクト固有の判断を行わない
- 承認を行わない

Referenceはあくまで既存の仕様と運用を理解するための参照層である。   

---

## Relationship with Other Documents

ReferenceはSpecification、Operation、およびGovernanceを補助する。   

- Specificationは構造と規則を定義する
- Operationは利用手順を定義する
- Governanceは責任・監査・規制との接続を定義する
- Referenceはそれらを横断的に参照・整理する

Reference自身は、新しいルールを生み出さない。   

---

Previous : [検証モデル](./08_verify_model.md)

Next : [状態遷移](./10_state_transition.md)

Back to : [Index](./index.md)
