<<<<<<< HEAD
# 🧩 Editions

## AnchorSpec Core（Full）

- Intent
- Spec
- Gap
- Current
- Freeze
- Thaw
- Impl
- ImplGap
- Verify

---

### なぜCoreを使うのか（追加推奨）

Core Editionは、**完全な責務分離と再現性を保証するためのAnchorSpec**である。

Intent / Spec / Gap / Impl / Verify に加え、
Freeze / Thaw による状態固定と再現が可能であり、
長期的な開発における整合性と履歴の信頼性を維持することができる。

---

### 用途（追加推奨）
* 本番開発
* チーム開発
* 長期運用プロジェクト
* 高い再現性・監査性が求められる開発

---

### 特徴（追加推奨）
* 全責務を保持（Intent / Spec / Gap / Impl / Verify）
* Freeze / Thaw による履歴固定と再現性
* Strandによる並行Spec管理
* 構造的リカバリが可能
* 最も重いが最も安全な構成

---

## AnchorSpec Lite Edition

### 構成

- Intent
- Spec
- Gap
- Impl
- Verify

---

### なぜLiteを使うのか

Lite Editionは、**最小構成で実装まで到達するためのAnchorSpec**である。

IntentとVerifyを保持することで、
意図からの逸脱や仕様とのズレを検出しつつ、
Implによって実際のアウトプットまで到達することができる。

Coreのような完全な構造は持たないが、
**実行と検証の両立を保ったまま軽量に運用できる**ことを目的とする。

---

### LiteでVerifyを残す理由

Lite EditionにおいてもVerifyは省略されない。

これは、Verifyが単なる検証機能ではなく、
**意図・仕様・実装のズレを検出するための最小限の構造的安全装置**であるためである。

Implを伴う運用において、Verifyを欠いた場合、
仕様との不整合や意図からの逸脱が検出されないまま進行する可能性がある。

Liteは軽量化を目的とした構成であるが、
**構造的破綻を検出する手段を失うことは許容しない。**

そのため、Freezeや履歴管理といった再現性に関わる要素は省略される一方で、
Verifyは最低限の保証として保持される。

---

### 用途

* 小規模開発
* プロトタイピング
* AIとの対話ベース実装
* 中規模未満の継続開発

---

### 特徴

- Intent / Spec / Gap / Impl / Verify を保持
- Freeze / Thaw を持たない
- 実装と検証を同一フロー内で扱う
- Coreに比べて履歴管理および再現性は弱い
- 軽量かつ高速に運用可能

👉 構造を保ちながら軽量化したモード

---

## AnchorSpec Nano Edition

### 構成

- Intent
- Spec
- Gap
- Verify

---

### なぜNanoを使うのか

Nano Editionは、**実装を伴わない設計・検証に特化した最小構成のAnchorSpec**である。

IntentとVerifyを保持することで、
仕様の整合性や意図との一致を確認しながら、
SpecとGapの整理に集中することができる。

Implを持たないため、
**純粋に思考と構造の整合性を扱う用途に最適化されている。**

---

### 用途
* 要件定義
* 仕様設計
* アイデア整理
* 構造検証
* 実装前の検討フェーズ

---

### 特徴
* Intent / Spec / Gap / Verify を保持
* Impl を持たない
* 実装に依存しない設計・検証が可能
* 思考と仕様の整合性維持に特化
* 最も軽量な構造で運用可能

---

## Lite や Nano から Core への段階移行

LiteやNanoからCoreへの段階的な移行は可能である。

しかし、各Editionは責務構造が異なるため、
途中からCoreを導入した場合、過去の履歴・意図・検証状態との整合性を保つことは難しい。
そのため、**移行前のコンテキストは独立したStrandとして扱う**。

これにより、責務構造の異なる履歴を分離し、
Coreにおける整合性を維持したまま運用を継続することができる。

Lite/NanoからCore移行時のStrand化は、標準仕様上は手動で行う。
（ユーザーが新規スレッドを作成し、Strandとして扱うことを明示する）

ただし、実行環境が対応する場合に限り、自動化してもよい。

AnchorSpecの導入においては、最も推奨する方法はCoreを前提とした構造を最初から適用する事である。
一方で、ユーザーの責務および運用方針に応じて、
Lite EditionやNano EditionからCore Editionへの移行も許容される。

また、別タスクで十分に運用に慣れた上で本番に適用することも、
有効な導入手段のひとつである。

---

Lite/NanoからCoreへ移行した場合、
移行前のコンテキストは既存のStrandとして保持される。

Core導入時には、新たにCore構造に基づくStrandが生成される。
この際、移行前のStrandはProtoStrandとはならず、
確定した文脈としてそのまま維持される。

また、移行後は新規に生成されたCoreのStrandが初期選択状態（Selected）となる。

これにより、従来の文脈を保持したまま、
Core構造での運用へ自然に移行することができる。

---

Lite EditionおよびNano Editionでは、Strandを生成することはできない。

StrandはCore Editionにおける構造要素であり、
複数のSpec系統を並行して扱うための機構である。

そのため、Strandを新たに導入した時点で、
当該運用はCore Editionへ移行したものとして扱う。

---

### Edition間(Lite/Nano/Core)のStrand命名規則

Strandの名称は自由に設定できるが、
Edition差分を識別するため、以下のプレフィックスを付与すること。

* nano-
* lite-
* core-

例：

* lite-main
* lite-experiment
* core-main
* core-refactor

---

# 🧠 Coreコンポーネント

---

## Intent

Intentは、システムが達成すべき目的（Why）を定義する最上位の基準である。

Intentは運用フローに属さない独立した要素であり、
いかなるスレッドおよび操作からも変更経路を持たない。

Intentは、Strandの選択（Select）、Specの検証（Verify）などにおいて参照されるが、
それ自体が処理対象となることはない。

Intentの更新はユーザーによる直接編集によってのみ行われる。

IntentはSpecを含むすべての構造の解釈基準となる。

---

## Spec

Specは、AnchorSpecにおける単一の不変な履歴保持主体である。
ここでの不変とは、スレッドのリロードにより常に同一の状態として再現可能であることを指す。

Specは常に成立していなければならず、
未確定・矛盾・仮説的な内容を含んではならない。

すべての変更はCRを経由してのみ反映され、
Approvedな変更のみがSpecに適用される。

Specは履歴としての連続性を保持し、
過去の状態および判断理由（Why）を失ってはならない。

既存のSpecを変更するのではなく、
新しいSpecとして追加されることでのみ更新される。

Specの更新は、ApprovedなCRの適用によってのみ行われる。

Specから既存の履歴を削除または改変してはならない。

FreezeはSpecの特定の時点を不変の参照点として固定する操作である。

