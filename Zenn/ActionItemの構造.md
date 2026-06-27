<<<<<<< HEAD

---
## Action Itemの構造

Action Itemは、LLM上で動作するコマンドを定義したものである。   
主に以下のコマンドを使用する。   

- Add      :書式 : Add    "議題" : Action Item "議題" を登録する。この際、登録順によってIndexが割り振られる。   
- Open     :書式 : Open   "議題" : Action Item "議題" を開き議論可能な状態にする。議論がまとまり次第Fixする。   
  - Open   :書式 : Open   Index  0 : Action Item を Index で指定して開き議論可能な状態にする。議論がまとまり次第Fixする。   
  - Fix    :書式 : Fix    Index  0 : Action Item を Index で指定して閉じる。Closeするまでは再Open可能。   
- Close    :書式 : Close  "議題" : Action Item を Closeする。議題は決定した内容で閉じる。   
  - Close  :書式 : Close  Index  0 : Action Item を Index で指定して Close する。議題は決定した内容で閉じる。   
- Clear    :書式 : Clear  : 登録されているActionItemをすべてクリアする。状態(Open/Fix/Close)などは関係ない。   
- Status   :書式 : Status "議題" : Action Item "議題" の今の状態(Open/Fix/Close)を表示する。    
  - Status :書式 : Status Index  0 : Action Item を Index で指定して今の状態(Open/Fix/Close)を表示する。   

※ Open の Index は番号を直接入力できるが、Nextを使用することで次の議題に移りやすくなる。   
※ 例) Open Index Next : 次に議論しなければならない議題を開く。   

※ Fix / Close の Index は番号を直接入力できるが、Currentを使用することで現在開いている Action Item を Fix / Close 出来る。   
※ 例) Fix Index Current : 現在開いている議論を Fix する。   
※ 例) Close Index Current : 現在開いている議論を Close する。   

---

## 使用例
例)朝ご飯を用意するTODOリスト

Add "味噌汁を作る"
Add "目玉焼きを焼く"
Add "サラダの用意"
Add "味噌汁をよそう"
Add "ご飯をよそう"
※ この時、最初のAddから順にIndexが割り振られている。

Open Index 0
　味噌汁を作る
Fix Index Current
Open Index Next
　目玉焼きを焼く
Fix Index Current
Open Index Next
　サラダを作る
Fix Index Current
Open Index Next
　お椀に味噌汁をよそう
Fix Index Current
Open Index Next
　お茶碗にご飯をよそう
Fix Index Current

Status Index All　←　一応全ActionItemの状態確認

Clear　←　登録されたActionItemをクリアする(Closeはあまり使う機会がない)

---

=======

---
## Action Itemの構造

Action Itemは、LLM上で動作するコマンドを定義したものである。   
主に以下のコマンドを使用する。   

- Add      :書式 : Add    "議題" : Action Item "議題" を登録する。この際、登録順によってIndexが割り振られる。   
- Open     :書式 : Open   "議題" : Action Item "議題" を開き議論可能な状態にする。議論がまとまり次第Fixする。   
  - Open   :書式 : Open   Index  0 : Action Item を Index で指定して開き議論可能な状態にする。議論がまとまり次第Fixする。   
  - Fix    :書式 : Fix    Index  0 : Action Item を Index で指定して閉じる。Closeするまでは再Open可能。   
- Close    :書式 : Close  "議題" : Action Item を Closeする。議題は決定した内容で閉じる。   
  - Close  :書式 : Close  Index  0 : Action Item を Index で指定して Close する。議題は決定した内容で閉じる。   
- Clear    :書式 : Clear  : 登録されているActionItemをすべてクリアする。状態(Open/Fix/Close)などは関係ない。   
- Status   :書式 : Status "議題" : Action Item "議題" の今の状態(Open/Fix/Close)を表示する。    
  - Status :書式 : Status Index  0 : Action Item を Index で指定して今の状態(Open/Fix/Close)を表示する。   

※ Open の Index は番号を直接入力できるが、Nextを使用することで次の議題に移りやすくなる。   
※ 例) Open Index Next : 次に議論しなければならない議題を開く。   

※ Fix / Close の Index は番号を直接入力できるが、Currentを使用することで現在開いている Action Item を Fix / Close 出来る。   
※ 例) Fix Index Current : 現在開いている議論を Fix する。   
※ 例) Close Index Current : 現在開いている議論を Close する。   

---

## 使用例


Add
>>>>>>> origin/main
