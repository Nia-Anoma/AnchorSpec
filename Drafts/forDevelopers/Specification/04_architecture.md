# AnchorSpec - Specification
# Architecture

---

# 基本原則

AnchorSpecは  
**思考（Intent）、仕様（Spec）、実装（Impl）**  
を分離し、  
**差分（Gap）と検証（Verify）**  
によって安全に進化させる開発プロトコルである。  

以下により、開発の混乱・破綻・属人化を防ぐ。

- Thread分離
- 意図駆動（Intent Driven）
- 差分管理（Gap / CR（Change Request））
- 検証（Verify）
- 凍結（Freeze）

> AI時代における「仕様迷子」を防ぐための開発プロトコル  
> 「ズレをGapに集めて、ChangeRequestで整理し、Specに反映する」

---

# Source of Truth

AnchorSpec における **Source of Truth** は **Spec** である。  
Specは唯一の正本として扱われる。
他状態は補助情報または一時状態として扱う。   

Intent、Gap、Current、Impl、Parameter、およびその他の状態はSpecの代替とならない。  
Spec以外の状態は不完全、一時的、または文脈依存である場合がある。  

Specの変更は明示的な手順を必要とする。  
**差異は直接Specへ反映されない**。  

**Current は Source of Truth として扱ってはならない**。  

---

## 人間-AIの責務分担

AnchorSpecは**AIと人間が共同で開発を行うためのThread分割型開発手法である**。  
開発対象の**仕様・議論・実装**を役割ごとに分離したThreadで管理する。  
**履歴の保持はSpecの責務**である。  

**Thread構造は最終的に人間によって決定される。**

■ ユーザーが担う：

- Thread構造の生成及び管理
- 構造（Intent、Gap、Spec、Impl、ImplGap）に対する決定権を持つ
- CRの発行及び承認・棄却
- Verify結果の確認及び議論
- GapThreadにおける議題の提案と決定
- 状態遷移オペレーション発行

※ Threadの中でもGapやCurrentは性質上肥大化しやすいため、こまめにThreadを作り変えること。  
※ [WARNING] AIに決定を委ねてはいけない。

■ AIは以下を担う：　　

- Gapの提案
- オペレーションを受けての内部状態遷移
- CRや実装支援などの補助的役割
- 検証（Verify）
- 整合性整理の補助

---

# コアアーキテクチャ

AnchorSpecを構成する主要要素は以下である。   

---

## レイヤーモデル

AnchorSpecの要素はまず、以下のレイヤーに分けられる。   

1. Thread Layer   
　スレッドが属するレイヤー

2. Operation Layer   
　状態遷移を起こす管理コマンドが属するレイヤー

3. Parameter Layer    
　一時的な情報の保持などを行うレイヤー   
　スレッドで実現することも可能だが、外部ファイル(Markdown形式)を使い実現することも可能   
　AnchorSpecでは、Markdown形式を使用する方法を推奨する。
　(WhyをLLMの揮発メモリに委ねることなく、ファイルシステム上で意図して編集しない限り不変を保てるのが理由である。)

---

## スレッド レイヤー　構成要素

Thread Layerに含まれる主要要素は以下である。

- Intent
- Spec
- Gap
- Current
- Impl
- ImplGap(必要に応じて)

---

## Spec Registration Unit

Spec Registration Unit
├─ Feature Spec (F-xxxx)
├─ Rule Spec (R-xxxx)
├─ Definition Spec (D-xxxx)
└─ Constraint Spec (C-xxxx)

SpecはApproved CRを受け取り、Spec Registration Unitとして保持する。   
CRの必須項目はSpec Registration Unitへ必ず継承される。    
Feature SpecはBuild / Thaw / Verifyの主要単位になる。   

---

### Characteristics
* 開発対象そのものを保持する
* 状態として参照される
* 差分・検証・凍結の対象になる
* Source of Truthになり得る要素を含む

---

## オペレーション　レイヤー　構成要素

Operation Layerは以下の管理コマンドにより構成される。   
Operation Layerは他レイヤーのオブジェクトに対する操作コマンド群と分類できる。

Gap-command {   
　emit-cr   
}   

CR-command {   
　review   
　approve   
　reject   
}   

Spec-command {   
　freeze   
　thaw   
　apply_cr   
}   

Verify-command {    
　verify   
}   

オペレーションレイヤーは以下の誓約を持つ   
- GapItem は Spec に直接入らない
- GapItem は CR に変換される
- Rejected CR は Spec に入らない
- Approved CR だけが Spec.apply の入力になれる

---

### Characteristics
* 状態そのものは保持しない
* Thread間の関係や変化を引き起こす
* 入出力や結果を伴う
* 単独では意味を持たず、Thread Layerに作用して意味を持つ

---

## パラメータ　レイヤー　構成要素

パラメータレイヤーはスレッド、または外部ファイル(Markdown形式)に特定の項目を記載したもので構成される。   

現在パラメータは以下の２つがある。   

- ImplContext：実装時の言語やプラットフォームなどの実装に必要な情報   
- CR(Change Request)：Specへの変更リクエストを行う際に荷つような情報   

