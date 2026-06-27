# AnchorSpec - Specification
# Implementation Model

---

# Purpose

**Implementationは、FreezeSpecおよびBuild Planに基づいて実装を生成するフェーズである**。   
Implementationの責務は、**Build Planで定義された内容を忠実に実装へ反映すること**である。   
**ImplementationはSpecを変更しない**。

---

# Input

Implementationは以下を入力として使用する。   

- FreezeSpec
- Build Plan
- ImplContext

ImplementationはBuild Planに従って実装を行う。   

---

# Output

Implementationは以下を生成する。  

- Source Code
- Configuration
- Build Artifacts
- BuiltImpl

ImplementationはBuild完了後、Verifyへ引き渡される。   

---

# Comment Rule

AnchorSpecでは、**生成されるコードに対して必要に応じてWhyおよびReasonを保持することを推奨**する。   
**コメントはコードの動作説明ではなく、意思決定理由を保持するために用いる**。

例：

```cpp
// Why:
// このアルゴリズムはリアルタイム性能を優先するため
//
// Reason:
// O(n²)よりもメモリ消費を許容し
// O(n log n)を採用する

```

**Whyは「なぜその実装を選択したか」を表す**。   
**Reasonは、その判断根拠や背景を表す**。   
**コメントはBuild Planに基づいて生成されることが望ましい**。   

---

# Build Plan
    
AnchorSpecでは、Build時にビルドプランをAIに提出させる。   
ユーザはビルドプランを確認の上、ビルドの承認を行う。   
この対応により、Source of Truth の整合や、Verifyの評価軸がブレていないかを確認することが出来る。   

ビルドプランには、以下の情報が書き込まれる。   

- target-spec : thawで解凍済みのSpec(Active Implementation Target)。複数指定可能。   
- ref-context : ImplContext   
- options : link_library, input_binary, target_files, forbidden_files, verification.   
- expected_output :
  - modified files
  - generated files
  - build artifacts
- risk_notes :
  - F-01234Spec and F-05678Spec may conflict in API boundary
  - implementation feasibility is not guaranteed by FreezeSpec

---

# Non-Authority

Implementationは以下を行わない。   

- Specの変更
- Intentの変更
- CRの承認
- Verifyの代替

ImplementationはSpecを実現する責務のみを持つ。   

---

Previous : [Freezeモデル](./09_freeze_model.md)

Next : [リファレンス](./10_reference.md)

Back to : [Index](./index.md)