各Freezeはバージョンとして扱われ、
以降の変更は新たな履歴として積み重ねられる。

必要に応じて、バージョン単位で独立したスレッドとして管理することができる。

- Source of Truth
- Approvedな変更のみ反映

---

### Why（Lite）の定義（どこまで圧縮するか）

WhyについてはCoreだろうとLiteだろうとDeigneだろうと密度は同じ。(圧縮しない)

---

### SpecとCRの参照関係（ID）

Specは履歴参照のために、CRの参照先情報（ID）を保持してもよいが、CRの完全性はその参照先に依存しない。
Specは、ApprovedとなったCRのIDを履歴参照情報として保持する。

ただし、CRの完全性および再現性は保証されないため、SpecはCRを参照せずとも理解可能であるよう、
Why（判断理由）を自己完結的に保持する必要がある。
Whatは補助情報として保持してもよいが、Specの理解はWhyによって成立することを前提とする。

Specへ転写される履歴情報は、原則としてWhy（判断理由）を中心とする。
Whyは意思決定の根拠として自立して理解可能でなければならず、SpecはCRを参照せずとも成立することを前提とする。

Whatは補助情報として保持してもよいが、Specの理解はWhyによって成立する。

ImpactはCR内の検討情報として扱い、Specの履歴としては保持しない。

AnchorSpecはプラットフォーム非依存を原則とするため、外部永続ストレージを前提としない。
また、AIの揮発性メモリも保存手段として扱わない。
永続変数および半永続変数は、専用のスレッド上で管理される。

CRもその保持先として専用スレッドを用い、必要に応じてThreadから再リードする。
ただし、再リードによって得られるCRは不完全になりうる。
そのため、SpecはCRが失われても意味を保てるよう、Whyを自立した形で保持する。

Whatは補助情報として扱う。

---

### Spec単体で理解できる粒度の保証

 判断理由が1回で理解できる程度

---

## Gap

すべての変化が流入する場所。

- 未定義
- 矛盾
- アイディア
- フィードバック

👉 AnchorSpecの「入口」

---

### GapのActionItem機能

ActionItemは、Gapで扱う差分・議題を追跡可能な単位として管理するための構造である。
ActionItemは、議論の取りこぼしや未処理状態を防ぐためのガードレールとして機能する。

---

#### ActionItemのテンプレートプロンプト

ActionItemは、Gapに流入する差分・議題・アイデアなどから生成される。

テンプレート：
 [ ] 要件、タイトル、問題概要など任意の文字列

---

#### ActionItem Lifecycle

Add → Open → Active → Resolved → Closed → (Optional) Remove

---

### Tips: ActionItemの一括確認

Gapに蓄積されたActionItemは、

Status Index All

のようなコマンドを用いることで一覧確認できる。

これは必須の操作ではないが、
未処理の議題を可視化するために有効である。
(必要に応じて適宜コマンドを定義し使用することができるが、
これはAnchorSpecのサポート外である。)

---

## Current

UI上の「実験サンドボックス」。

- SpecやImplに影響しない
- 正しさ保証なし
- 仮説・検証用

⚠️ 直接採用は禁止

👉 採用または共有する場合は、必ずGapへ昇格すること  
👉 昇格時には「Why（理由）」の明示を必須とする

---

## Impl

Specに基づいて実現された実装状態。

- Specの具体化である
- 正しさは保証されない（Verify対象）
- Currentとは異なり、現実の実装として扱われる
- GapおよびVerifyの比較対象となる

---

## ImplGap

Implにおける未整理の差分・問題・制約・仮説を保持する領域。

- Impl内部の視点で発生する
- Verifyの出力先の一部となる
- 必要に応じてGapへ昇格する
- 直接CRを生成しない

---

## Gapの取り扱いについて

ImplGapは、概念的にはGapに内包することができる。
一方で、GapをSpec側とImpl側に分離して扱うことも可能である。

本仕様では、このいずれかを正解として固定しない。

その理由は明確である。
Gapは本質的に「一時的なもの」であり、いずれ陳腐化する性質を持つためである。

また、Gapの内容はあくまで「差分に関する議論」であり、
どのような議論であっても、その段階では依然としてGapに過ぎない。

したがって、Gapの命名や粒度は、
ユーザーにとって理解しやすい形で自由に定義してよい。

---

### 分離の推奨

Gapは可能な限り分離することが望ましい。

これは、コンテキストドリフトのリスクを最小化するためである。

Gapを細分化することで、議論の焦点が明確になる。
不要な文脈の混入を防ぐ各スレッドの独立性が保たれる。

これこそが、AnchorSpecにおける重要な設計原則のひとつである。

---

### ライフサイクルと整理

ただし、Gapは分離を進めるほど数が増加するという問題を持つ。

そのため、公式の推奨として：

不要になったGap、あるいは陳腐化したGapは、適切に削除することを明示する。

Gapは蓄積するものではなく、役割を終えた時点で整理されるべき一時的な構造である。

---

## CR（Change Request）

CRは単なる変更提案ではない。  

**「仕様変更を安全に通すための中核構造」**である。

👉 Issue + Pull Request のハイブリッド

---

### Status

- Draft：提案段階
- Reviewed：議論済
- Approved：反映可能

⚠️ ApprovedのみSpecへ反映可能

---

### 役割

CRは履歴保持主体ではない。
履歴としての正規な記録はSpecのみが担う

CRはSpecへ変更を適用するための中間構造であり、
Parameter Layer上の一時入力として扱われる。

そのため、CRは完全性および永続性を保証されず、
役割を終えた後は破棄されうる。

- Gapを構造化する
- 変更の正当性を担保する
- Specの破壊を防ぐ

👉 「変更の関所」

---

### ライフサイクル

CRは以下のライフサイクルを持つ：

1. 生成（Generate）
   - GapからCRが生成される

2. 処理（Process）
   - What / Why / Impact を明確化する
   - 議論・整理が行われる

3. 承認（Approve）
   - StateがApprovedになる
   - Specへの反映が可能になる

4. Spec適用（Apply）
   - ApprovedなCRのみがSpecに反映される

5. 破棄（Discard）
   - 役割を終えたCRは保持される必要はない
   - Parameter Layer上の一時入力として扱われる

---

### 構成

CRは以下で構成される：

* ID : 必須
* What : 必須
* Why : 必須
* Impact : 任意
* Feature : 必須
* Discussion : 任意
* State : 必須

---

### 最小構造（Minimal）

CRは以下の最小構造で成立する：

* ID
* What
* Why
* State

---

### 推奨構造（Recommended）

より明確な議論と影響把握のため、以下の構造を推奨する：

