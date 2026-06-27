# AnchorSpec - Specification
# Appendix

---

# Purpose

Appendixは、AnchorSpec本体の仕様を補足するための付録である。

ここに記載される内容は理解を支援することを目的とし、
Specificationで定義された規則を変更または上書きしない。

Appendixは以下のような情報を保持できる。

- Deprecated Features
- Experimental Features
- Migration Notes
- Terminology
- Historical Notes

---

# Deprecated Features

以下の機能は過去の仕様として残されるが、
現在のAnchorSpecでは正式仕様ではない。

## Strand

---
> ~~Strand モデル~~ [Deprecated]  
> Strand is Deprecated.  
> Strand関連機能はAnchorSpecの責務外とする。  
> 並列状態管理はプロジェクト戦略または外部ツールで扱う。  
---

> **Deprecated**

Strandは複数の仕様状態を並列管理するために設計された。

しかし現在は、

- プロジェクト戦略
- 外部ツール
- バージョン管理システム

で十分代替可能と判断し、
AnchorSpec Coreの責務から除外した。

既存プロジェクトで利用することは禁止しないが、
今後の仕様ではサポート対象外とする。

---

# Experimental Features

以下は検討中の概念であり、
正式仕様ではない。

これらは将来変更・削除される可能性がある。

---

# Terminology

代表的な略語を示す。

|略語|意味|
|----|----|
|Intent|開発目的|
|Spec|Specification|
|Gap|差分・議論入口|
|CR|Change Request|
|Verify|検証主体|
|FreezeSpec|凍結されたSpecification|
|BuiltImpl|Build済み成果物|
|ImplContext|実装条件|
|Build Plan|実装計画|

---

# Migration Notes

AnchorSpecは今後も改善を続ける。

互換性に影響する変更が発生した場合は、
この章に移行方法を記載する。

---

# Historical Notes

本章には、
設計思想の変遷や過去仕様との違いを記録してもよい。

ただし、
これらは現在のSpecificationを変更するものではない。

---

# Non-Authority

Appendixは以下を行わない。

- Specificationの変更
- Operationの変更
- Governanceの変更
- 新しいルールの定義

Appendixはあくまで補助情報であり、
AnchorSpec本体の仕様はSpecificationを正本とする。

---

Next : [オプションコマンド](./15_appendix_commands.md)

Previous : [制約事項](./13_limitations.md)

Back to : [Index](./index.md)
