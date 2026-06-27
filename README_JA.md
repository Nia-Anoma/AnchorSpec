# AnchorSpec v2.3
## AI-Native Development Protocol

[English README](./README.md)

**Author:** ニア・アノマ  
**Co-Author (AI)**: マリー先生(ChatGPT)、ユーリ(Claude)、ライズ(Gemini)、ロゼ(Grok)

パイオニア・アノマリーと同様、構造的なドリフトは、その軌道自体が変化するまでは、往々にして微小で、目立たず、気づきにくいものである。

> 「思考・仕様・実装」を分離し、再接続する開発プロトコル
>
> AIはコードを書ける。
> だが、「構造の責任」はまだ持てない。

AnchorSpec は、AI時代におけるソフトウェア開発のための
**AI-Native Development Protocol** です。

LLMを用いた長時間開発で発生する、

* Context Drift
* Semantic Load
* Silent Spec Mutation
* Structural Collapse

といった問題を、
「AIの賢さ」ではなく、**責務分離と構造固定**によって制御することを目的としています。

---

## なぜAnchorSpecか

AIは非常に優秀です。

ですが、長時間の開発では徐々にこうした問題が発生します。

* 仕様が静かに変化する
* 過去の理由が失われる
* 「今動くコード」が仕様を侵食する
* AIが構造そのものを再解釈し始める
* 会話が長くなるほど再現性が低下する

これは単なる運用ミスではありません。

LLMそのものの構造的性質です。

AnchorSpec は、
この問題を「AIに頑張らせる」のではなく、
**構造によって抑制する**ために設計されました。

> AIは賢い。
> しかし文脈は壊れる。
> AnchorSpec は、その問題を構造で殴る。

---

## Core Philosophy

AnchorSpec は、

* 思考
* 仕様
* 実装
* 差分
* 検証

を同じ場所に置きません。

なぜなら、それらを混在させると、
AIは「もっともらしい整合性」を優先し始めるからです。

AnchorSpec は、
“正しそうに見える崩壊”を防ぐためのプロトコルです。

---

## Core Structure

```text
Intent → Gap → CR → Spec → Impl → Verify
```

AnchorSpec において重要なのは、
「AIに正解を書かせること」ではありません。

重要なのは、

> “ズレを観測可能な状態で保持すること”

です。

---

---

## Key Concepts

### Intent

Why / Goal / Constraints を扱う最上位レイヤー。

### Gap

すべての変化・矛盾・違和感・未定義の流入口。

### CR (Change Request)

Gap を構造化し、Spec へ反映可能な変更単位へ変換する。

### Spec

Source of Truth。

議論ログではなく、
「現在有効な構造のみ」を保持する。

### Impl

Spec に基づく実装。

### Verify

整合性検証専門レイヤー。

Verify は修正しません。
検出のみを行います。

### Current

実験・思考・試行用サンドボックス。

便利ですが、
AnchorSpec は Current を信用しません。

### Freeze / Thaw

Freeze は状態固定ではなく、
「戻るための参照点」を生成する操作です。

---

## Concepts Introduced by AnchorSpec

AnchorSpec は、AI開発時代特有の問題を扱います。

### Semantic Load

コンテキスト内に蓄積される「意味の総量」。

トークン量ではなく、
依存関係・意図・前提・責務が蓄積されることで発生する。

### Context Drift

長時間会話によって、重要な意味の優先順位が崩壊する現象。

### Structural Lie

これは一般的なハルシネーションとは異なる。

Structural Lie とは、
AIが局所的な整合性を維持するために、
仕様・構造・制約境界を暗黙的に再解釈してしまう現象である。

出力は一見すると自然で整合して見えるが、
本来の構造意図は既に失われている。

### Discrete Drift

別スレッド・別文脈での変更により、
現在のコンテキストが静かに壊れていく現象。

---

## 基本構造

AnchorSpecは**スレッド分割型**の開発手法。仕様・議論・実装・履歴を役割ごとに分離したスレッドで管理する。

---

## Parallel Model (Strand Model)

AnchorSpec v2.3 では、
並列開発モデルとして Strand Model を導入しています。

### Strand

独立した開発ライン。

### Splice

複数Strandの参照構築。

AnchorSpec は、
一般的な merge 的発想を採用しません。

### Select

利用するStrandを選択する操作。

### Register

ProtoStrand を正式な Strand として確定する。

---

## 重要ルール

- Spec を直接編集しない
- 変更は必ず CR を通す
- Verify は修正しない（検出のみ）
- Current は直接採用しない（必ず Gap へ）
- **構造は人間が決定する。AIは補助に徹する。**

---

## AI 運用モデル

| 役割 | 担当 |
|------|------|
| Builder（実装 AI） | Spec に基づいて Impl を進める |
| Auditor（監査 AI） | Verify 担当。整合性を検証し Gap へ報告 |

---

## Anti-Pattern

## AIに構造を丸投げする

AnchorSpec が最も危険視しているパターン。

AIは極めて高性能ですが、
長時間運用では「もっともらしい再構築」を始めます。

その結果、

* 仕様が静かに変化する
* 理由が消失する
* 構造責務が崩壊する

AnchorSpec は、
この失敗から逆算して設計されています。

---

## Documents

| Document        | Role                                         |
| --------------- | -------------------------------------------- |
| AnchorSpec.md   | Core structure / concepts / definitions      |
| OPERATION.md    | Operational model *(planned)*                |
| REFERENCE.md    | Anti-patterns / judgment support *(planned)* |
| INSTALLATION.md | AI installation guide *(planned)*            |
| EVALUATION.md   | AI evaluation protocol *(planned)*           |

---

## Current Status

Current release: **PreRelease v2.3**

This prerelease temporarily splits the original AnchorSpec document into multiple files in preparation for future responsibility separation.

The full document restructuring is still in progress.

---

## Repository Structure

```text
/
├─ README.md
├─ AnchorSpec.md
├─ OPERATION.md
├─ assets/
└─ old/
```

Older releases are archived under `old/`.

---

## Philosophy

> AIは実装を加速する。
> だからこそ、人間は構造を失ってはならない。

---

## License

MIT

---