■ ImplContext   
ターゲットプラットフォームや実装言語など、ビルドに必要な情報を保持する。   

ImplContext {   
　TargetPlatform：ターゲットプラットフォーム情報   
　Build Language：使用言語    
　Option(1～n)：プロジェクトの要求に応じて必要な情報を与える。   
　　　　　　　　この情報はBuild Plan及びImplementationで使用する。    
}   

■ CR(Change Request)   
Change Requestの承認判断、及び承認履歴の管理に使用する。   

CR {   
　Why(必須)   
　Reason(必須)   
　What(必須)  
　Unit(必須) 
　How(任意)   
　Who(任意)
　Impact(任意)
　Discussion(任意)   
}   

Unit : Feature / Rule / Definition / Constraint   

※ パラメータレイヤーにおけるメモリの信頼性は低いです。   
※ スレッド形式で残す場合でも、AIに尋ねて追跡するのは危険です。   
※ 最低限、Whyだけでも別口で保存するなどして履歴を残してください。   
※ Whyだけでも最低限残してほしいですが、必須項目は可能な限り残してください。

■ Build Plan   
AnchorSpecでは、Freezeで不変となったSpecをSource Of Trutuとして、   
実装時はFreezeされたSpecをThawすることでビルド対象とすることが出来る。
    
Build(Generator)に関してAnchorSpecは、ビルドプランという情報をユーザに提示する。   
ユーザはビルドプランを確認の上、ビルドの承認を行う。   

---

### Memory and Trust

Parameterには、AIの会話メモリや外部メモリ機構に保持されるものが含まれる場合がある。   
しかし、それらの記憶は常に永続・完全・可視・制御可能であるとは限らない。   

そのためAnchorSpecでは、   
**AIが覚えていることを前提にParameterを信頼してはならない。**   

記憶に依存するParameterは便利ではあるが、   
それはあくまで補助的キャッシュであり、正式なSource of Truthではない。   

重要なParameterは、必要に応じてThread上に明示的に書き戻し、   
人間とAIの双方から再参照できる状態にすることが望ましい。   

Parameter LayerにおけるRAM的データは、   
スレッドを用いた疑似的な情報保持によって実現される。   

これらのデータは再リード時に完全性が保証されず、   
特にCRにおいては一部情報が欠落した状態で参照される可能性がある。   

CRは構造的には完全なデータとして定義されるが、   
Parameter Layerにおいて参照される際には、   
その再現性および完全性は保証されない。   

不完全なCRはそのまま信頼してはならない。   
必要に応じてGapに再投入し、再構築または再検証を行う。   

---

### Design Principle
* Parameterは運用を助けるが、仕様を代替しない
* AIメモリ上のParameterは不完全である可能性を前提とする
* 重要なParameterは明文化されるまで信頼しない
* 必要なら永続Parameterと一時Parameterを区別して扱う

### Characteristics
* 状態そのものではなく、解釈条件を与える
* Thread / Operationの挙動に影響する
* 永続のものと一時的なものがある
* 暗黙化しやすいため、可視化が推奨される
* Source of Truthではなく、補助条件である

---

## Short Definitions

Parameter Layerは、運用上の前提条件を扱うLayerである。
それらは挙動に影響を与えうるが、見えない依存関係になってはならない。

* Thread Layer: 開発中の実体となる状態・記録を保持するLayer
* Operation Layer: Thread Layer上の状態に作用する操作を定義するLayer
* Parameter Layer: ThreadやOperationの解釈・運用条件を与える補助情報を保持するLayer

---

# Implementation Model

Build Plan は、AIが勝手に実装へ突っ込む前の“発注確認書”である。   

AI「このFreezeSpec群を、こういう依存・制約・検証条件で実装します」   
人間「その計画でOK / ここだけ修正」   
AI「ではImplContextに落として実装します」   
といったやり取りをするフェーズと考えて問題ない。

---

## ビルドプラン

FreezeSpecBuildPlan {
  id: 任意の文字列
  unit: Feature / Rule / Definition / Constraint
  why: なぜ必要か
  what: 何を作るのか
  Source: Build対象となったFreezeSpec
}

---

## ビルド手順

Thaw   
→ Build Planning   
→ Prompt Input   
→ Build Execution   
→ Build Result   
→ Verify   

---

## 実際の流れ

ここでは例として、Freezeされた"F-01234_spec"と"F-05678_spec"を用いて解説する。   
実際の手順は以下のようになる。    

thaw "F-01234_spec"
thaw "F-05678_spec"
↓
"F-01234Spec" と "F-05678Spec" が Active Implementation Target になる。   
**ThawしてFrozenになる**ではなく、**FreezeSpecをThawして、実装対象として開く**が近い。

ビルドまで含めた流れはこのようになる。   

FreezeSpec   
　↓ thaw   
Active Implementation Target   
　↓   
Build Plan 出力   
　↓   
Prompt Input Phase   
　↓   
ImplContext 生成/参照   
　↓   
Build / Implementation   
　↓   
Build Result   
　↓   
Verify   

---

Previous : [問題定義](./03_problem_definition.md)

Next : [スレッド定義](./05_thread_definition.md)

Back to : [Index](./index.md)