* ID
* What
* Why
* Impact（任意）
* Feature
* Discussion（任意）
* State

---

### 構造

- ID : タイトルなどの任意の文字列
- What：何を変更するか  
- Why：なぜ変更するのか  
- Impact：影響範囲  
- Feature：対象機能  
- Discussion：議論ログ  
- State : Draft / Reviewed / Approved / Rejected

---

### Gapとの関係

CRはGapから生成される構造である
* Resolved → CR

---

### Verifyとの関係

* detect / emit gap
* 非破壊
* ActionItem生成可

---

## Verify

VerifyはSpecとImplを比較し、ズレ（差異・不整合・未定義挙動・実現不可など）を検出し、その結果をGapへ出力する非破壊オペレーションである。

VerifyはThreadではなく操作である。
Intent、Spec、Implなどの明示的な参照先に対して、対象が整合しているかを確認するために用いられる。
その役割は承認や書き換えではなく、ズレ・曖昧さ・カバレッジ不足を明らかにすることである。

**「構造が破綻していないか」を検査するレイヤー**である。

Verifyは、対象が基準に対して整合しているか、どこにずれがあるかを確認する作業である。
Verifyは、対象が参照先と競合しているかを確認し、ずれを明示するための操作である。

👉 修正はしない、検出のみ

Verifyが普通の"検証"と違うのは、AnchorSpecには参照先になり得るものが複数ある点。
つまり、Verify は
- Impl <-> Spec
- Spec <-> Intent
- Proposal <-> Current Intent
- Coverage <-> Spec / Impl / Requirement

---

### Input
- target:検証したい対象
- reference:比較基準
- criteria:必要なら観点や条件

### Output
- aligned / mismatched / unknown
- mismatch list
- open questions

---

### 非責務
- Verifyは対象を書き換えない
- Verifyは単独で承認を意味しない
- Verifyは整合または不整合を明らかにする

---

### Verify種類

- Spec Verify：仕様がIntentやCR、あるいは上位方針と整合しているか確認する  
- Impl Verify：実装がSpecと整合しているか確認する
- Intent Verify：現在の方針や提案がIntentから逸脱していないか確認する
- Coverage Verify：仕様・実装・要求の対応に抜け漏れがないか確認する

---

#### verifyのレコード構造

 verify spec against intent
 verify impl against spec
 verify coverage of impl against spec
 verify current proposal against intent
 
---

### 役割

- 仕様崩壊の防止
- 意図逸脱の検出
- 実装暴走の抑制

Verifyは、対象が明示的な参照先と整合しているかを確認し、そのズレを可視化するための操作である。

---

### 運用ルール

- 問題を見つけても直接修正しない
- 必ずGapへフィードバックする

---

### フィードバックフロー

Verifyの結果は、直接修正に用いてはならない。

検出された不整合・疑問・未定義挙動は、
すべてGapへフィードバックされる。

Gapでは、それらの内容をもとに必要に応じてCRが生成される。

CRは整理・承認を経て、Specへ反映される。

このフローにより、Verifyは検出のみを担い、
修正責務はGapおよびCRへ分離される。

---

### 出力

- 不整合レポート
- Gap提案

👉 Verifyは「修正しない品質保証レイヤー」

---

## Freeze

Freezeは、SpecやImplなどの状態を特定の時点で固定し、参照可能な形で保存する操作である。

- Version Lock
- 履歴確定
- Spec/Implの状態を不変として固定する

---

### 本質

- 状態の固定
- 参照点の生成

つまり Freeze は
 「この時点を基準にしていいよ」
を作る操作。

---

### 非責務

- Freeze は変更を禁止するわけではない(新しい状態は作れる)
- Freeze は差分を管理しない(それはGap/CRの役割)
- Freeze は選択を行わない(それはSelect)

---

## Thaw

FreezeされたSpecを再びImpl可能な状態に展開すること。

- Gitにおけるcheckout相当
- Strand / Selectとは別軸の操作（分岐ではない）
- Freezeの解除ではなく、**参照・再起動**
- FreezeされたSpec自体は変更されない（履歴は不変）

👉 過去バージョンのSpecをベースに新たなImplを開始できる

---

## ImplContext

Specを実装に落とし込むための環境定義。

- Language（例: TypeScript / Python）
- Framework（例: React / FastAPI）
- Runtime（例: Node / Browser）
- Constraints（制約条件）
- Non-Functional Requirements（性能・セキュリティ等）

---

## 🆕 Parallel Model

---

### Operationの扱い

Strandに対する操作は、コマンドとしては定義されない。

Select / Splice / Register / Separate などは、
いずれもAnchorSpecにおけるOperationであり、
構造や状態に作用する概念的操作として扱われる。

これらはユーザーの意思決定および文脈操作として実現され、
特定のコマンド入力形式を前提としない。

また、Operationは単独で実行されるものではなく、
文脈上の変化や判断の結果として適用される。

---

### Separate

Separateは、既存の前提系から独立した文脈として扱うことを決定する操作である。

Separateが行われた時点で、当該文脈は新たなStrandとして成立する。

Separateは専用のスレッド生成機構を持たず、
実行環境において新たな文脈を開始し、それを独立した前提系として扱うことにより実現される。

Separateは、実行環境において新たな文脈を開始し、それを独立した前提系として扱うことにより実現される。
このとき、手動で開始された文脈はSeparateの実行と同時にStrandとして成立する。
したがって、Separateにおいては手動作成と論理登録を分離しない。

#### 振る舞い

* Separateは状態遷移イベントである
* StrandはSeparateの結果として成立する
* SeparateはProtoStrandを生成しない
* SeparateはRegisterを必要としない

#### トリガー条件

以下のいずれかに該当する場合、Separateを行う：

* 既存の前提系と両立しない方針を採用する場合
* 並行して複数の解を保持する必要がある場合
* Editionの責務構造が変化する場合（Lite/Nano → Core）

**これは「コマンド」ではなく「Operation」である**

---

### Strand

**互いに矛盾し得るSpecを安全に共存させるコンテナ。**

- 各Strandは独立したSpecを保持する
- 各Strandは独立して状態遷移を進行する
- Strandはマージしない。収束はSelectで行う

StrandはIntentを参照してSelectされるが、
StrandがIntentを書き換えることはできない。

IntentはThread Layerに属する独立した状態であり、
Operation Layerの操作は入力を書き換えない非破壊操作として定義されているため、
StrandからIntentへの影響は物理的に発生しない。

**これは「コマンド」ではなく「Operation」である**

---

### ProtoStrand

**ProtoStrand** は、Spliceによって生成されるStrand候補である。
ProtoStrandは参照ベースで構成された未登録の文脈であり、SelectおよびRegisterを経るまで正式なStrandとはみなされない。

