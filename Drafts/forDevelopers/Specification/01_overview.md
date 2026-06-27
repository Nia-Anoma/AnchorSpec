# AnchorSpec - Specification
# Overview

---

# 概要

> AnchorSpecは思考・仕様・実装を分離し再接続する開発プロトコルである  
> AIはコードを書く。だが、「構造に責任」は持たない

**AnchorSpec** は、AI時代におけるソフトウェア開発のための **AI-Native Development Protocol**である。  
共著に個体名を与えたAI達を据える、真のAI-Native世代によるドキュメントかもしれない。

---

# 目的

AIは非常に優秀だ。  
これらはLLMの長期利用時に発生する構造的問題である。  
AnchorSpecでは、その一部をContext・driftとして扱う。  

- 仕様が静かに変化する
- 過去の理由が失われる
- 「今動くコード」が仕様を侵食する
- AIが構造そのものを再解釈し始める
- 会話が長くなるほど再現性が低下する

これは単なる運用ミスではない。  
LLMそのものの構造的性質である。  
AnchorSpec では、このような問題を**Context・drift**と定義している。

AnchorSpec は、LLMを使った長時間開発で発生する以下の問題を**責務分離と構造固定**によって制御することを目的としている。  

- Contextの崩壊
- 意味的負荷による仕様の不整合
- 思いつきによる仕様の破壊
- 仕様変遷の追跡困難

AnchorSpecはこれらを解決するために、以下を提供する。

-   Threadの役割分離
-   変更プロセスの強制
-   履歴の明確化

---

## AnchorSpec Core（Full）

AnchorSpecのフルスペック版では以下のスレッドが必須となる。   

- Intent
- Spec
- Gap
- Current(Sandbox)
- Impl
- ImplGap

---

## AnchorSpec Lite Edition

AnchorSpecの軽量版では以下のスレッドが必須となる。   

- Intent
- Spec
- Gap
- Impl

---

## AnchorSpec Nano Edition

AnchorSpecの最小版では以下のスレッドが必須となる。   

- Intent
- Spec
- Gap

---

## Lite や Nano から Core への段階移行

LiteやNanoからCoreへの段階的な移行は可能である。   

しかし、各Editionは責務構造が異なるため、   
途中からCoreを導入した場合、過去の履歴・意図・検証状態との整合性を保つことは難しい。   
そのため、**移行前のコンテキストを破棄するか継続するかはプロジェクトの判断に委ねられる**。

AnchorSpecの導入においては、最も推奨する方法はCoreを前提とした構造を最初から適用する事である。
一方で、ユーザーの責務および運用方針に応じて、
Lite EditionやNano EditionからCore Editionへの移行も許容される。

また、別タスクで十分に運用に慣れた上で本番に適用することも、
有効な導入手段のひとつである。

---

Next : [適用範囲](./02_scope.md)

Back to: [Index](./index.md)
