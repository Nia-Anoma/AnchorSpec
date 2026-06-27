# AnchorSpec - Specification
# 状態遷移

---

## スレッドとコマンド

AnchorSpecでは、各スレッドはそれぞれ異なる責務を持つ。   
コマンドは、責務を持つスレッドに対してのみ実行できるものとし、責務を越えて他のスレッドを直接変更してはならない。   
各スレッドで実行可能なコマンドを以下に示す。   

| スレッド | 主な責務 | 実行可能なコマンド |
|---|---|---|
| Intent | プロジェクトの目的を定義する | なし |
| Gap | 仕様化前の課題・提案・議論を管理する | Discuss, Create CR |
| Change Request | 仕様変更案を審査する | Approve, Reject, Apply |
| Spec | 承認済みの仕様を保持する | Apply CR, Freeze |
| FreezeSpec | 凍結された仕様を保持する | Thaw |
| Impl | 仕様に基づいて実装する | Build |
| Verify | 仕様・実装・意図の整合性を検証する | Execute |

---

## Intent の扱い

Intent はプロジェクトの目的を定義する。
Intent の粒度は権限保有者に委ねられる。   
Intent は、仕様変更や実装判断の基準となる。   
Intent は Freeze、Gap、Change Request の対象ではない。   
Intent は、権限保有者の責任を持つ作業者の判断によって更新できる。   
ただし、Intent の変更は Spec 全体の解釈に影響するため、変更理由を明記することが望ましい。   

---

## Specの更新フロー

Spec は承認済みの仕様のみを保持する。   
**仕様変更は Gap で議論され、Change Request の承認後に反映される**。   

Gap   
 │   
Discuss   
 ▼   
Change Request    
 │   
Approve   
 ▼   
Approved   
 │   
Apply   
 ▼   
Spec
 │   
Freeze   
 ▼   
FreezeSpec   
 │   
Thaw   
 ▼   
新しい実装コンテキスト   

**FreezeSpec は変更されない**。   
**Thaw は FreezeSpec を変更するのではなく、新しい作業コンテキストを生成する**。

---

## Change Request の状態遷移

Change Request は仕様変更の候補である。   

Draft    
 │    
Review   
 ▼   
Reviewed   
 │   
Approve / Reject   
 ▼   
Approved / Rejected   
 │   
Apply   
 ▼   
Applied   

Approve / Rejectの判断の前にReviewを行う。
**Rejected は Spec に反映されない**。   
**Approved のみ Apply を実行できる**。   

---

## Impl の状態遷移

Impl は Spec を実装する。   

Impl   
 │   
Build   
 ▼   
Completed   
 │   
Freeze   
 ▼   
BuiltImpl   

BuiltImpl は実装スナップショットであり、変更してはならない。   

---

## Verify の状態遷移

Verify は Spec、Impl、および Intent の整合性を検証する。   

Idle    
 │   
Execute   
 ▼   
Verifying    
 │   
Complete   
 ▼   
Report   

**Verify は問題を検出する責務のみを持つ**。   
**Verify は Spec および BuiltImpl を直接変更してはならない**。   
**検出結果は Gap にフィードバックされる**。

---

## 状態遷移の制約

各状態遷移は定義されたコマンドによってのみ実行される。   

以下の遷移は禁止される。   

- FreezeSpec の直接編集
- Verify による Spec の変更
- Gap を経由しない Spec の変更
- 承認されていない Change Request の適用

すべての変更は、AnchorSpec が定義するライフサイクルに従わなければならない。   

各状態遷移は、対応するコマンドによってのみ実行される。   
状態遷移は責務を持つスレッド内でのみ発生する。   
スレッドを跨ぐ変更は、Workflowで定義された経路を経由しなければならない。   
Freezeされた成果物は変更してはならない。   

---

Previous : [実装モデル](./10_implementation_model.md)

Next : [リファレンス](./12_reference.md)

Back to : [Index](./index.md)