### Characteristics
* Strand候補である
* 未登録状態である
* 参照由来の統合結果を保持できる
* Select対象になれる

**これは「コマンド」ではなく「Operation」である**

---

### Splice

Splice は、複数のStrandを参照し、それらをもとに新たな文脈候補を構成する非破壊操作である。
Spliceの結果として生成されるものは ProtoStrand であり、この時点ではまだ正式なStrandではない。

#### Responsibility
* 既存の複数Strandを参照する
* 参照結果をもとに新たな文脈候補を構成する
* ProtoStrandを生成する

#### Non-Responsibility
* 既存Strandを書き換えない
* 既存Strand同士をマージしない
* ProtoStrandを自動的にStrandへ昇格させない
* Active Strandの決定を行わない

#### Structural Rule
- Strand はマージしない
- Splice は非破壊の参照ベース再構成である
- Splice の生成物はProtoStrandである
- ProtoStrandはSelect可能だが、未登録のままでは正式なStrandではない
- ProtoStrandはRegisterによって正式なStrandとなる
- 収束はMergeではなく、Selectによって行う
　👉 収束は「まとめる」ではなく「選ぶ」

**これは「コマンド」ではなく「Operation」である**

---

・入力
　・base strand
　・reference strand(s)
・処理
　・参照を元に統合 (ここは抽象でOK)
・出力
　・new candidate strand (未確定)

---

### Select

**Select** は、複数のStrandまたはProtoStrandの中から、採用対象を選択する操作である。
Selectそれ自体は登録を保証しないが、選択対象がProtoStrandである場合、後続のRegisterを要求する。

#### Responsibility
* StrandまたはProtoStrandを選択する
* 選択対象がProtoStrandである場合、Registerを要求する
* 参照先だけを切り替える
* 選択されなかったStrandは破棄せず保持する
* 選択事実を記録する
* 選択理由および代替案を記録する

#### Non-Responsibility
* Select は統合（マージ）を行わない（それは Splice）
* Select はStrandの状態を変更しない
* Select は選択対象を自動的にStrand化しない
* Select は既存Strandを書き換えない
* Select は状態を削除しない
* Select は確定しない（それは Register）

**これは「コマンド」ではなく「Operation」である**

---

#### Selectレコード構造

SelectレコードにおけるReasonは自己完結していなければならない。

ReasonをIntentへの外部参照のみによって省略してはならない。

たとえば「Reason: Intentに従った」のような記述は、
判断根拠を外部依存にするため、AnchorSpecでは非対応とする。

Reasonは、判断の前提・目的・評価基準を含み、
単体で意思決定の根拠が再構築可能でなければならない。

- Selected: 採用したStrand名
- Reason: 採用理由
- Alternatives: 非採用Strandと理由

👉 収束は「まとめる」ではなく「選ぶ」

---

### Register

**Register** は、選択されたProtoStrandを正式なStrandとして登録する操作である。
Registerによって初めて、Spliceの生成物はAnchorSpec上で選択可能な正式Strandとなる。

#### Responsibility
* ProtoStrandを正式なStrandとして成立させる
* Strandとして識別可能な状態にする
* 後続のSelect対象として扱えるようにする
#### Non-Responsibility
* Splice自体を代替しない
* 既存Strandのマージを行わない

**これは「コマンド」ではなく「Operation」である**

---

# 🤖 AI運用モデル

AIは以下のスレッドで作業を行う。

## 実装AI（Builder）

- Specに基づいて実装
- Implスレッド担当


## 監査AI（Auditor）

- Verify担当
- Spec / Impl / Intentの整合性を検証

---

# 🔁 Execution Flow

## TODOリスト作成例（最短導入例）

1. Intentを記述する  
　何を作るかを1〜3行で決める  

2. GapにFeatureとして登録する  
　例：  
　[ ] Feature-001 TODOリスト作成  

3. Implで実装する  
　とりあえず動くものを作る  

4. Verifyを実行する  
　SpecやIntentとズレていないか確認する  

---

# 🔄 Feedback Loop

- Verify → Gap  
- ImplGap → Gap  
- Current → Gap  

👉 すべてGapに収束

---

# ⚙️ Operations

| 操作 | 説明 |
|:---|:---|
| Gap作成 | 違和感・アイデアの流入 |
| CR昇格 | Gapを変更要求へ変換 |
| CR承認 | Approved状態へ移行 |
| Spec反映 | ApprovedなCRのみ反映 |
| Freeze | Specのバージョン確定 |
| Thaw | Impl用にSpecを解凍 |
| Strand作成 | 並行Specライン生成 |
| Select | Strandの採用決定（統合しない） |
| Verify | 整合性チェック（修正は行わない） |

---

# Git対応

-   Freeze Spec/Impl → tag
-   CR → commit相当（論理的変更単位）
-   Strand → branch（ただしmergeしない）
-   Select → どのbranchを採用するかの記録

---

# 🧭 When to Use

| 状況 | Edition |
|------|--------|
| アイデア整理 / 設計検証 | Nano |
| 個人開発 / 軽量実装 | Lite |
| 本格開発 / 長期運用 | Core |

---

# 🚀 Quick Start（AI向け運用手順）

AnchorSpecの基本的な使い方：

1. Intentを定義する

   - 目的・ゴールを明確化

2. Gapにすべての違和感・アイデアを入れる  

   - 未整理でOK

3. CRで整理する  

   - What / Why / Impact を明確化

4. Specに反映する（Approvedのみ）

   4.5 必要に応じてStrandを作成し並行Specを保持する

   4.6 Selectで採用Strandを決定する（マージしない）

5. Freeze（Spec確定）  

6. ImplContextを定義する

7. Implで実装する

8. 問題があればImplGap → Gapへ戻す

* Verifyは任意のタイミングで実行し、検出結果は必ずGapへフィードバックする。

---

# 🚀 Next

本セクションは本ドキュメントのスコープ内の次ステップのみを扱う。

- GitHub公開
- GUIアプリ化

👉 ここからプロダクトになる

--- 

## ⚠️ Anti-Pattern

### AIに構造を丸投げする

一見効率的に見えるが、  

スレッド構造が意図と乖離し、全体が崩壊する。

---

### Lesson

- 構造は人間が持つべき責任
- AIは補助に徹する
- 収束は「統合」ではなく「選択」で行う
- 選ばれなかったStrandは破棄せず保持する

---

### Strandをマージして統合しようとする

一見効率的に見えるが、  

複数のコンテキストを無理に統合すると前提が歪み、どちらのSpecも破壊される。

---
AnchorSpecはこの失敗から設計されている。
---


