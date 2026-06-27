# AnchorSpec - Specification
# Appendix

---

# ActionItem

ActionItemには本来、会議や議論の後に発生する「誰が・何を・いつまでにやるか」   
を明確にした具体的なタスク対応事項のことです。   
Next Actionや実行項目とも呼ばれ、単なるメモと異なり実行可能なレベルまで落とし込まれているのが特徴です。   
AnchorSpecではこのActionItemをコマンド体系化し、様々なことに活用しています。   

---

## ActionItem Commands

ActionItemはGap上の議題・作業・調査事項を管理するための補助コマンド群である。   

ActionItemはAnchorSpecの必須機能ではない。   
必要に応じて自由に拡張・変更してよい。   

---

### Add

新しいActionItemを追加する。
追加したActionItemにはIndexが割り振られ、Index指定で操作を行うことが出来る。   

> Add "Implement Login API"

---

### Open

ActionItemを作業対象として開始する。   

> Open Index 12   

または   

> Open Index Next   

OpenされたActionItemはCurrentとして扱われる。   

---

### Fix

ActionItemを修正・更新する。    

> Fix Index Current

または

> Fix Index 12

説明や内容、タイトルの変更を行う。

---

### Close

ActionItemを完了状態へ変更する。   

> Close Index Current

CloseされたActionItemは一覧には残るが、通常の作業対象から除外される。   

---

### Remove

不要になったActionItemを削除する。   

> Remove Index 12

Removeは履歴管理方針に従い利用することが望ましい。   

---

### Index

ActionItemには一意なIndex番号が割り当てられる。   

[Index:1]   
[Index:2]   
[Index:3]   

IndexはActionItemを指定するための識別子であり、   
Feature IDとは異なる。   

---

### Status

StatusはActionItemの現在状態を表す。   

|Status	 |説明       |
|--------|-----------|
|Open	 |未着手     |
|Current |現在作業中  |
|Fix	 |更新中     |
|Close	 |完了       |
|Remove	 |削除済み    |

※ Status名は運用に応じて追加・変更してよい。   

---

### Status Command

Statusを利用すると、現在のActionItem一覧を取得できる。   

> Status

現在作業中のActionItemを表示する。   

---

> Status Index 12

Index指定で表示する。   

---

> Status Index All

すべてのActionItemを表示する。   

---

> Status Current

Currentのみ表示する。   

---

## Practical Examples

現在作業中の議題を開く。

> Open Index Next

---

現在作業中の議題を修正する。   

> Fix Index Current

---

未処理一覧を表示する。   

> Status Open

---

すべてのActionItemを表示する。   

> Status Index All

---

完了したら閉じる。   

> Close Index Current

---

## AnchorSpec内でのActionItem機能

ActionItemは、Gapで扱う差分・議題を追跡可能な単位として管理する構造として使うことが出来る。   
ActionItemは、議論の取りこぼしや未処理状態を防ぐためのガードレールとして機能する。    

---

## ActionItemのテンプレートプロンプト

ActionItemは、Gapに流入する差分・議題・アイデアなどから生成される。   

テンプレート：   
 [ ] 要件、タイトル、問題概要など任意の文字列   

---

## ActionItem Lifecycle

Add → Open → Active → Resolved → Closed → (Optional) Remove   

---

## AnchorSpecでのGap運用のActionItemでの状態変化
 - ActionItem：議題の状態管理。Store/Gap/Reject/Apploval。Applovalの場合のみ仕様に昇格する。
 - Lifecycle：議論の開始/議論の再構築/議論の再評価/帰結。などの履歴保存は何より優先される。
 - Feature ID：Gapから生成されたCRのライフサイクルである。ApplyすればSpec昇格。それ以外の判断はGapへ戻される・

---

これらのコマンドは推奨例であり、プロジェクトに応じて自由に追加・変更・削除してよい。   
AnchorSpecはActionItemコマンド群の実装および利用を必須とはしない。   

---

Previous : [制約事項](./14_appendix.md)

Back to : [Index](./index.md)
