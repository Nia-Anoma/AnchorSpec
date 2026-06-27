# AnchorSpec - Specification
# Freezeモデル

---

## Freeze Model

Freezeは、Specの特定時点を変更不能な参照点として固定する操作である。   
**FreezeされたSpec（FreezeSpec）は、実装・検証・レビューにおける共通の基準として扱われる**。    
Freezeは変更を禁止するためではなく、同一の基準を複数の工程で共有するために存在する。   

---

## Freeze Purpose

Freezeは以下を目的とする。   

- 実装対象を固定する
- 検証対象を固定する
- レビュー対象を固定する
- 再現可能な基準を保持する
- 仕様ドリフトを防ぐ

**FreezeはSource of Truthのスナップショットであり**、以降の変更は新しい履歴として扱われる。    

---

## FreezeSpec

FreezeSpecはFreeze時点のSpecを表す。   
**FreezeSpecは変更してはならない。**   
新しい要求や改善案が発生した場合でも、FreezeSpecを直接編集することは認められない。   

必要な変更はGapおよびCRを経由して、新しいSpecとして管理される。   

---

## Thaw

FreezeSpecはそのままでは実装対象にならない。   
**実装を開始する際はFreezeSpecをThawし、現在のImplContextを適用した作業コンテキストを生成する**。   
ThawはFreezeSpecを変更する操作ではない。    
Thawによって生成される作業状態は一時的なものであり、FreezeSpecは常に不変のまま保持される。    

---

## Freeze Relationship

Freezeは以下のライフサイクルに属する。   

Spec   
　↓    
Freeze   
　↓   
FreezeSpec   
　↓   
Thaw    
　↓   
Build Plan   
　↓   
Impl    
　↓   
BuiltImpl   
　↓   
Verify   

FreezeはBuild Loopの開始点であり、VerifyはFreezeSpecを基準として整合性を確認する。   

---

## Freeze Guarantees

Freezeは以下を保証する。　　　

- 実装基準の固定
- 検証基準の固定
- レビュー基準の固定
- 再現可能な参照点

---

## Freeze Non-Guarantees

Freezeは以下を保証しない。　　　

- 実装の正しさ
- ビルド成功
- Runtime Confirmation
- ユーザー要求の妥当性
- 将来変更の禁止

Freezeはあくまで参照点を固定する操作であり、品質や正しさそのものを保証するものではない。　　　

---

Previous : [検証モデル](./08_verify_model.md)

Next : [実装モデル](./10_implementation_model.md)

Back to : [Index](./index.md)