=======
# 🧩 Editions

## AnchorSpec Core（Full）

- Intent
- Spec
- Gap
- Current
- Freeze
- Thaw
- Impl
- ImplGap
- Verify

---

### なぜCoreを使うのか（追加推奨）

Core Editionは、**完全な責務分離と再現性を保証するためのAnchorSpec**である。

Intent / Spec / Gap / Impl / Verify に加え、
Freeze / Thaw による状態固定と再現が可能であり、
長期的な開発における整合性と履歴の信頼性を維持することができる。

---

### 用途（追加推奨）
* 本番開発
* チーム開発
* 長期運用プロジェクト
* 高い再現性・監査性が求められる開発

---

### 特徴（追加推奨）
* 全責務を保持（Intent / Spec / Gap / Impl / Verify）
* Freeze / Thaw による履歴固定と再現性
* Strandによる並行Spec管理
* 構造的リカバリが可能
* 最も重いが最も安全な構成

---

## AnchorSpec Lite Edition

### 構成

- Intent
- Spec
- Gap
- Impl
- Verify

---

### なぜLiteを使うのか

Lite Editionは、**最小構成で実装まで到達するためのAnchorSpec**である。

IntentとVerifyを保持することで、
意図からの逸脱や仕様とのズレを検出しつつ、
Implによって実際のアウトプットまで到達することができる。

Coreのような完全な構造は持たないが、
**実行と検証の両立を保ったまま軽量に運用できる**ことを目的とする。

---

### LiteでVerifyを残す理由

Lite EditionにおいてもVerifyは省略されない。

これは、Verifyが単なる検証機能ではなく、
**意図・仕様・実装のズレを検出するための最小限の構造的安全装置**であるためである。

Implを伴う運用において、Verifyを欠いた場合、
仕様との不整合や意図からの逸脱が検出されないまま進行する可能性がある。

Liteは軽量化を目的とした構成であるが、
**構造的破綻を検出する手段を失うことは許容しない。**

そのため、Freezeや履歴管理といった再現性に関わる要素は省略される一方で、
Verifyは最低限の保証として保持される。

---

### 用途

* 小規模開発
* プロトタイピング
* AIとの対話ベース実装
* 中規模未満の継続開発

---

### 特徴

- Intent / Spec / Gap / Impl / Verify を保持
- Freeze / Thaw を持たない
- 実装と検証を同一フロー内で扱う
- Coreに比べて履歴管理および再現性は弱い
- 軽量かつ高速に運用可能

👉 構造を保ちながら軽量化したモード

---

## AnchorSpec Nano Edition

### 構成

- Intent
- Spec
- Gap
- Verify

---

### なぜNanoを使うのか

Nano Editionは、**実装を伴わない設計・検証に特化した最小構成のAnchorSpec**である。

IntentとVerifyを保持することで、
仕様の整合性や意図との一致を確認しながら、
SpecとGapの整理に集中することができる。

Implを持たないため、
**純粋に思考と構造の整合性を扱う用途に最適化されている。**

---

### 用途
* 要件定義
* 仕様設計
* アイデア整理
* 構造検証
* 実装前の検討フェーズ

---

### 特徴
* Intent / Spec / Gap / Verify を保持
* Impl を持たない
* 実装に依存しない設計・検証が可能
* 思考と仕様の整合性維持に特化
* 最も軽量な構造で運用可能

---

## Lite や Nano から Core への段階移行

LiteやNanoからCoreへの段階的な移行は可能である。

しかし、各Editionは責務構造が異なるため、
途中からCoreを導入した場合、過去の履歴・意図・検証状態との整合性を保つことは難しい。
そのため、**移行前のコンテキストは独立したStrandとして扱う**。

これにより、責務構造の異なる履歴を分離し、
Coreにおける整合性を維持したまま運用を継続することができる。

Lite/NanoからCore移行時のStrand化は、標準仕様上は手動で行う。
（ユーザーが新規スレッドを作成し、Strandとして扱うことを明示する）

ただし、実行環境が対応する場合に限り、自動化してもよい。

AnchorSpecの導入においては、最も推奨する方法はCoreを前提とした構造を最初から適用する事である。
一方で、ユーザーの責務および運用方針に応じて、
Lite EditionやNano EditionからCore Editionへの移行も許容される。

また、別タスクで十分に運用に慣れた上で本番に適用することも、
有効な導入手段のひとつである。

---

Lite/NanoからCoreへ移行した場合、
移行前のコンテキストは既存のStrandとして保持される。

Core導入時には、新たにCore構造に基づくStrandが生成される。
この際、移行前のStrandはProtoStrandとはならず、
確定した文脈としてそのまま維持される。

また、移行後は新規に生成されたCoreのStrandが初期選択状態（Selected）となる。

これにより、従来の文脈を保持したまま、
Core構造での運用へ自然に移行することができる。

---

Lite EditionおよびNano Editionでは、Strandを生成することはできない。

StrandはCore Editionにおける構造要素であり、
複数のSpec系統を並行して扱うための機構である。

そのため、Strandを新たに導入した時点で、
当該運用はCore Editionへ移行したものとして扱う。

---

### Edition間(Lite/Nano/Core)のStrand命名規則

Strandの名称は自由に設定できるが、
Edition差分を識別するため、以下のプレフィックスを付与すること。

* nano-
* lite-
* core-

例：

* lite-main
* lite-experiment
* core-main
* core-refactor

---

# 🧠 Coreコンポーネント

---

## Intent

Intentは、システムが達成すべき目的（Why）を定義する最上位の基準である。

Intentは運用フローに属さない独立した要素であり、
いかなるスレッドおよび操作からも変更経路を持たない。

Intentは、Strandの選択（Select）、Specの検証（Verify）などにおいて参照されるが、
それ自体が処理対象となることはない。

Intentの更新はユーザーによる直接編集によってのみ行われる。

IntentはSpecを含むすべての構造の解釈基準となる。

---

## Spec

Specは、AnchorSpecにおける単一の不変な履歴保持主体である。
ここでの不変とは、スレッドのリロードにより常に同一の状態として再現可能であることを指す。

Specは常に成立していなければならず、
未確定・矛盾・仮説的な内容を含んではならない。

すべての変更はCRを経由してのみ反映され、
Approvedな変更のみがSpecに適用される。

Specは履歴としての連続性を保持し、
過去の状態および判断理由（Why）を失ってはならない。

既存のSpecを変更するのではなく、
新しいSpecとして追加されることでのみ更新される。

Specの更新は、ApprovedなCRの適用によってのみ行われる。

Specから既存の履歴を削除または改変してはならない。

FreezeはSpecの特定の時点を不変の参照点として固定する操作である。

各Freezeはバージョンとして扱われ、
以降の変更は新たな履歴として積み重ねられる。

必要に応じて、バージョン単位で独立したスレッドとして管理することができる。

- Source of Truth
- Approvedな変更のみ反映

---

### Why（Lite）の定義（どこまで圧縮するか）

WhyについてはCoreだろうとLiteだろうとDeigneだろうと密度は同じ。(圧縮しない)

---

### SpecとCRの参照関係（ID）

Specは履歴参照のために、CRの参照先情報（ID）を保持してもよいが、CRの完全性はその参照先に依存しない。
Specは、ApprovedとなったCRのIDを履歴参照情報として保持する。

ただし、CRの完全性および再現性は保証されないため、SpecはCRを参照せずとも理解可能であるよう、
Why（判断理由）を自己完結的に保持する必要がある。
Whatは補助情報として保持してもよいが、Specの理解はWhyによって成立することを前提とする。

Specへ転写される履歴情報は、原則としてWhy（判断理由）を中心とする。
Whyは意思決定の根拠として自立して理解可能でなければならず、SpecはCRを参照せずとも成立することを前提とする。

Whatは補助情報として保持してもよいが、Specの理解はWhyによって成立する。

ImpactはCR内の検討情報として扱い、Specの履歴としては保持しない。

AnchorSpecはプラットフォーム非依存を原則とするため、外部永続ストレージを前提としない。
また、AIの揮発性メモリも保存手段として扱わない。
永続変数および半永続変数は、専用のスレッド上で管理される。

CRもその保持先として専用スレッドを用い、必要に応じてThreadから再リードする。
ただし、再リードによって得られるCRは不完全になりうる。
そのため、SpecはCRが失われても意味を保てるよう、Whyを自立した形で保持する。

Whatは補助情報として扱う。

---

### Spec単体で理解できる粒度の保証

 判断理由が1回で理解できる程度

---

## Gap

すべての変化が流入する場所。

- 未定義
- 矛盾
- アイディア
- フィードバック

👉 AnchorSpecの「入口」

---

### GapのActionItem機能

ActionItemは、Gapで扱う差分・議題を追跡可能な単位として管理するための構造である。
ActionItemは、議論の取りこぼしや未処理状態を防ぐためのガードレールとして機能する。

---

#### ActionItemのテンプレートプロンプト

ActionItemは、Gapに流入する差分・議題・アイデアなどから生成される。

テンプレート：
 [ ] 要件、タイトル、問題概要など任意の文字列

---

#### ActionItem Lifecycle

Add → Open → Active → Resolved → Closed → (Optional) Remove

---

### Tips: ActionItemの一括確認

Gapに蓄積されたActionItemは、

Status Index All

のようなコマンドを用いることで一覧確認できる。

これは必須の操作ではないが、
未処理の議題を可視化するために有効である。
(必要に応じて適宜コマンドを定義し使用することができるが、
これはAnchorSpecのサポート外である。)

---

## Current

UI上の「実験サンドボックス」。

- SpecやImplに影響しない
- 正しさ保証なし
- 仮説・検証用

⚠️ 直接採用は禁止

👉 採用または共有する場合は、必ずGapへ昇格すること  
👉 昇格時には「Why（理由）」の明示を必須とする

---

## Impl

Specに基づいて実現された実装状態。

- Specの具体化である
- 正しさは保証されない（Verify対象）
- Currentとは異なり、現実の実装として扱われる
- GapおよびVerifyの比較対象となる

---

## ImplGap

Implにおける未整理の差分・問題・制約・仮説を保持する領域。

- Impl内部の視点で発生する
- Verifyの出力先の一部となる
- 必要に応じてGapへ昇格する
- 直接CRを生成しない

---

## Gapの取り扱いについて

ImplGapは、概念的にはGapに内包することができる。
一方で、GapをSpec側とImpl側に分離して扱うことも可能である。

本仕様では、このいずれかを正解として固定しない。

その理由は明確である。
Gapは本質的に「一時的なもの」であり、いずれ陳腐化する性質を持つためである。

また、Gapの内容はあくまで「差分に関する議論」であり、
どのような議論であっても、その段階では依然としてGapに過ぎない。

したがって、Gapの命名や粒度は、
ユーザーにとって理解しやすい形で自由に定義してよい。

---

### 分離の推奨

Gapは可能な限り分離することが望ましい。

これは、コンテキストドリフトのリスクを最小化するためである。

Gapを細分化することで、議論の焦点が明確になる。
不要な文脈の混入を防ぐ各スレッドの独立性が保たれる。

これこそが、AnchorSpecにおける重要な設計原則のひとつである。

---

### ライフサイクルと整理

ただし、Gapは分離を進めるほど数が増加するという問題を持つ。

そのため、公式の推奨として：

不要になったGap、あるいは陳腐化したGapは、適切に削除することを明示する。

Gapは蓄積するものではなく、役割を終えた時点で整理されるべき一時的な構造である。

---

## CR（Change Request）

CRは単なる変更提案ではない。  

**「仕様変更を安全に通すための中核構造」**である。

👉 Issue + Pull Request のハイブリッド

---

### Status

- Draft：提案段階
- Reviewed：議論済
- Approved：反映可能

⚠️ ApprovedのみSpecへ反映可能

---

### 役割

CRは履歴保持主体ではない。
履歴としての正規な記録はSpecのみが担う

CRはSpecへ変更を適用するための中間構造であり、
Parameter Layer上の一時入力として扱われる。

そのため、CRは完全性および永続性を保証されず、
役割を終えた後は破棄されうる。

- Gapを構造化する
- 変更の正当性を担保する
- Specの破壊を防ぐ

👉 「変更の関所」

---

### ライフサイクル

CRは以下のライフサイクルを持つ：

1. 生成（Generate）
   - GapからCRが生成される

2. 処理（Process）
   - What / Why / Impact を明確化する
   - 議論・整理が行われる

3. 承認（Approve）
   - StateがApprovedになる
   - Specへの反映が可能になる

4. Spec適用（Apply）
   - ApprovedなCRのみがSpecに反映される

5. 破棄（Discard）
   - 役割を終えたCRは保持される必要はない
   - Parameter Layer上の一時入力として扱われる

---

### 構成

CRは以下で構成される：

* ID : 必須
* What : 必須
* Why : 必須
* Impact : 任意
* Feature : 必須
* Discussion : 任意
* State : 必須

---

### 最小構造（Minimal）

CRは以下の最小構造で成立する：

* ID
* What
* Why
* State

---

### 推奨構造（Recommended）

より明確な議論と影響把握のため、以下の構造を推奨する：

* ID
* What
* Why
* Impact（任意）
* Feature
* Discussion（任意）
* State

---

### 構造

- ID : タイトルなどの任意の文字列
- What：何を変更するか  
- Why：なぜ変更するのか  
- Impact：影響範囲  
- Feature：対象機能  
- Discussion：議論ログ  
- State : Draft / Reviewed / Approved / Rejected

---

### Gapとの関係

CRはGapから生成される構造である
* Resolved → CR

---

### Verifyとの関係

* detect / emit gap
* 非破壊
* ActionItem生成可

---

## Verify

VerifyはSpecとImplを比較し、ズレ（差異・不整合・未定義挙動・実現不可など）を検出し、その結果をGapへ出力する非破壊オペレーションである。

VerifyはThreadではなく操作である。
Intent、Spec、Implなどの明示的な参照先に対して、対象が整合しているかを確認するために用いられる。
その役割は承認や書き換えではなく、ズレ・曖昧さ・カバレッジ不足を明らかにすることである。

**「構造が破綻していないか」を検査するレイヤー**である。

Verifyは、対象が基準に対して整合しているか、どこにずれがあるかを確認する作業である。
Verifyは、対象が参照先と競合しているかを確認し、ずれを明示するための操作である。

👉 修正はしない、検出のみ

Verifyが普通の"検証"と違うのは、AnchorSpecには参照先になり得るものが複数ある点。
つまり、Verify は
- Impl <-> Spec
- Spec <-> Intent
- Proposal <-> Current Intent
- Coverage <-> Spec / Impl / Requirement

---

### Input
- target:検証したい対象
- reference:比較基準
- criteria:必要なら観点や条件

### Output
- aligned / mismatched / unknown
- mismatch list
- open questions

---

### 非責務
- Verifyは対象を書き換えない
- Verifyは単独で承認を意味しない
- Verifyは整合または不整合を明らかにする

---

### Verify種類

- Spec Verify：仕様がIntentやCR、あるいは上位方針と整合しているか確認する  
- Impl Verify：実装がSpecと整合しているか確認する
- Intent Verify：現在の方針や提案がIntentから逸脱していないか確認する
- Coverage Verify：仕様・実装・要求の対応に抜け漏れがないか確認する

---

#### verifyのレコード構造

 verify spec against intent
 verify impl against spec
 verify coverage of impl against spec
 verify current proposal against intent
 
---

### 役割

- 仕様崩壊の防止
- 意図逸脱の検出
- 実装暴走の抑制

Verifyは、対象が明示的な参照先と整合しているかを確認し、そのズレを可視化するための操作である。

---

### 運用ルール

- 問題を見つけても直接修正しない
- 必ずGapへフィードバックする

---

### フィードバックフロー

Verifyの結果は、直接修正に用いてはならない。

検出された不整合・疑問・未定義挙動は、
すべてGapへフィードバックされる。

Gapでは、それらの内容をもとに必要に応じてCRが生成される。

CRは整理・承認を経て、Specへ反映される。

このフローにより、Verifyは検出のみを担い、
修正責務はGapおよびCRへ分離される。

---

### 出力

- 不整合レポート
- Gap提案

👉 Verifyは「修正しない品質保証レイヤー」

---

## Freeze

Freezeは、SpecやImplなどの状態を特定の時点で固定し、参照可能な形で保存する操作である。

- Version Lock
- 履歴確定
- Spec/Implの状態を不変として固定する

---

### 本質

- 状態の固定
- 参照点の生成

つまり Freeze は
 「この時点を基準にしていいよ」
を作る操作。

---

### 非責務

- Freeze は変更を禁止するわけではない(新しい状態は作れる)
- Freeze は差分を管理しない(それはGap/CRの役割)
- Freeze は選択を行わない(それはSelect)

---

## Thaw

FreezeされたSpecを再びImpl可能な状態に展開すること。

- Gitにおけるcheckout相当
- Strand / Selectとは別軸の操作（分岐ではない）
- Freezeの解除ではなく、**参照・再起動**
- FreezeされたSpec自体は変更されない（履歴は不変）

👉 過去バージョンのSpecをベースに新たなImplを開始できる

---

## ImplContext

Specを実装に落とし込むための環境定義。

- Language（例: TypeScript / Python）
- Framework（例: React / FastAPI）
- Runtime（例: Node / Browser）
- Constraints（制約条件）
- Non-Functional Requirements（性能・セキュリティ等）

---

## 🆕 Parallel Model

---

### Operationの扱い

Strandに対する操作は、コマンドとしては定義されない。

Select / Splice / Register / Separate などは、
いずれもAnchorSpecにおけるOperationであり、
構造や状態に作用する概念的操作として扱われる。

これらはユーザーの意思決定および文脈操作として実現され、
特定のコマンド入力形式を前提としない。

また、Operationは単独で実行されるものではなく、
文脈上の変化や判断の結果として適用される。

---

### Separate

Separateは、既存の前提系から独立した文脈として扱うことを決定する操作である。

Separateが行われた時点で、当該文脈は新たなStrandとして成立する。

Separateは専用のスレッド生成機構を持たず、
実行環境において新たな文脈を開始し、それを独立した前提系として扱うことにより実現される。

Separateは、実行環境において新たな文脈を開始し、それを独立した前提系として扱うことにより実現される。
このとき、手動で開始された文脈はSeparateの実行と同時にStrandとして成立する。
したがって、Separateにおいては手動作成と論理登録を分離しない。

#### 振る舞い

* Separateは状態遷移イベントである
* StrandはSeparateの結果として成立する
* SeparateはProtoStrandを生成しない
* SeparateはRegisterを必要としない

#### トリガー条件

以下のいずれかに該当する場合、Separateを行う：

* 既存の前提系と両立しない方針を採用する場合
* 並行して複数の解を保持する必要がある場合
* Editionの責務構造が変化する場合（Lite/Nano → Core）

**これは「コマンド」ではなく「Operation」である**

---

### Strand

**互いに矛盾し得るSpecを安全に共存させるコンテナ。**

- 各Strandは独立したSpecを保持する
- 各Strandは独立して状態遷移を進行する
- Strandはマージしない。収束はSelectで行う

StrandはIntentを参照してSelectされるが、
StrandがIntentを書き換えることはできない。

IntentはThread Layerに属する独立した状態であり、
Operation Layerの操作は入力を書き換えない非破壊操作として定義されているため、
StrandからIntentへの影響は物理的に発生しない。

**これは「コマンド」ではなく「Operation」である**

---

### ProtoStrand

**ProtoStrand** は、Spliceによって生成されるStrand候補である。
ProtoStrandは参照ベースで構成された未登録の文脈であり、SelectおよびRegisterを経るまで正式なStrandとはみなされない。

### Characteristics
* Strand候補である
* 未登録状態である
* 参照由来の統合結果を保持できる
* Select対象になれる

**これは「コマンド」ではなく「Operation」である**

---

### Splice

Splice は、複数のStrandを参照し、それらをもとに新たな文脈候補を構成する非破壊操作である。
Spliceの結果として生成されるものは ProtoStrand であり、この時点ではまだ正式なStrandではない。

#### Responsibility
* 既存の複数Strandを参照する
* 参照結果をもとに新たな文脈候補を構成する
* ProtoStrandを生成する

#### Non-Responsibility
* 既存Strandを書き換えない
* 既存Strand同士をマージしない
* ProtoStrandを自動的にStrandへ昇格させない
* Active Strandの決定を行わない

#### Structural Rule
- Strand はマージしない
- Splice は非破壊の参照ベース再構成である
- Splice の生成物はProtoStrandである
- ProtoStrandはSelect可能だが、未登録のままでは正式なStrandではない
- ProtoStrandはRegisterによって正式なStrandとなる
- 収束はMergeではなく、Selectによって行う
　👉 収束は「まとめる」ではなく「選ぶ」

**これは「コマンド」ではなく「Operation」である**

---

・入力
　・base strand
　・reference strand(s)
・処理
　・参照を元に統合 (ここは抽象でOK)
・出力
　・new candidate strand (未確定)

---

### Select

**Select** は、複数のStrandまたはProtoStrandの中から、採用対象を選択する操作である。
Selectそれ自体は登録を保証しないが、選択対象がProtoStrandである場合、後続のRegisterを要求する。

#### Responsibility
* StrandまたはProtoStrandを選択する
* 選択対象がProtoStrandである場合、Registerを要求する
* 参照先だけを切り替える
* 選択されなかったStrandは破棄せず保持する
* 選択事実を記録する
* 選択理由および代替案を記録する

#### Non-Responsibility
* Select は統合（マージ）を行わない（それは Splice）
* Select はStrandの状態を変更しない
* Select は選択対象を自動的にStrand化しない
* Select は既存Strandを書き換えない
* Select は状態を削除しない
* Select は確定しない（それは Register）

**これは「コマンド」ではなく「Operation」である**

---

#### Selectレコード構造

SelectレコードにおけるReasonは自己完結していなければならない。

ReasonをIntentへの外部参照のみによって省略してはならない。

たとえば「Reason: Intentに従った」のような記述は、
判断根拠を外部依存にするため、AnchorSpecでは非対応とする。

Reasonは、判断の前提・目的・評価基準を含み、
単体で意思決定の根拠が再構築可能でなければならない。

- Selected: 採用したStrand名
- Reason: 採用理由
- Alternatives: 非採用Strandと理由

👉 収束は「まとめる」ではなく「選ぶ」

---

### Register

**Register** は、選択されたProtoStrandを正式なStrandとして登録する操作である。
Registerによって初めて、Spliceの生成物はAnchorSpec上で選択可能な正式Strandとなる。

#### Responsibility
* ProtoStrandを正式なStrandとして成立させる
* Strandとして識別可能な状態にする
* 後続のSelect対象として扱えるようにする
#### Non-Responsibility
* Splice自体を代替しない
* 既存Strandのマージを行わない

**これは「コマンド」ではなく「Operation」である**

---

# 🤖 AI運用モデル

AIは以下のスレッドで作業を行う。

## 実装AI（Builder）

- Specに基づいて実装
- Implスレッド担当


## 監査AI（Auditor）

- Verify担当
- Spec / Impl / Intentの整合性を検証

---

# 🔁 Execution Flow

## TODOリスト作成例（最短導入例）

1. Intentを記述する  
　何を作るかを1〜3行で決める  

2. GapにFeatureとして登録する  
　例：  
　[ ] Feature-001 TODOリスト作成  

3. Implで実装する  
　とりあえず動くものを作る  

4. Verifyを実行する  
　SpecやIntentとズレていないか確認する  

---

# 🔄 Feedback Loop

- Verify → Gap  
- ImplGap → Gap  
- Current → Gap  

👉 すべてGapに収束

---

# ⚙️ Operations

| 操作 | 説明 |
|:---|:---|
| Gap作成 | 違和感・アイデアの流入 |
| CR昇格 | Gapを変更要求へ変換 |
| CR承認 | Approved状態へ移行 |
| Spec反映 | ApprovedなCRのみ反映 |
| Freeze | Specのバージョン確定 |
| Thaw | Impl用にSpecを解凍 |
| Strand作成 | 並行Specライン生成 |
| Select | Strandの採用決定（統合しない） |
| Verify | 整合性チェック（修正は行わない） |

---

# Git対応

-   Freeze Spec/Impl → tag
-   CR → commit相当（論理的変更単位）
-   Strand → branch（ただしmergeしない）
-   Select → どのbranchを採用するかの記録

---

# 🧭 When to Use

| 状況 | Edition |
|------|--------|
| アイデア整理 / 設計検証 | Nano |
| 個人開発 / 軽量実装 | Lite |
| 本格開発 / 長期運用 | Core |

---

# 🚀 Quick Start（AI向け運用手順）

AnchorSpecの基本的な使い方：

1. Intentを定義する

   - 目的・ゴールを明確化

2. Gapにすべての違和感・アイデアを入れる  

   - 未整理でOK

3. CRで整理する  

   - What / Why / Impact を明確化

4. Specに反映する（Approvedのみ）

   4.5 必要に応じてStrandを作成し並行Specを保持する

   4.6 Selectで採用Strandを決定する（マージしない）

5. Freeze（Spec確定）  

6. ImplContextを定義する

7. Implで実装する

8. 問題があればImplGap → Gapへ戻す

* Verifyは任意のタイミングで実行し、検出結果は必ずGapへフィードバックする。

---

# 🚀 Next

本セクションは本ドキュメントのスコープ内の次ステップのみを扱う。

- GitHub公開
- GUIアプリ化

👉 ここからプロダクトになる

--- 

## ⚠️ Anti-Pattern

### AIに構造を丸投げする

一見効率的に見えるが、  

スレッド構造が意図と乖離し、全体が崩壊する。

---

### Lesson

- 構造は人間が持つべき責任
- AIは補助に徹する
- 収束は「統合」ではなく「選択」で行う
- 選ばれなかったStrandは破棄せず保持する

---

### Strandをマージして統合しようとする

一見効率的に見えるが、  

複数のコンテキストを無理に統合すると前提が歪み、どちらのSpecも破壊される。

---
AnchorSpecはこの失敗から設計されている。
---


>>>>>>> origin/main
