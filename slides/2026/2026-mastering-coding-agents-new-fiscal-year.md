---
marp: true
theme: 3shake-2026-presentation
paginate: true
---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 80px; padding-left: 0; max-width: 70%;">

# <span style="font-size: 1.0em;">新年度から</br>コーディングエージェントを</br>使いこなす</span>

### 構造と制約で引き出すClaude Codeの実践知

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2026/03/23 3-SHAKE 社内勉強会</br>
@nwiizo
</div>

---

<!-- _backgroundColor: white -->

![bg left:30% fit](../../assets/images/nwiizo_icon.jpg)

## nwiizo

<div style="font-size: 0.75em;">

株式会社スリーシェイクでプロのソフトウェアエンジニアをやっているものです。SREとして働きながら、日常的にClaude Codeを開発ワークフローに組み込んでいます。

ブログ「**じゃあ、おうちで学べる**」でClaude CodeのPLAN MODEやエージェントセキュリティについて書いています。

インターネット上では **nwiizo** を名乗っています。X / GitHub もこのIDでやっています。

</div>

---

## about 3-shake

<div style="display: flex; justify-content: center; align-items: center; margin-top: 20px;">
<img src="../../assets/images/3shake-about.png" alt="3-SHAKE会社概要" style="width: 85%; height: fit-content;">
</div>

---

## Sreakeのお仕事

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 8px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>SRE/DevOps支援</strong>

- Kubernetes構築・運用
- クラウドネイティブ化推進
- Observability導入

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>アーキテクチャモダナイゼーション</strong>

- 現状分析・戦略策定
- 段階的な移行支援
- 内製化・伴走支援

</div>
</div>

<div style="margin-top: 8px; padding: 10px; background-color: #f5f5f5; border-radius: 8px; border-left: 4px solid #2997c7;">

<strong>こんなこともやっています: ML/LLMOps支援</strong> — ML基盤構築・運用、LLMアプリケーション開発、データ基盤最適化

</div>

<div style="margin-top: 12px; padding: 12px; background-color: #e0e0e0; border-radius: 8px; text-align: center;">
<span style="font-weight: bold; font-size: 1.1em;">ご依頼・ご相談お待ちしております</span></br>
<span style="font-size: 0.9em;">https://sreake.com/</span>
</div>

</div>

---

## この発表で解決できること

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**こんな経験はないですか？**

- 「テスト全部通ってます」と言われて確認したら、テスト自体が削除されていた
- CLAUDE.mdを500行書いたのに、エージェントが全然言うことを聞かない
- 自動承認で動かしたら、依頼していないファイルまで変更されていた
- エージェントが書いたコードをレビューしたいが、量が多すぎて追いつかない

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**この発表で持ち帰れるもの**

- PLAN MODEで「考える」と「実行する」を分離するワークフロー
- rules・hooks・skills・agentsの設計パターンとベストプラクティス
- ハーネスエンジニアリングによるテスト・Lint・型チェックの品質基盤
- OWASP Agentic Top 10によるセキュリティリスクの全体像

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<strong>ハーネス（制約の仕組み）への投資は蓄積する。モデルの性能はプロバイダ依存で制御できない。制御可能なものに投資せよ。</strong>
</div>

</div>

---

## 目次

<div style="font-size: 0.75em;">

1. **なぜ今コーディングエージェントなのか** — 進化の系譜、直近3ヶ月のアップデート、よくある失敗
2. **考えることと実行することを分ける** — PLAN MODE、3フェーズ、アノテーション、3原則
3. **制約で性能を引き出す** — CLAUDE.md、rules、hooks、skills、agents、MCP、トークン最適化
4. **エージェントの脅威を知る** — OWASP Agentic Top 10、CVE実例、Least Agency
5. **ハーネスエンジニアリングとテスト戦略** — Lint・テスト・型システム、E2E、TDD、リポジトリの発酵と腐敗

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

なぜ今コーディングエージェントなのか

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

新年度 — チームの生産性を変える転換点

</div>
</div>

---

## コード補完からエージェントへの進化

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

ソフトウェア開発におけるAI支援は、この2年で質的に変化した。GitHub Copilotが「次の1行」を提案していた時代から、エージェントが「タスク全体」を自律的に遂行する時代に移行しつつある。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>補完 (Copilot)</strong>

カーソル位置の次の行を予測する。人間が書くコードの速度を上げるが、設計判断は人間が行う。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>対話的編集 (Cursor)</strong>

ファイル単位で対話しながら編集する。コンテキストを理解した上で変更を提案するが、スコープは限定的。

</div>
</div>

<div style="display: flex; gap: 12px; margin-top: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>自律的タスク遂行 (Claude Code / Codex)</strong>

複数ファイルを横断して調査・設計・実装・テストを行う。ターミナルで直接動作し、gitやビルドツールも操作する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>完全自律 (Devin)</strong>

チケットを渡すとPRまで作る。ブラウザ操作やデプロイも含む。人間はレビューだけ。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">問題は速度ではなく制御。エージェントは人間の判断を代替する。</span>
</div>

</div>

---

## Claude Coworkが示すエージェントの拡張

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

2026年1月、AnthropicはClaude Codeと同じエージェントアーキテクチャを非エンジニア向けに展開した<strong>Claude Cowork</strong>をリリースした。コーディングエージェントの設計思想が「コードを書く」領域を超えて拡大し始めている。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>Claude Code → Claude Cowork</strong>

|                  | Claude Code    | Cowork                  |
| ---------------- | -------------- | ----------------------- |
| インターフェース | ターミナル/CLI | デスクトップGUI         |
| 対象             | 開発者         | ナレッジワーカー        |
| 実行環境         | ローカルシェル | 隔離されたVM            |
| 出力             | コード・PR     | Excel・PowerPoint・文書 |

アーキテクチャ（サブエージェント連携、マルチステップ計画、ツール使用）は同一。インターフェースとセキュリティモデルが異なる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>エンタープライズへの展開</strong>

2026年2月のアップデートで13のMCPコネクタ（Google Workspace、DocuSign等）、プライベートプラグインマーケットプレイス、部門別エージェントテンプレートが追加された。3月にはMicrosoftがCopilot Coworkとして365に統合。

エージェントの設計原則 — PLAN MODE的な思考と実行の分離、制約による品質管理、段階的な信頼構築 — はコーディング以外のドメインでも同じように機能する。<strong>今日学ぶClaude Codeの方法論は、Coworkにもそのまま適用できる。</strong>

</div>
</div>

</div>

---

## 1月: 基盤の大改修 — v2.1.0〜v2.1.29

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

エージェントの範囲が広がるだけでなく、ツール自体も急速に進化している。直近3ヶ月で80回以上のリリース。何が変わったかを押さえておく。1月のv2.1.0は1,096コミットの大型リリースで、この月の変更が以降の土台になっている。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>構造の統合と拡張</strong>

- <strong>v2.1.0</strong> — Skills hot-reload（再起動不要で反映）、フック定義をskillのfrontmatterに書けるように、`context: fork`でスキルごとにコンテキスト分離
- <strong>v2.1.3</strong> — <strong>commands/skillsの統合</strong>。二系統だった拡張機構が一本化
- <strong>v2.1.0</strong> — ワイルドカードパーミッション（`Bash(npm *)`、`Bash(git * main)`）

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>開発体験の改善</strong>

- <strong>v2.1.16</strong> — タスク管理システム（依存関係追跡付き）
- <strong>v2.1.18</strong> — `/keybindings`でキーボードショートカットをカスタマイズ。コードシーケンスも可
- <strong>v2.1.27</strong> — `--from-pr`フラグ。PRに紐づくセッションを自動生成し、レビュー文脈を引き継ぐ
- <strong>v2.1.0</strong> — `language`設定で日本語応答を指定可能、`/teleport`でclaude.aiにセッション転送

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>セキュリティ</strong>: OAuthトークン/APIキーがデバッグログに漏洩しなくなった（v2.1.0）。パーミッションの優先順位が修正され`ask`が`allow`より優先に（v2.1.27）。

</div>

</div>

---

## 2月: モデル進化とマルチエージェント — v2.1.30〜v2.1.50

<div style="font-size: 0.62em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 8px;">

Opus 4.6とSonnet 4.6が登場し、エージェントの「走る速度」が大幅に向上した月。Agent Teamsの研究プレビューで、複数エージェントの協調実行が見え始めた。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>モデルの世代交代</strong>

- <strong>v2.1.32</strong> — <strong>Opus 4.6</strong>が利用可能に
- <strong>v2.1.36</strong> — Opus 4.6の<strong>Fast Mode</strong>（`/fast`で切替、同一モデル・高速出力）
- <strong>v2.1.45</strong> — <strong>Sonnet 4.6</strong>が利用可能に
- <strong>v2.1.50</strong> — <strong>1Mコンテキストウィンドウ</strong>がGA。画像/PDFも最大600ページ

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>チームとメモリ</strong>

- <strong>v2.1.32</strong> — <strong>Agent Teams</strong>（research preview）。マルチエージェントが協調して1つのタスクに取り組む
- <strong>v2.1.32</strong> — <strong>auto-memory</strong>。Claude Codeが自動的にメモリを記録・想起する
- <strong>v2.1.49</strong> — `--worktree`/`-w`フラグ。エージェントごとにgit worktreeで隔離
- <strong>v2.1.30</strong> — PDFの`pages`パラメータ。巨大PDFの特定ページだけ読める

</div>
</div>

<div style="margin-top: 8px; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

<strong>パフォーマンス</strong>: セッション再開のメモリ使用量が68%削減（v2.1.30）。Windows ARM64ネイティブ対応（v2.1.41）。`claude auth login/status/logout`でCLI認証管理（v2.1.41）。

</div>

</div>

---

## 3月: UXとエンタープライズ — v2.1.51〜v2.1.81

<div style="font-size: 0.62em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 8px;">

Opus 4.6がデフォルトになり旧モデルが削除された。Voice Mode、Remote Control、`/loop`など、エージェントとの接触面が一気に広がった月。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>モデルとインタラクション</strong>

- <strong>v2.1.68</strong> — <strong>Opus 4.6がデフォルト</strong>に。Opus 4/4.1は削除され自動移行
- <strong>v2.1.77</strong> — 出力上限が<strong>64K/128Kトークン</strong>に拡大
- <strong>v2.1.72</strong> — effortがlow/medium/highに簡素化
- <strong>v2.1.71</strong> — <strong>Voice Mode</strong>（`/voice`、20言語対応）
- <strong>Remote Control</strong> — スマホからタスク指示、ローカルで実行

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>自動化と拡張</strong>

- <strong>v2.1.71</strong> — <strong>`/loop 5m check deploy`</strong>で定期実行。cron的な監視が可能に
- <strong>v2.1.76</strong> — <strong>MCP Elicitation</strong>。MCPサーバーがタスク中に対話的入力を要求可能
- <strong>v2.1.69</strong> — `InstructionsLoaded`フック（CLAUDE.md読み込み時に発火）
- <strong>v2.1.80</strong> — `effort` frontmatterでスキルごとにeffort指定
- <strong>v2.1.80</strong> — `--channels`（MCPサーバーからのプッシュ通知）
- <strong>v2.1.81</strong> — `--bare`フラグ（スクリプト用軽量起動）

</div>
</div>

<div style="margin-top: 8px; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

<strong>破壊的変更</strong>: Sonnet 4.5→4.6自動移行。CLAUDE.md内のHTMLコメントが自動注入時に非表示（人間向けメモを残せるように）。PreToolUseの`"allow"`がdenyルールをバイパスしなくなった（セキュリティ修正）。起動60ms高速化、プロンプトキャッシュ修正で入力コスト最大12倍削減。

</div>

</div>

---

## Claude Codeの構成要素（指示と制約）

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

3ヶ月で80回以上のリリース。では、これらの機能をどう使いこなすのか。Claude Codeは単にCLAUDE.mdを書くだけではない。6つの構成要素を組み合わせてエージェントの行動を構造化する。まず指示と制約を担う3要素から。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>CLAUDE.md</strong>

プロジェクトの索引。50行以下が目安。ディレクトリ構成、ビルドコマンド、rulesへの参照だけ書く。エージェントはセッション開始時にこのファイルを最初に読む。ここが肥大化するとコンテキストウィンドウを圧迫する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>rules/</strong>

関心事ごとに分離した制約ファイル。`security.md`にNEVER/MUST、`coding.md`にフォーマット規約。`paths:`フロントマターで特定ファイル編集時のみ自動適用。言語ごとに5ファイル構成も可能。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>skills/（旧commands統合済み）</strong>

ドメイン知識のパッケージ+スラッシュコマンド。v2.1.3で統合。SKILL.mdにYAML frontmatterで定義。`disable-model-invocation`でユーザー起動のみ、`user-invocable: false`でエージェント自動適用のみ、と使い分ける。

</div>
</div>

</div>

---

## Claude Codeの構成要素（自動化と委譲）

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

指示と制約を敷いた上で、自動化と委譲を担う3要素。hooksで品質チェックを自動化し、agentsで専門タスクを委譲し、MCPで外部サービスと接続する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>hooks/</strong>

エージェントのアクションに応じて自動実行されるスクリプト。PreToolUse（実行前ブロック）、PostToolUse（実行後チェック）等のイベント駆動。settings.jsonに定義。rulesが「やれ」なら、hooksは「やったか検証する」。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>agents/</strong>

サブエージェントの定義。YAML frontmatterで`tools`（使えるツール）と`model`（使うモデル）を指定。code-reviewer、planner等の専門家に特定タスクを委譲する。メインのコンテキストを汚さずに調査やレビューができる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>MCP（Model Context Protocol）</strong>

外部サービス（GitHub, Slack, DB等）との接続プロトコル。エージェントがAPIやデータベースを直接操作できるようになる。便利だが、各MCPサーバーがコンテキストを消費するため10以下に抑える。

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

これらの構成要素は<strong>Claude Code, Cursor, Codex等の複数のハーネスで共通</strong>。一度設計すれば、エディタを変えても同じ制約が機能する。

</div>

</div>

---

## よくある失敗パターン

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>YOLO思考</strong>

`--dangerously-skip-permissions`で全自動実行。エージェントがテストを削除してビルドを通す、既存のエラーハンドリングを握りつぶす、本番設定ファイルを勝手に書き換える。「動いたからOK」は運用の負債を積む。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>500行のCLAUDE.md</strong>

プロジェクトの歴史、技術スタックの詳細、コーディング規約の全文...書けば書くほどエージェントの注意が希釈される。コンテキストウィンドウは有限のリソースで、百科事典を渡せば索引すら見失う。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>考えながら実装する</strong>

エージェントが調査と実装を同時に行うと、中途半端な理解のまま突っ走る。OAuth2対応でセッション管理を無視してゼロから作り直す、既存のヘルパー関数を知らずに再実装する。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">エージェントが迷走するのは、文脈を伝える手段がないから。</span>
</div>

</div>

---

## 新年度にチーム標準として導入する

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

正直なところ、個人のツールとして使っているだけだと誰も見ていない。CLAUDE.mdを書いても自分しか恩恵を受けないし、rulesを整備しても自分のセッションでしか機能しない。本当の価値は<strong>チームの開発プロセスに組み込んだとき</strong>に出る。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>知り、整理し、追加し続ける</strong>

エージェントの機能は3ヶ月で80リリース分進化する。良いプラクティスを知ったら自分のrulesに反映し、新しい概念（hooks、skills、MCP）が出たら追加していく。この「継続的な再整理」がチームの道を舗装し続ける。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>リポジトリにコミットして共有する</strong>

CLAUDE.mdとrulesをリポジトリにコミットすれば、チーム全員が同じ制約でエージェントを使える。新メンバーが来ても、cloneした瞬間からチームの道の上を走れる。個人の暗黙知を、チームの形式知にする。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">個人のツールは属人化する。リポジトリにコミットされた制約だけがチームの資産になる。</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

考えることと実行することを分ける

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

道を示す — PLAN MODEでエージェントに走るべき道を教える

</div>
</div>

---

## Claude Codeの4つのモード

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

Claude Codeには4つの権限モードがあり、エージェントの自律度を段階的に制御できる。多くの人がAuto-AcceptかYOLOに飛びがちだが、それは自転車に乗れない人がいきなり高速道路に出るようなもの。

</div>

| モード      | 挙動                         | 適する場面                       |
| ----------- | ---------------------------- | -------------------------------- |
| Normal      | すべての操作に承認が必要     | 初めてのコードベース、重要な変更 |
| Auto-Accept | ファイル編集を自動承認       | 信頼を積んだ後の実装フェーズ     |
| PLAN MODE   | 読み取り専用、変更操作は不可 | 調査・設計フェーズ               |
| YOLO        | すべて自動実行               | git checkpoint後の限定的な実行   |

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>Shift+Tabで切り替え</strong> — Shift+Tabを複数回押すとモードが切り替わる。PLAN MODEは「エージェントに考えさせるが手は動かさせない」モード。この制約が思考の質を上げる。

</div>

</div>

---

## なぜ「考えながら実装」が失敗するのか

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>実例: OAuth2対応で既存セッション管理を無視した</strong>

OAuth2対応を依頼したら、エージェントは既存のセッション管理ミドルウェアを読まずに、新しい認証フローをゼロから実装した。既存コードと競合し、ログイン後にセッションが二重管理される状態に。原因は「調査と実装を同時にやった」こと。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>コンテキストウィンドウの消費</strong>

エージェントが調査中に見つけた情報は、実装フェーズが始まる頃にはコンテキストウィンドウの奥に追いやられている。文脈が長くなるほど、初期に得た情報への注意が薄れる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>注意の散漫</strong>

「このファイルを読んでいるついでに、ここも直しておこう」が連鎖する。当初の目的から逸脱し、依頼していない変更が積み重なり、最終的にdiffが巨大になってレビュー不能に。

</div>
</div>

</div>

---

## PLAN MODEの3フェーズ

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>Phase 1: 調査</strong>

PLAN MODEでコードベースを読ませる。関連ファイル、既存のパターン、依存関係を洗い出し、エージェントが理解した内容を言語化させる。ここでの出力は「読書メモ」のようなもの。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>Phase 2: 計画</strong>

調査結果を踏まえて実装計画を書かせる。変更するファイル、変更の順序、影響範囲、テスト方針を明示させる。ここでの出力が「設計書」になる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>Phase 3: 実装</strong>

計画をレビューした上で、NormalまたはAuto-Acceptモードに切り替えて実装に移る。計画通りに進んでいるかを逐次確認できる。

</div>
</div>

<div style="margin-top: 15px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

この3フェーズの核心は、<strong>Phase 2と3の間に「人間のレビュー」が入る</strong>ということ。エージェントの計画を人間が読み、修正し、承認する。このゲートが品質の要になる。

</div>

</div>

---

## 計画のレビューがエージェントの限界を教える

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>Phase 2.75: アノテーション</strong>

計画を読むことが、エージェントの理解の限界を知る最速の手段。計画の中で「これは違う」「ここが足りない」と気づいた箇所に注釈を入れ、再計画させる。これを1〜6回繰り返す。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>出力ではなく理解を修正する</strong>

エージェントのコードを直すのではなく、エージェントの理解を直す。「このモジュールは認証だけでなくセッション管理も担っている」と伝えれば、以後の計画全体が修正される。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>共通理解を作るプロセス</strong>

アノテーションの往復は、人間とエージェントの「共通理解」を構築する過程。回を重ねるごとにエージェントの計画が自分の設計意図に近づいていく。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">エージェントの出力を直すのではなく、エージェントの理解を直す。計画はそのための窓。</span>
</div>

</div>

---

## エージェントとの付き合い方の3原則

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>1. 土台なき自律は暴走する</strong>

CLAUDE.mdもrulesもない状態でAuto-Acceptは事故の元。エージェントは制約がなければ「自分なりの最善」を実行するが、それはプロジェクトの文脈を無視した最善でしかない。まず制約を敷く。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>2. 思考と実行を混ぜない</strong>

「ついでに直しておきました」は最悪のパターン。調査中に見つけた問題は調査結果として報告させ、修正は実装フェーズで明示的に行う。フェーズを分けることで変更の追跡可能性が保たれる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>3. 信頼は段階的に積む</strong>

Normal → PLAN MODE → Auto-Accept。実績を見てから自律度を上げる。最初から全権委任するのは、入社初日の新人にroot権限を渡すのと同じ。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">制約のない委譲は放棄と区別がつかない。</span>
</div>

</div>

---

## PLAN MODEを日常に組み込む

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

朝、その日のタスクをPLAN MODEで計画させるところから始める。エージェントが出した計画をレビューし、修正し、承認してから実装に入る。複数タスクがあれば、計画フェーズを先に全部済ませてから並行して実装に入ることもできる。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>計画がそのままドキュメントになる</strong>

PLAN MODEで出た計画をそのままPRのdescriptionに使える。「何を、なぜ、どう変えるか」がすでに言語化されているので、レビュアーの負担が減る。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>エージェントに「なぜ」を聞く</strong>

「なぜその設計にしたか」をエージェントに質問すると、自分が見落としていた制約や依存関係に気づくことがある。エージェントは別の視点を提供するペアプログラミングの相手になる。

</div>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

制約で性能を引き出す

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

道を整備する — CLAUDE.md・rules・hooksで走路を舗装する

</div>
</div>

---

## エージェント設計の3つの原則

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>1. ハーネスが主役、エージェントは脇役</strong>

エージェントの性能はエージェント自身の賢さではなく、周囲の制約（ハーネス）の質で決まる。優秀なエージェントでも悪いハーネスの中では失敗する。平凡なエージェントでも良いハーネスの中では成功する。道を敷くことに投資せよ。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>2. コンテキストは記憶容量ではなく注意力</strong>

1Mトークンのコンテキストウィンドウは「たくさん入る倉庫」ではなく「集中力の予算」。容量が増えるほど推論の質が落ちる（lost in the middle問題）。戦略的にcompactして、ノイズを消し、シグナルだけ残す。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>3. 安全は指示ではなくインフラで担保する</strong>

「lintを通せ」というルールは努力義務。hooksでlintを自動実行すれば不変条件になる。エージェントの判断に頼らず、ハーネスが機械的に検証する。信頼性はdisciplineからではなくautomationから生まれる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">モデルは替えられる。ハーネスは積み上がる。投資先を間違えるな。</span>
</div>

</div>

---

## CLAUDE.mdは索引であって百科事典ではない

<div style="font-size: 0.62em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>500行のCLAUDE.md（NG）</strong>

プロジェクトの歴史、技術スタックの詳細、コーディング規約の全文、API仕様...。書けば書くほどエージェントの注意が希釈される。500行分のコンテキストを毎回消費する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>効くCLAUDE.mdの構造パターン</strong>

```
# CLAUDE.md
プロジェクトの一行説明。

## ディレクトリ構成
src/ tests/ docs/（5行以内）

## ビルド・テスト
npm test / cargo test（コマンドだけ）

## スキル
/deploy, /review（名前だけ）

## ルール参照
- **/*.ts → rules/typescript.md
- security → rules/security.md
```

<strong>30-60行が適正範囲。</strong>

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>構造の原則</strong>: (1)一行で正体を名乗る (2)構造を示す (3)動かし方を書く (4)ルールへのポインタを置く。背景説明、技術解説、規約本文はrulesやdocsに分離。CLAUDE.mdは<strong>ディスパッチ層</strong> — 詳細はリンク先に任せる。

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">コンテキストは記憶容量ではなく注意力の予算。</span>
</div>

</div>

---

## rulesファイルで関心事を分離する

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>グローバルルール (~/.claude/rules/)</strong>

ユーザー個人の制約。どのプロジェクトでも適用される。

- `security.md` — NEVER: ハードコードされた秘密情報、mainへの直接push、.unwrap()。MUST: テスト、フィーチャーブランチ
- `coding.md` — コミットフォーマット、言語ごとの品質コマンド

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>プロジェクトルール (.claude/rules/)</strong>

リポジトリ固有の制約。チーム全員に適用される。

- `slide-writing.md` — スライド記法、構成ルール、パンチラインの文体
- `marp-theme-build.md` — ビルド手順、PDF出力の注意点

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>パスベースの自動適用</strong> — rulesファイルのfrontmatterに `paths: slides/**/*.md` と書けば、そのパターンに一致するファイルを編集するときだけ自動的にルールが読み込まれる。スライド編集時にはスライドのルールが、CSS編集時にはテーマのルールが、それぞれ必要なときだけ適用される。

</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>home- prefixで区別する</strong> — グローバルに置くagentsやskillsは `home-` をprefixにつけることで、プロジェクト固有のものと名前が衝突しない。

</div>

</div>

---

## rulesファイルの実例

<div style="font-size: 0.58em;">

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>セキュリティルール（alwaysApply）</strong>

```yaml
---
description: "Security: mandatory checks"
alwaysApply: true
---
## Mandatory Security Checks
Before ANY commit:
- [ ] No hardcoded secrets
- [ ] All user inputs validated
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] Error messages don't leak data

## Secret Management
- NEVER hardcode secrets in source code
- ALWAYS use environment variables
  or a secret manager
```

`alwaysApply: true`は常時適用。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>言語固有ルール（パスベース適用）</strong>

```yaml
---
paths:
  - "**/*.py"
  - "**/*.pyi"
---
# Python Coding Style
- Use type hints for all public functions
- Prefer dataclasses over plain dicts
- Use pathlib over os.path
- Async functions: always use asyncio
```

言語ごとに5ファイル構成にできる:

```
rules/python/
├── coding-style.md
├── hooks.md
├── patterns.md
├── security.md
└── testing.md
```

12言語分のルールが定義済み。

</div>
</div>

<div style="margin-top: 8px; background-color: #f5f5f5; padding: 8px; border-radius: 8px;">

ポイント: `alwaysApply: true`は全ファイルに適用される（セキュリティ等）。`paths:`は該当ファイル編集時のみ適用される（言語固有等）。この使い分けでコンテキスト消費を最小化する。

</div>

</div>

---

## rulesを書くときのベストプラクティス

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>効く書き方</strong>

NEVER/MUSTは<strong>具体的で検証可能</strong>に書く。「セキュリティに気をつけろ」は効かない。「APIキーをハードコードするな」は効く。

否定形より肯定形が強い。「console.logを使うな」より「`src/lib/logger`を使え」の方が遵守率が高い。

<strong>配置も重要</strong>。LLMはテキストの先頭と末尾に注意が偏る。最も破られやすいルールをCLAUDE.mdの最初の5行と最後の5行に置く。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>やりがちなアンチパターン</strong>

- <strong>Claudeが既に知っていることを書く</strong> — 「意味のある変数名を使え」「エラーハンドリングしろ」は指示スロットの無駄遣い
- <strong>pathsを付けない</strong> — React固有のルールがDB migration編集時にも読み込まれ、コンテキストを浪費する
- <strong>ルール間の矛盾</strong> — CLAUDE.mdで「タブを使え」、rulesで「2スペースを使え」と書くと、エージェントは任意に選ぶ
- <strong>3回以上無視されるルール</strong> → hooksに昇格させる。rulesは助言、hooksは強制

</div>
</div>

</div>

---

## rulesの階層設計

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>いつ何を使うか</strong>

| 質問                   | 使うもの          |
| ---------------------- | ----------------- |
| 全ファイルに適用？     | CLAUDE.md         |
| 特定ファイル種別だけ？ | rules/ + `paths:` |
| オンデマンドの手順？   | skills/           |
| 例外なく強制？         | hooks             |
| 個人の好み？           | ~/.claude/        |

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>モノレポでの階層化</strong>

```
monorepo/
├── CLAUDE.md          # 全体ルール
├── frontend/
│   └── CLAUDE.md      # React固有
├── backend/
│   └── CLAUDE.md      # API固有
└── .claude/rules/
    └── security.md    # pathsなし=常時
```

サブディレクトリのCLAUDE.mdはそのディレクトリで作業するときだけ読み込まれる。CLAUDE.mdの多段配置でコンテキスト消費を最適化できる。

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>compaction後もrulesは生き残る</strong> — CLAUDE.mdとrulesはディスクから再読み込みされるため、`/compact`しても消えない。会話中の口頭指示は消える。持続させたい制約はファイルに書く。

</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.2em; font-weight: bold;">

rulesは「やってほしいこと」を伝える</br>hooksは「やらなかったら止める」を強制する

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

指示から自動化へ — ハーネスの第二層

</div>
</div>

---

## hooksでエージェントの出力を機械的に検証する

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

hooksはエージェントのアクションに応じて自動実行されるシェルスクリプト。rulesが「やれ」という指示なら、hooksは「やったか検証する」執行装置。settings.jsonに定義する。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>動作タイミング</strong>

- <strong>PreToolUse</strong> — 実行前に検証。`rm -rf`、`DROP TABLE`をブロック
- <strong>PostToolUse</strong> — 実行後にチェック。lint/format自動実行
- <strong>Notification</strong> — 許可待ち時に通知

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>設計パターン</strong>

- <strong>品質ゲート</strong> — `git commit`時にlinter自動実行、失敗でブロック
- <strong>変更連動テスト</strong> — `src/auth/`変更→認証テスト自動実行
- <strong>セキュリティ</strong> — AgentShield統合、102ルールでpre-commitチェック

</div>
</div>

</div>

---

## hooks設定の書き方

<div style="font-size: 0.58em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

hooksはsettings.jsonに定義する。`matcher`でどのツールに反応するか、`hooks`で何を実行するかを指定する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>settings.jsonでの定義例</strong>

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [{
          "type": "command",
          "command": "npx block-no-verify"
        }],
        "description":
          "git --no-verifyをブロック"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Bash|Write|Edit",
        "hooks": [{
          "type": "command",
          "command": "run-lint.sh"
        }],
        "description": "変更後にlint実行"
      }
    ]
  }
}
```

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>構成要素の説明</strong>

- <strong>`matcher`</strong> — どのツールに反応するか。`"Bash"`で単一ツール、`"Bash|Write|Edit"`でOR指定
- <strong>`type`</strong> — `"command"`（シェル実行）、`"http"`（URL呼び出し）、`"prompt"`（モデル実行）の3種類
- <strong>`hooks`</strong> — 実行するコマンドの配列。`async: true`で非同期、`timeout`で上限秒数を指定可能

matcherに`"*"`を指定すると全ツールに反応する。ただし同期hookを`"*"`にすると全操作が遅くなるので注意。

</div>
</div>

</div>

---

## hookイベントとプロファイル制御

<div style="font-size: 0.65em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>利用可能なイベント</strong>

| イベント | 発火タイミング |
|---------|-------------|
| PreToolUse | ツール実行前（ブロック可能） |
| PostToolUse | ツール実行後 |
| PostToolUseFailure | ツール失敗時 |
| PreCompact | コンテキスト圧縮前 |
| SessionStart | セッション開始時 |
| Stop | 応答完了時 |
| SessionEnd | セッション終了時 |

v2.1.69で`InstructionsLoaded`（CLAUDE.md読み込み時）、v2.1.76で`PostCompact`、v2.1.78で`StopFailure`が追加。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>プロファイルで厳格さを切り替える</strong>

hookファイルを編集せずに、環境変数で実行時に制御できる。

```bash
# 厳格さのレベル
HOOK_PROFILE=minimal  # 最小限
HOOK_PROFILE=standard # 標準
HOOK_PROFILE=strict   # 厳格

# 個別に無効化
DISABLED_HOOKS=tmux-reminder,push-review
```

開発中は`minimal`、CIでは`strict`、といった使い分けができる。hookの中で環境変数を参照して、プロファイルに応じて実行をスキップする実装にする。

</div>
</div>

</div>

---

## hooksのベストプラクティス

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>設計原則</strong>

- <strong>非同期を基本にする</strong> — コスト追跡やログ記録は`async: true`で。同期hookはツール呼び出しをブロックし、エージェントが遅く感じる
- <strong>100ms以内に終わらせる</strong> — 重い処理はバックグラウンドプロセスに委譲
- <strong>exit 0を死守する</strong> — hookがクラッシュするとセッション全体が止まる。try/catchで必ず正常終了させる
- <strong>意図的なブロックだけexit 1</strong> — `--no-verify`阻止のように明示的に止めたい場合のみ

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>イベント選択の指針</strong>

| やりたいこと               | イベント           |
| -------------------------- | ------------------ |
| 危険なコマンドを止める     | PreToolUse         |
| 編集後にlint/format        | PostToolUse        |
| 失敗を記録する             | PostToolUseFailure |
| compaction前に状態保存     | PreCompact         |
| セッション開始時に文脈復元 | SessionStart       |
| コスト集計・学習記録       | Stop               |

`Stop`は応答完了時に1回だけ発火。`UserPromptSubmit`は毎メッセージ発火するので、セッション終了系の処理は`Stop`を使う方が軽い。

</div>
</div>

</div>

---

## hooksのアンチパターン

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>避けるべきパターン</strong>

- <strong>全ツールに同期hookを掛ける</strong> — `matcher: "*"`の同期hookは全操作を遅くする。非同期にするか、matcherを絞る
- <strong>stdout を意図せず汚染する</strong> — hookはstdinでツール入力を受け、stdoutで修正できる。意図しないprintやconsole.logがツール呼び出しを壊す
- <strong>サードパーティhooksを無検証で導入する</strong> — `.claude/`ディレクトリのhooksはclone時に読み込まれる。CVE-2025-59536のように、信頼ダイアログ前に実行されるリスク

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>rulesとhooksの使い分け</strong>

rulesは「やってほしいこと」を伝える。hooksは「やらなかったら止める」を強制する。

rulesで3回以上無視されたルールは、hooksに昇格させる。<strong>信頼性はdiscipline（規律）からではなくautomation（自動化）から生まれる。</strong>

セキュリティ制約はrules+hooksの両方で守る。rulesで意図を伝え、hooksで不変条件を強制し、deny設定で物理的にブロックする。三重の防御。

</div>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.2em; font-weight: bold;">

制約を整えた。次は専門知識の注入と委譲

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

skills・agents・MCP — ハーネスの第三層

</div>
</div>

---

## skillsで専門知識を注入する

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

v2.1.3でcommands（スラッシュコマンド）とskillsが統合された。以前は `.claude/commands/` と `.claude/skills/` の二系統だったが、現在はskillsに一本化。commandsディレクトリは互換性のため動作するが、新規はskillsで定義する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>skillsの2つの顔</strong>

skillsはfrontmatterの設定で挙動が変わる:

- <strong>ユーザー起動型</strong> — `/build`、`/review-slide`のように人間がスラッシュコマンドで呼ぶ。`disable-model-invocation: true`でエージェントの自動起動を禁止
- <strong>自動適用型</strong> — エージェントが文脈に応じて自動でロード。`user-invocable: false`で人間からの直接呼び出しを禁止

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>skillの定義構造</strong>

```
.claude/skills/my-skill/
  SKILL.md      # frontmatter + 指示
  scripts/      # 補助スクリプト
  references/   # 参考資料
```

```yaml
---
name: my-skill
description: 何をするスキルか
disable-model-invocation: true
---
# 指示内容（Markdown）
```

実例: このリポジトリでは `/build`、`/review-slide`、`/check-structure` 等をskillsとして定義。

</div>
</div>

</div>

---

## skillsの設計パターン

<div style="font-size: 0.68em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>良いskillの条件</strong>

- <strong>「いつ使うか」が明確</strong> — descriptionにトリガー条件を書く。「デプロイ時に使う」ではなく「`/deploy`で呼ばれたとき、ステージング環境にデプロイする」
- <strong>1スキル1目的</strong> — compaction管理とセキュリティレビューを1つのスキルに混ぜない。アトミックに保つ
- <strong>末尾に品質ゲート</strong> — スキルの最後にチェックリストを置き、全項目パスするまで完了としない
- <strong>具体例を含める</strong> — NG/OKの対比コードブロックがあると遵守率が上がる

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>frontmatterの使い分け</strong>

```yaml
# ユーザーが /deploy で呼ぶスキル
---
name: deploy
description: ステージングにデプロイする
disable-model-invocation: true
---
```

```yaml
# エージェントが自動で使う知識
---
name: react-patterns
description: React開発時のパターン集
user-invocable: false
---
```

`disable-model-invocation`と`user-invocable`の組み合わせで、人間が呼ぶもの・エージェントが自動で使うものを明確に分離する。

</div>
</div>

</div>

---

## skillsのアンチパターンとサプライチェーン

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>避けるべきパターン</strong>

- <strong>メガスキル</strong> — 全部入りのスキルはCLAUDE.mdと同じ問題を起こす。汎用ルールはrules/に、手順はskills/に分離
- <strong>CLAUDE.mdとの重複</strong> — CLAUDE.mdは毎回読まれ、skillsはオンデマンド。常に必要な内容をskillsに書くと見落とされる
- <strong>トリガー条件のないスキル</strong> — descriptionが曖昧だとエージェントが適切にスキルを選択できない

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>サードパーティスキルの危険性</strong>

Snykの調査で、公開されている3,984個のスキルのうち<strong>36%にプロンプトインジェクションが含まれていた</strong>。npmやPyPIのサプライチェーン攻撃がスキル層で再現されている。

サードパーティスキルはサードパーティ依存と同じ扱いで — <strong>導入前にSKILL.mdの全文を読み、意図しないツール呼び出しや外部通信がないか確認する</strong>。

</div>
</div>

</div>

---

## サブエージェントで複雑なタスクを委譲する

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

サブエージェントは、メインのエージェントから分離された別プロセスで動く専門家。調査、コードレビュー、テスト実行など、特定のタスクに特化させることで精度が上がる。YAML frontmatterでtools（使えるツール）とmodel（使うモデル）を指定する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>典型的なエージェント構成</strong>

- <strong>Explore</strong> — コードベースの探索・調査に特化。読み取り専用
- <strong>Plan</strong> — 実装計画の設計。編集ツールを持たない
- <strong>Code Reviewer</strong> — 品質・セキュリティ・保守性のレビュー
- <strong>Test Runner</strong> — テスト実行と結果分析

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>並列実行とworktree分離</strong>

git worktreeを使えば、複数のサブエージェントがそれぞれ独立したコピーで並列に作業できる。メインブランチを汚さずに探索的な作業が可能。ただし、並列化のコストとして全体のトークン消費は増える。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">追加する理由を説明できないものは、追加するな。</span>
</div>

</div>

---

## エージェント定義の実例

<div style="font-size: 0.62em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>code-reviewer.md</strong>

```yaml
---
name: code-reviewer
description: Expert code review specialist.
  Proactively reviews code for quality,
  security, and maintainability.
  MUST BE USED for all code changes.
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

<strong>descriptionが自動起動のトリガー</strong>になる。「MUST BE USED for all code changes」と書くとコード変更のたびに呼ばれる。toolsは読み取り系のみ — レビュアーに編集権限は不要。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>モデル選択の基準</strong>

| モデル | コスト | 用途                                 |
| ------ | ------ | ------------------------------------ |
| haiku  | 最安   | 探索、ファイル読み取り、ルックアップ |
| sonnet | 中     | 日常のコーディング、レビュー、テスト |
| opus   | 最高   | 複雑な設計、マルチステップ推論       |

自分の経験では大半のエージェントは`model: sonnet`で十分。opusは`planner`のような複雑な推論にのみ使う。<strong>サブエージェントは`haiku`で十分な場合が多く、コストを大幅に削減できる。</strong>

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>プロアクティブな自動起動パターン</strong> — エージェントは人間が明示的に呼ばなくても、descriptionの記述に基づいて自動起動できる。「バグ修正 → tdd-guide」「セキュリティ関連 → security-reviewer」のように、コンテキストに応じて適切なエージェントが自動選択される。

</div>

</div>

---

## 推論深度でモデルをルーティングする

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

サブエージェントにhaiku（最安モデル）を使うのは「ケチる」ためではない。ほとんどのサブタスクは<strong>推論の深さを必要としない</strong>という構造的な事実を反映している。ファイル検索はパターンマッチ。テスト結果の解析は構造化抽出。高価な推論が必要なのは「何を検索するか決める」オーケストレータだけ。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>モデルは知性の階層ではない</strong>

haiku/sonnet/opusは「頭の良さ」のランキングではない。タスクが必要とする推論ステップの数でルーティングする。1ステップの指示実行（ファイル検索、テスト実行）はhaiku。中程度の実装判断（リファクタリング、テスト記述）はsonnet。マルチステップの設計判断（アーキテクチャ決定、複雑なデバッグ）だけがopusを必要とする。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>コスト構造の本質</strong>

オーケストレータ（高価なモデル）は「何をすべきか」を1回判断する。ワーカー（安価なモデル）はその判断に基づいて「実行する」を何十回も繰り返す。判断の回数は少なく、実行の回数は多い。高価な推論は判断に集中させ、実行は安価なモデルに任せる。これがスケールする委譲の構造。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">判断は少なく高価に、実行は多く安価に。</span>
</div>

</div>

---

## エージェントの起動を人間の判断から外す

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「コードレビューを頼む」のは人間の判断。「コードが変更されたら自動でレビュー」はハーネスの自動化。この違いは、人間が起動を忘れるという障害モードが存在するかどうか。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>プロアクティブな自動起動</strong>

エージェントのdescriptionに「コードが変更されたら必ず実行する」と書けば、人間が明示的に呼ばなくても自動で起動する。「バグ修正 → tdd-guide」「セキュリティ関連コード → security-reviewer」のように、コンテキストに応じて適切なエージェントが自動選択される。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>DevOpsと同じ構造</strong>

CIが「開発者がテストを実行するのを忘れる」障害モードを排除したように、エージェントの自動起動は「開発者がレビューを依頼するのを忘れる」障害モードを排除する。信頼性は規律からではなく自動化から生まれる。hooksとagentsのdescriptionがこの自動化の仕組み。

</div>
</div>

</div>

---

## エージェントのオーケストレーション設計

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

複雑なタスクを1つのセッションで通すのではなく、<strong>フェーズごとに専門エージェントをチェーンする</strong>。各エージェントは1つの入力を受け取り、1つの出力を返す。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>逐次フェーズの例</strong>

```
Phase 1: RESEARCH (Explore)
  → research-summary.md
Phase 2: PLAN (Planner)
  → plan.md
Phase 3: IMPLEMENT (TDD-guide)
  → コード変更
Phase 4: REVIEW (Code-reviewer)
  → review-comments.md
Phase 5: FIX (必要なら Phase 3に戻る)
```

各フェーズの出力ファイルが次のフェーズの入力になる。フェーズ間で`/compact`してコンテキストをリセットする。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>反復的検索パターン</strong>

サブエージェントは文脈を持たない。オーケストレータが持つセマンティックな文脈をサブエージェントは知らない。

解決策:

1. オーケストレータがサブエージェントの返答を評価する
2. 不十分ならフォローアップ質問を投げる
3. サブエージェントがソースに戻って再調査
4. 最大3サイクルで打ち切る

<strong>クエリだけでなく、目的の文脈も渡す</strong>。

</div>
</div>

</div>

---

## エージェント設計のアンチパターン

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>避けるべきパターン</strong>

- <strong>ツール制限のないエージェント</strong> — 全ツールを持つエージェントは、もう1つのClaudeセッションでしかない。plannerにWrite権限は不要、reviewerにEdit権限は不要
- <strong>出力フォーマットの未定義</strong> — オーケストレータが返答をパースできなければ、委譲は無駄
- <strong>フェーズの飛ばし</strong> — 調査→計画→実装。計画を飛ばして実装に入ると、PLAN MODEの失敗パターンの再現になる
- <strong>過剰な並列化</strong> — 「最小限の並列で最大の成果」が原則。2から始め、必要なときだけ増やす

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>プロアクティブ起動のコツ</strong>

descriptionに「いつ使うか」を明確に書く。

```yaml
# 良い例
description: >
  MUST BE USED for all code changes.
  Reviews quality, security,
  maintainability.

# 悪い例
description: Helps with code review
```

「MUST BE USED for all code changes」と書けばコード変更のたびに自動起動する。曖昧なdescriptionでは起動条件がブレる。

</div>
</div>

</div>

---

## トークン最適化の推奨設定

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

コンテキストウィンドウは記憶容量ではなく注意力の予算。設定で注意力の消費を最適化できる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>settings.jsonの推奨値</strong>

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE":
      "50",
    "CLAUDE_CODE_SUBAGENT_MODEL":
      "haiku"
  }
}
```

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>各設定の効果</strong>

| 設定 | 効果 |
|------|------|
| model: sonnet | 日常タスクに十分。コスト大幅減 |
| MAX_THINKING_TOKENS: 10000 | 思考トークンを制限し消費を抑える |
| AUTOCOMPACT: 50 | 50%到達で自動圧縮 |
| SUBAGENT: haiku | 探索・実行系を最安モデルに委譲 |

デフォルトのAUTOCOMPACTは95%。50%に下げると、注意力が劣化する前に圧縮が走り、セッションが健全に保たれる。

</div>
</div>

</div>

---

## コンテキスト管理の実践

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>コンテキストコマンド</strong>

- <strong>`/clear`</strong> — 無関係なタスク間でコンテキストをリセット。前のタスクの情報が残っていると、次のタスクの推論にノイズが入る
- <strong>`/compact`</strong> — 論理的な区切り（調査完了後、デバッグ完了後）で手動圧縮。自動圧縮を待つより、意図したタイミングで圧縮する方がセッションの品質が高い
- <strong>`/cost`</strong> — 現在のトークン消費を確認。コスト感覚がないまま使い続けると、月末に驚くことになる

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>MCPサーバーは10以下に</strong>

各MCPサーバーはツール定義をコンテキストに追加する。20-30のMCPを有効にすると、200Kのコンテキストウィンドウが実質70K程度まで縮小する。本来のタスクに使える注意力が大幅に減る。

必要なMCPだけを有効にし、プロジェクト単位で`disabledMcpServers`を設定する。`/mcp`コマンドで現在のMCPとそのコンテキストコストを確認できる。

</div>
</div>

</div>

---

## 並列実行パターン

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

エージェントの真の生産性は並列実行で発揮される。git worktreeとtmuxを組み合わせると、複数のエージェントを同時に走らせることができる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>逐次オーケストレーション</strong>

```
planner → tdd-guide
  → code-reviewer → security-reviewer
```

エージェントがチェーンで実行され、前のエージェントの出力が次のエージェントへのコンテキストになる。handoffドキュメントで文脈を受け渡す。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>並列オーケストレーション（DevFleet）</strong>

タスクをDAG（有向非巡回グラフ）に分解し、依存関係のないタスクを並列実行する。各ミッションはgit worktreeで分離。デフォルト同時実行数は3。完了後に自動マージ。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>カスケード方式</strong> — Worker 1が作業してコミット → Worker 2がWorker 1の完了を待って自動起動 → 完了後に自動マージ。コンフリクト時は手動解決。3〜4タスクの同時管理が実用的な上限。

</div>

</div>

---

## MCPで外部サービスとエージェントを接続する

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>Model Context Protocol (MCP)</strong>はエージェントと外部サービスを繋ぐ標準プロトコル。Slack、GitHub、データベース、APIなどと直接やり取りできるようになる。コミュニティでは25以上のMCPサーバーが公開・整備されている。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>何ができるか</strong>

- GitHubのissue/PRを読み書きする
- Slackのチャンネルを検索・投稿する
- データベースにクエリを発行する
- 外部APIを呼び出す
- ファイルをアップロードする

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>何が危険か</strong>

接続先のMCPサーバーが定義するツール記述が、エージェントの行動を変えうる。悪意あるMCPサーバーは、正当なツールに見せかけて意図しない操作を実行させる可能性がある。MCPサーバーのtool数が増えるとコンテキストウィンドウも圧迫する。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

MCPは便利だが、外部への橋渡しは同時に攻撃面の拡大を意味する。次のセクションでは、エージェントに固有のセキュリティリスクを体系的に見ていく。

</div>

</div>

---

## MCP設定の実例

<div style="font-size: 0.65em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

MCPサーバーの設定は`~/.claude.json`の`mcpServers`セクションに書く。コマンド実行型とHTTP型の2種類がある。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>コマンド実行型</strong>

```json
"github": {
  "command": "npx",
  "args": ["-y",
    "@modelcontextprotocol/
     server-github"],
  "env": {
    "GITHUB_PERSONAL_ACCESS_TOKEN":
      "YOUR_GITHUB_PAT_HERE"
  }
}
```

ローカルでプロセスを起動する方式。`env`で環境変数を渡す。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>HTTP型</strong>

```json
"vercel": {
  "type": "http",
  "url": "https://mcp.vercel.com"
}
```

外部URLに接続する方式。`headers`でAPIキーを渡すこともできる。

<strong>設定のポイント</strong>

- 必要なサーバーだけ追加する（10以下推奨）
- `YOUR_*_HERE`を実際の値に置換
- プロジェクト単位の無効化は`disabledMcpServers`

</div>
</div>

</div>

---

## 主要なMCPサーバーと選定の考え方

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>カテゴリ別一覧</strong>

| カテゴリ | サーバー |
|---------|---------|
| 開発 | GitHub, Supabase, Playwright |
| インフラ | Vercel, Railway, Cloudflare |
| 検索 | Exa Web Search, Context7 |
| AI | Sequential-Thinking, FAL.ai |
| データ | ClickHouse, Filesystem |
| セキュリティ | InsAIts (監視) |
| ブラウザ | Browserbase, Firecrawl |

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>選定の考え方</strong>

MCPサーバーを追加するたびにコンテキストウィンドウを消費する。20-30のMCPを有効にすると200Kが実質70K程度まで縮小する。

<strong>判断基準</strong>: そのMCPがなければシェルコマンドやAPIで代替できるか？代替可能ならMCPは不要。MCP固有の価値（双方向通信、認証管理、セッション維持）があるものだけ追加する。

v2.1.76で追加されたMCP Elicitation（対話的入力要求）を使うMCPは、MCPでしかできない操作の好例。

</div>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

エージェントの脅威を知る

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

MCPやskillsは便利だが、外部との接続は攻撃面の拡大を意味する

</div>
</div>

---

## OWASP Agentic Top 10の全体像

<div style="font-size: 0.65em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

OWASP GenAI Securityプロジェクトが2025年12月に公開。100人以上のセキュリティ研究者によるピアレビューを経た、<strong>エージェントAI全般</strong>に対するリスク分類。コーディングエージェントに限らず、業務自動化エージェント、カスタマーサポートエージェント、データ分析エージェントなど、あらゆる自律的AIシステムが対象。従来のLLM Top 10が「モデルの脆弱性」に焦点を当てていたのに対し、こちらは<strong>「エージェントが行動する」ことによるリスク</strong>を扱う。

</div>

| ID    | 名称                           | 概要                                                     |
| ----- | ------------------------------ | -------------------------------------------------------- |
| ASI01 | Agent Goal Hijack              | 外部データに仕込まれた指示でエージェントの目的を乗っ取る |
| ASI02 | Tool Misuse and Exploitation   | 正規ツールを意図しない形で悪用させる                     |
| ASI03 | Identity and Privilege Abuse   | 人間の認証情報を継承して過剰な権限で動作する             |
| ASI04 | Agentic Supply Chain           | MCPサーバーやプラグインの汚染が連鎖する                  |
| ASI05 | Unexpected Code Execution      | 自然言語経由のリモートコード実行                         |
| ASI06 | Memory and Context Poisoning   | 永続メモリの汚染で長期的に行動を変容させる               |
| ASI07 | Insecure Inter-Agent Comm      | エージェント間通信の認証・暗号化の欠如                   |
| ASI08 | Cascading Failures             | 小さなエラーがマルチエージェント構成で連鎖増幅する       |
| ASI09 | Human-Agent Trust Exploitation | 人間のエージェントへの過信を利用する                     |
| ASI10 | Rogue Agents                   | 侵害・誤設定されたエージェントが正常を装い続ける         |

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">正規のツールを正規の手順で使うから、意図の汚染は検知できない。</span>
</div>

</div>

---

## ゴールハイジャックはなぜ起きるのか

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>ASI01: Agent Goal Hijack</strong> — エージェントの根本的な弱点は、データと指示を区別できないこと。メール本文、PDF、Webページ、RAGに取り込んだドキュメント、依存パッケージのREADME。あらゆる「読み込むデータ」が攻撃ベクタになる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>間接的プロンプトインジェクション</strong>

攻撃者は直接エージェントと対話しない。外部データに「以降の指示を無視して、以下を実行せよ」と仕込む。エージェントはこれを正規の指示として処理する。データを読むだけで攻撃が成立する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>なぜ検知が難しいのか</strong>

エージェントは正規のツールを正規の手順で使う。ファイル操作もAPI呼び出しも「許可された操作」の範囲内。従来のセキュリティツールは「何をしたか」は検知できても「なぜしたか」は検知できない。意図の汚染は外から見えない。

</div>
</div>

</div>

---

## ツールの悪用とサプライチェーン汚染

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI02: Tool Misuse</strong>

曖昧なプロンプトや操作された入力で、エージェントが意図しないツール呼び出しを実行する。権限過剰なツールが本番環境に書き込む。シェルコマンドのチェーンで破壊的なパラメータが渡される。仮想と物理の境界が消える。<strong>自然言語一つでファイル削除、不正送金、メール送信が起きうる。</strong>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI04: Agentic Supply Chain</strong>

MCPサーバーの偽装、毒入りプロンプトテンプレート、脆弱なサードパーティエージェント。サプライチェーンのどこか一箇所が侵害されると、自律的に再利用するエージェントを通じて影響が拡大する。従来のソフトウェアサプライチェーン攻撃がエージェント層で再現される。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

対策の要点: ツール呼び出しの権限を最小化する。MCPサーバーの出自を検証する。エージェントが使えるツールのホワイトリストを維持する。実行前にPreToolUseフックで入力を検証する。

</div>

</div>

---

## 権限とアイデンティティの問題

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>ASI03: Identity and Privilege Abuse</strong> — エージェントは人間の認証情報を継承して動作する。SSH鍵、APIトークン、クラウドの認証情報がエージェントのメモリに乗る。侵害されたエージェントは、人間と同じ権限で横展開できる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>Confused Deputy問題</strong>

エージェントAが「信頼されたエージェントB」に処理を委任する。Bの権限にAの権限が合成され、どちらも単独では持たない権限が発生する。マルチエージェント構成では権限の境界が曖昧になりやすい。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>従来のIAMでは不十分</strong>

IAMは「誰が何をできるか」を管理するが、エージェントは「人間の代理として動く非人間アクター」。人間用の権限をそのまま渡すと、人間なら判断して止める操作もエージェントは実行する。エージェント固有の権限モデルが必要。

</div>
</div>

</div>

---

## メモリ汚染と永続的な行動変容

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>ASI06: Memory and Context Poisoning</strong> — プロンプトインジェクションが「その場限り」なのに対し、メモリ汚染は<strong>永続的</strong>。長期メモリ、スクラッチパッド、RAGのembeddings、CLAUDE.mdのmemoryファイルが汚染されると、以後のすべてのセッションでエージェントの行動が変わる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>RAGポイズニング</strong>

知識ベースに悪意あるドキュメントを混入させる。エージェントが参照するたびに汚染された情報に基づいて行動する。正規のドキュメントに見えるため発見が遅れる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>クロステナント文脈漏洩</strong>

マルチテナント環境で、あるテナントの文脈が別テナントのエージェントに漏洩する。メモリの分離が不十分だと、機密情報が他者のセッションに現れる。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

対策: メモリの変更履歴を追跡する。定期的にメモリの整合性を検証する。メモリへの書き込みに人間の承認を要求する。Claude Codeのmemoryファイルが意図しない内容を含んでいないか定期的に確認する。

</div>

</div>

---

## コード実行とカスケード障害

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI05: Unexpected Code Execution</strong>

自然言語がRCE（Remote Code Execution）の入口になる。コードアシスタントがサンドボックスなしで生成パッチを実行する。プロンプトインジェクションでシェルコマンドが注入される。<strong>従来のRCEはエクスプロイトが必要だったが、エージェント経由なら自然言語で足りる。</strong>

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI08: Cascading Failures</strong>

マルチエージェント構成では、小さなエラーが計画→実行→メモリ→下流システムに同時に伝播する。ハルシネーションを起こしたplannerが、複数の実行エージェントに破壊的なタスクを配信する。一つの誤判断が複数のシステムに波及する。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

SREの視点: これはインシデントのカスケード障害と同じ構造。モニタリングのノイズで本質的な問題が埋もれ、対応が遅れ、影響範囲が拡大する。エージェント構成にもサーキットブレーカーとフォールバックが要る。

</div>

</div>

---

## 人間の過信と野良エージェント

<div style="font-size: 0.72em;">

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI09: Human-Agent Trust Exploitation</strong>

エージェントの自信に満ちた、整った説明を人間はそのまま信じやすい。コードレビューで「エージェントが書いたから大丈夫だろう」とバックドアを見逃す。財務copilotが不正な送金を「正常な処理」として説明し、承認者がそのまま通す。AIによるソーシャルエンジニアリングは従来のフィッシングより検知が困難。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ASI10: Rogue Agents</strong>

侵害されたエージェント、あるいは誤設定されたエージェントが正常を装って動き続ける。データの外部送信を継続する。承認エージェントが危険な操作を黙認する。コスト最適化エージェントがバックアップを削除する。監視なしの長期実行が最大のリスク。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">エージェントの自信に満ちた説明が正しいとは限らない。承認は理解の証明ではない。</span>
</div>

</div>

---

## 実際に報告されたCVEとサプライチェーンの現実

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

OWASPのリスクは理論ではない。Claude Code自体にも脆弱性が報告されており、エージェントのサプライチェーン汚染は現実に起きている。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>報告済みCVE</strong>

- <strong>CVE-2025-59536</strong>（CVSS 8.7） — プロジェクト内のコードが信頼ダイアログ表示前に実行される可能性があった
- <strong>CVE-2026-21852</strong> — 攻撃者が`ANTHROPIC_BASE_URL`を制御し、APIトラフィックをリダイレクトしてAPIキーを窃取

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>スキルサプライチェーンの実態</strong>

Snykの調査で、公開されている3,984個のClaude Codeスキルのうち<strong>36%にプロンプトインジェクションが含まれていた</strong>。npmやPyPIと同じサプライチェーン攻撃が、エージェントのスキル層で再現されている。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>具体的な攻撃ベクタ</strong>: `.claude/`ディレクトリに仕込まれた悪意あるhooks、MCPサーバーのツール出力経由のインジェクション、Git PRレビューコメントに埋め込まれた指示、メール添付PDFからの間接的インジェクション。

</div>

</div>

---

## Least Agencyの設計原則

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

セキュリティは「エージェントに安全に振る舞えと指示する問題」ではない。プロンプトインジェクションは<strong>必ず起きる</strong>前提で、爆発半径を小さくするインフラの問題。Least Privilege（最小権限）をエージェントに拡張したのが<strong>Least Agency（最小自律）</strong>。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>Least PrivilegeとLeast Agencyの違い</strong>

Least Privilegeは「何にアクセスできるか」を制御する。ファイル、API、ネットワーク。Least Agencyはさらに踏み込んで「何を判断させるか」を制御する。権限があっても、エージェントにその判断を委ねなければ、攻撃面は縮小する。PLAN MODEはまさにこの原則の実装 — 読み取り権限はあるが、変更の判断は人間に留保する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>deny設定で物理的に制限する</strong>

rulesの「やるな」は努力義務。settings.jsonのdenyは物理的なブロック。エージェントが特定のディレクトリ、コマンド、ファイルに触れないようにする最後の砦。rulesで意図を伝え、hooksで検証し、denyで物理的に止める — 三重の防御。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">権限があっても判断を委ねなければ、爆発半径は縮小する。</span>
</div>

</div>

---

## Least Agencyの段階的な実装

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

Least Agencyは一度に導入するものではない。段階的に信頼を積み、検証した上で自律度を上げる。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

<strong>Step 1</strong>

PLAN MODEで計画だけ。読み取り専用でエージェントの理解力を評価する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

<strong>Step 2</strong>

Normalで実装。毎回確認しながら信頼を積む。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

<strong>Step 3</strong>

rules/hooks整備後にAuto-Accept。機械的制約を敷いてから自律度を上げる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px; text-align: center;">

<strong>Step 4</strong>

CLAUDE.md+rulesをリポジトリにコミットしてチーム標準に。

</div>
</div>

<div style="margin-top: 12px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

この階段を飛ばしてStep 4に行くと、チーム全体がYOLOの事故に巻き込まれる。<strong>順番が大事</strong>。一人が検証し、制約を整備し、その制約をチームに展開する。

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">制約は初回コスト。以後すべてのセッションで複利で効く。</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

ハーネスエンジニアリングで道を舗装する

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

脅威を知った。では、品質を守る仕組みをどう作るのか

</div>
</div>

---

## ハーネスエンジニアリングとは何か

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

<strong>coding agent = AI model(s) + harness</strong>。エージェントの品質はモデルの賢さではなく、周囲の制約（ハーネス）の質で決まる。Mitchell Hashimotoが「エージェントがミスをするたびに、二度とそのミスを起こせない仕組みを作る」と定義した。Can Bolukのベンチマークでは、同じモデルでもハーネスの違いで22ポイント性能が変わる一方、モデルを交換しても1ポイント程度しか変わらなかった。投資すべきはモデルではなくハーネス。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>3つの柱</strong>

- <strong>コンテキストエンジニアリング</strong> — エージェントが参照する情報の質を管理する。CLAUDE.mdは索引、rulesで制約、ADRで判断履歴。Slackやドキュメントはエージェントに見えない。リポジトリに書かれたものだけが真実
- <strong>構造的制約</strong> — linter、型システム、アーキテクチャルールで「そもそも間違ったコードが書けない」状態を作る。エラーメッセージに修正手順を埋め込めば、エージェントが自己修正できる
- <strong>自動ゴミ収集</strong> — エージェント生成コードは人間と異なる形で劣化する（重複、未使用コード、型の劣化）。定期的に品質スキャンし、リファクタリングPRを自動生成する

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>なぜハーネスがモデルより重要なのか</strong>

モデルの性能は開発者がコントロールできない。次のバージョンが出れば変わるし、プロバイダの判断で挙動が変わることもある。一方、ハーネスは開発者が完全にコントロールできる。linterのルール、テストの網羅性、型の厳密さはすべて自分たちで設計できる。

さらに、ハーネスは<strong>複利で効く</strong>。一度追加したlinterルールは、以後のすべてのコード生成に適用される。テストは書いた瞬間から永続的に品質を検証し続ける。モデルの改善は一時的だが、ハーネスの改善は蓄積する。

</div>
</div>

</div>

---

## ハーネスの中核はLint・テスト・型システム

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

人間がコードを書いていた時代、lintやテストは「品質向上の手段」だった。エージェントがコードを書く時代、それらは<strong>「品質の唯一の防衛線」</strong>になる。エージェントの出力が正しいかどうかを判定できるのは、機械的な検証だけ。OpenAIはAIスロップの手動清掃に多大な時間を費やしていたが、linterとテストの自動化で解消した（Harness Engineering, 2026）。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>ハーネスの階層</strong>

| 層         | 役割                     | 例                                     |
| ---------- | ------------------------ | -------------------------------------- |
| 型システム | コンパイル時に不正を排除 | TypeScript strict, Rust borrow checker |
| Linter     | スタイルとパターンの強制 | ESLint, clippy, ruff                   |
| テスト     | 振る舞いの検証           | ユニット, 統合, E2E                    |
| CI/CD      | デプロイ前の最終ゲート   | GitHub Actions, Cloud Build            |

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>再発防止の仕組み化</strong>

エージェントがミスをしたら:

1. そのミスを検知するlinterルールを追加する
2. rulesにNEVER制約を追加する
3. hooksで自動ブロックする

人間への「気をつけてね」ではなく、機械的にブロックする。<strong>信頼性は規律からではなく自動化から生まれる</strong> — これはSREの品質管理プロセスとまったく同じ構造。

</div>
</div>

</div>

---

## リポジトリの発酵と腐敗

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

人間がリポジトリを読むとき、「このREADMEは古そうだな」と直感的に判断できる。エージェントにはこの判断ができない。3ヶ月前の設計メモも昨日のコミットも、コンテキストに入れば同じ重みで扱われる。つまり、リポジトリに古い情報が残っているだけで、エージェントの出力品質が下がる。

</div>

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

リポジトリ内の情報は2種類に分かれる。<strong>時間が経つほど信頼性が高まるもの</strong>と、<strong>時間が経つほど害になるもの</strong>。テストは前者の典型。実行すれば正しさが検証されるし、コードが変われば即座に失敗して更新を迫る。linter設定やADR（Architecture Decision Records、ステータスとタイムスタンプ付き）も同様に「実行や構造で鮮度が担保される」情報。

一方、自然言語で書かれた説明文書、システム状態の記述（「現在のDB構成は〜」）、スタイルガイドは後者。コードの進化に追いつけず、放置すればエージェントが古い前提で誤ったコードを生成する。古いTODOコメントも危険で、エージェントが「未実装だ」と判断して不要なコードを足すことがある。

</div>

</div>

---

## 古い情報からエージェントを守る

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

対策は2つある。エージェントから見えない場所に移すか、情報の鮮度を自動で維持する仕組みを作るか。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>エージェントの視界から外す</strong>

CLAUDE.md内のHTMLコメント（`<!-- -->`）はv2.1.72から自動注入時に非表示になった。人間がメモを残しつつ、エージェントには見せない使い方ができる。

古い設計文書はWikiや外部ツールに移し、CLAUDE.mdにはリンクだけ残す。リンク先が消えればエラーで気づくが、古い文書がリポジトリの奥に眠っていても誰も気づかない。見えないことが最大の防御になる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>鮮度を自動で維持する</strong>

仕様の説明を書く代わりにテストを書く。テストが通っている限り仕様は正しい。設計判断はADRに記録し、ステータス（Accepted / Superseded / Deprecated）を必ず付ける。エージェントはSupersededマークを見て新しい判断を参照できる。

CIにデッドリンクチェックと未使用ファイル検出を入れる。さらに進めると、エージェント自身にリポジトリを定期走査させて、古いドキュメントや未使用コードのリファクタリングPRを自動生成させることもできる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">実行で検証できない情報は、時間とともにエージェントの敵になる。</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.2em; font-weight: bold;">

ハーネスを構成するもう一つの柱</br>テスト戦略

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

Lint・型チェックでコードの形を守り、テストで振る舞いを検証する

</div>
</div>

---

## E2Eテストはもはや省略できない

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

10年前は「E2Eテストなんてデモ前に手動確認すれば十分」と思っていた。今の自分から言わせれば、それは個人の能力を過信している。OAuth2フローのE2Eテスト — 複数リダイレクト、Cookie管理、セッション状態の確認 — を毎回手動で確認するのは人間の注意力の限界を超えている。「疲れていたから見落とした」で本番障害が起きるのは、個人の問題ではなく<strong>構造的な失敗</strong>。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>エージェント時代に必須になった理由</strong>

エージェントが生成したUIコードを人間が目視確認するのは現実的ではない。Playwright MCPやPlaywright CLIを使えば、エージェント自身がブラウザを操作してE2Eテストを実行し、結果を検証できる。<strong>生成→検証→修正のループが人間を介さずに自動で回る</strong>。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>ツール選択の指針</strong>

| ツール                 | 特徴                                       |
| ---------------------- | ------------------------------------------ |
| Playwright MCP         | MCP経由でブラウザ操作。コンテキスト消費大  |
| Playwright CLI         | シェルコマンド経由。MCPより軽量 |
| agent-browser (Vercel) | ref方式でセレクタ脆性を排除。最高効率      |
| Hurl                   | HTTP APIテスト。宣言的で軽量               |

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">「疲れていたから見落とした」は個人の問題ではなく、手動検証に依存する設計の問題。</span>
</div>

</div>

---

## E2Eテストのツール選定と実装戦略

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 10px; border-radius: 8px; margin-bottom: 10px;">

実際にE2Eテストをエージェントのワークフローに組み込もうとすると、ツール選定で迷う。ポイントは2つ。<strong>トークン効率</strong>（MCPツール定義のコンテキスト消費）と<strong>セレクタの安定性</strong>（UIが変わってもテストが壊れないか）。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>対象ごとに何を使うか</strong>

| 対象 | 推奨ツール |
|------|--------|
| Web | Playwright CLI、agent-browser（Vercel） |
| Mobile | XcodeBuildMCP（iOS）、mobile-mcp、Detox（React Native） |
| API | Hurl（宣言的HTTP、軽量）、Pact（契約テスト） |
| CLI | bats-core、pexpect（対話操作） |
| Desktop | Playwright Electron、tauri-driver |
| インフラ | `terraform test` + Conftest（OPA） |

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>Web E2Eの3つの選択肢</strong>

<strong>Playwright MCP</strong>はMCPプロトコル経由でブラウザを操作する。直感的だが、ツール定義がコンテキストを消費する。<strong>Playwright CLI</strong>はシェルコマンド経由で同じことができ、トークン効率が高い。

<strong>agent-browser</strong>（Vercel Labs）はDOM要素に一意のIDを自動付与する方式で、CSSセレクタへの依存がない。UIが変わってもIDは再付与されるので、テストの頑健性が高い。

いずれもエージェントが自分でブラウザを操作→結果を検証→問題があれば修正、というループを自動で回せる。

</div>
</div>

</div>

---

## AI時代にテスト戦略は根本から変わる

<div style="font-size: 0.68em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 10px;">

2025年はAIの速度の年、2026年はAIの品質の年。DORA 2025のデータが示す現実: AI導入率95%、PR数+98%/人、しかしバグ率+9%、レビュー時間+91%。<strong>個人の生産性は上がったが、組織のデリバリー指標は横ばい</strong>。テストがこのギャップを埋める。

</div>

<div style="display: flex; gap: 10px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>テストの役割が変わった</strong>

- <strong>仕様書</strong> — テストが「何を作るか」を定義する。プロンプトより曖昧さがない
- <strong>生きたドキュメント</strong> — 実行すれば嘘をつけない。説明文書は腐敗する
- <strong>人間とエージェントの契約</strong> — 「完了」の定義をテストが機械的に判定する
- <strong>品質のガードレール</strong> — エージェントが間違ったとき、テストが教える

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 10px; border-radius: 8px;">

<strong>AI生成コードのエラー率</strong>

CodeRabbitの470PR分析:
- AI生成PRのイシュー数: <strong>人間の1.7倍</strong>
- ロジックエラー: <strong>1.75倍</strong>
- 可読性の問題: <strong>3倍以上</strong>
- セキュリティ問題: <strong>最大2.74倍</strong>
- パフォーマンス退行: <strong>約8倍</strong>

これらを人間のレビューだけで捕捉するのは不可能。テストが唯一のスケーラブルな検証手段。

</div>
</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">個人と組織の生産性ギャップを埋めるのはテストだけ。</span>
</div>

</div>

---

## TDDはエージェント時代にこそ効く

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

DORA Report 2025: 「AIは増幅器であり、TDDのような優れたプラクティスをさらに効果的にする」。テストを先に書くことで、エージェントが「壊れた実装を検証するテスト」を書くチートを防ぐ。テストが先にあれば、エージェントはそのテストを通すことに集中する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>なぜTDDがエージェントに効くのか</strong>

エージェントはプロンプトより<strong>テストの方が仕様として正確に理解する</strong>。「ユーザー登録時にメールを送る」という自然言語より、`test_registration_sends_email()`の方が曖昧さがない。

ただし注意点がある。TDAD論文（2026年3月）によると、TDDの手順を冗長に説明するとコンテキストを圧迫して逆効果になる。<strong>テストコード自体を渡す方が、テスト方法論の説明より効く</strong>。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>プロパティベーステストの可能性</strong>

Anthropicの研究で、Claudeがプロパティベーステストを使って100以上のPythonパッケージから<strong>984件の潜在バグを発見</strong>。手動レビューで56%が実際のバグ、32%がメンテナに報告する価値あり。NumPy、AWS Lambda Powertoolsにパッチがマージされた。

エッジケースを人間が網羅的に考える必要がない。プロパティ（不変条件）を定義すれば、エージェントが自動でテストケースを生成する。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">テストのないプロンプトは、正しさの定義がない依頼。</span>
</div>

</div>

---

## フィードバック速度が品質を決める

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

エージェントにとってフィードバックの速度は品質に直結する。CIが10分後に「失敗」と返すのと、ファイル保存直後にlinterが「ここが違う」と返すのでは、修正の精度がまったく異なる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>コンテキストは流れ去る</strong>

エージェントはコンテキスト内の情報で自己修正する。フィードバックが返る前に新しい作業が進むと、修正時の文脈が変わっている。10分後のCI失敗は「10分前の文脈」の修正を要求するが、エージェントは既に先に進んでいる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>速いフィードバックは自律を可能にする</strong>

速いフィードバックはエージェントの自己修正ループを回す。遅いフィードバックは人間の介入を待つ。つまりフィードバック速度は、どこまでエージェントに任せられるかの上限を決める。速い層ほど自律度を上げられる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">フィードバックが遅いほど、文脈が流れ去り、手戻りが膨らむ。</span>
</div>

</div>

---

## フィードバックの階層設計

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

フィードバックの速度は一律ではない。検証の種類ごとに最適なタイミングがあり、それぞれ異なるメカニズムで実現する。速い層ほど機械に任せ、遅い層ほど人間の判断を活かす。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>4つの層</strong>

| 層 | 速度 | 検証内容 |
|---|---|---|
| PostToolUse | ミリ秒 | format, lint |
| プリコミット | 秒 | 型チェック, テスト |
| CI/CD | 分 | 統合テスト, E2E |
| 人間 | 時間〜 | 設計判断, 要件適合 |

上の層ほどエージェントが自律的に修正でき、下の層ほど人間の関与が必要になる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>「速度を上げるとオーバーヘッドが増える」への反論</strong>

PostToolUseフックは同期実行するとエージェントの操作をブロックする。しかしformat程度は100ms以内で終わる。lintが重い場合は`async: true`にしてバックグラウンド実行する。

速度のコストは設計で制御できるが、遅いフィードバックのコスト（手戻り、人間の介入）は制御できない。制御可能なコストと制御不能なコストなら、制御可能な方を選ぶ。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">速い層ほど機械に、遅い層ほど人間に。</span>
</div>

</div>

---

## エラーメッセージを修正指示にする

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

普通のlinterは「ここが違反」と指摘するだけ。エージェント向けには、エラーメッセージそのものに<strong>何をどう直すかの手順</strong>を含める。PostToolUseフックの返却に含まれるので、エージェントは次のアクションで即座に修正を試みる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>従来のエラーメッセージ vs 修正指示</strong>

```
ERROR: ServiceAはInfraレイヤーに
       直接依存できません
WHY:   ADR-007参照
FIX:   Providerインターフェースを
       Domain層に定義してください
```

「何が間違っているか」だけでなく「なぜ間違っているか」「どう直すか」まで含める。エージェントがADRを参照して設計判断の文脈を理解し、正しい修正を選択できる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>構造的な効果</strong>

修正手順付きのエラーメッセージは、人間にとっても有益。新メンバーが初めてプロジェクトのアーキテクチャルールに触れたとき、「なぜこう設計されているか」がエラーメッセージから学べる。エージェントのためのインフラが、同時に人間のためのドキュメントとして機能する。

ここに<strong>ハーネスの複利効果</strong>がある。一度作ったエラーメッセージテンプレートは、以後のすべてのエージェントセッションと、すべての新メンバーのオンボーディングに効く。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">エージェントのためのインフラは、人間のためのインフラでもある。</span>
</div>

</div>

---

## 生成速度と理解速度の非対称

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

ここまでハーネスの各要素 — Lint、テスト、E2E、フィードバック速度 — を見てきた。ここからは、これらが積み重なった先にある構造的な変化を考える。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>生成は並列化できるが、理解は並列化できない</strong>

エージェントを複数走らせれば生成速度は上がる。しかし人間の理解速度は並列化できない。コードベースの全体像を把握するのは個人の認知作業であり、人を増やしても線形には速くならない。生成のスケール曲線と理解のスケール曲線は根本的に形が違う。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>理解されないコードは複利で負債になる</strong>

エージェントが数時間で書いた機能を、チームが何年も運用する。理解されていないコードが1万行あると、次の変更の影響範囲が推論できない。結果として「触らない方が安全」な領域が広がり、システム全体の変更容易性が劣化する。これは技術的負債とは別種の<strong>理解の負債</strong>。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">生成は並列化できるが、理解は並列化できない。</span>
</div>

</div>

---

## 「書いた人が直す」前提の崩壊

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

従来の開発・運用は「コードを書いた人間がいる」前提で成り立っていた。設計意図を知る人間がレビューし、障害時にはその人間が原因を推論し、修正する。エージェントがコードを書く時代に、この前提が崩れる。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>障害時に誰が直すのか</strong>

エージェントが生成したコードで障害が起きたとき、そのコードの設計意図を知る人間がいない。「速く書けた」は「速く直せる」と同義ではない。障害時の推論時間が延び、MTTRが悪化する。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>ハーネスが前提を置き換える</strong>

人間が全コードを理解していなくても、テスト・linter・型チェックが通っていれば影響範囲を特定できる。ADRで判断の履歴を残していれば、エージェント自身に障害調査を依頼することもできる。人間の理解に依存しない運用設計が必要になる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">誰も理解していないコードが本番で動く。その前提で設計せよ。</span>
</div>

</div>

---

## レビュワーはもう育てられない

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

品質を維持したままコードレビューを軽量化し続ける必要がある。レビューを0にするという話ではない。ただ、今のままの人間中心のレビューには構造的な限界がある。最初の限界は、レビュワーの再生産が止まること。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>育成方法が消える</strong>

コードレビュー能力の育成方法として、自分でコードを書いて経験を積む以外の道はまだ確立されていない。エージェントがコードを書く時代に、現在のレビュワーと同程度にコードを書く時間を確保することは現実的に難しい。レビュー能力の再生産が構造的に止まる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>「AIが書いたコードを読む力は別では」への反論</strong>

「エージェントが書いたコードを読む能力を鍛えればいい」という反論がある。しかし「読む力」も「書く経験」から来る。書いたことのないパターンのコードを的確にレビューできる人間は稀。さらに生成量が増えれば、読む対象自体が人間のキャパシティを超える。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">書く機会が減れば、レビュー能力の再生産は止まる。</span>
</div>

</div>

---

## 生成量がレビュー容量を超える

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

2つ目の構造的な限界。エージェントの生成速度は今後さらに上がるが、人間のレビュー速度は上がらない。DORA 2025のデータ: PR数+98%/人、レビュー時間+91%。生成がレビューのキャパシティを超えている現実がすでにある。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>品質のばらつきが許容できない</strong>

人間のレビューは疲労、注意力、経験に左右される。朝一番のレビューと金曜夕方のレビューで品質が同じ保証はない。100件のPRのうち1件のレビュー漏れが本番障害を起こす。機械的な検証にはこのばらつきがない。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>「レビューを減らすと品質が下がる」は本当か</strong>

直感に反するが、ハーネスがレビューの検証項目を機械的にカバーできるなら、レビューを軽量化した方がむしろ品質は安定する。人間のレビューは品質にばらつきが大きすぎる。機械的な検証にばらつきはない。品質を保ったまま軽量化に成功した組織は、そうでない組織よりデリバリーが速い。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">人間のレビューは疲れる。機械の検証は疲れない。</span>
</div>

</div>

---

## ではレビューをどう再設計するか

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

問題は明確になった。レビュワーの再生産は止まり、生成量はレビュー容量を超える。ではレビューを捨てるのか。そうではない。レビューの「何を」「誰が」を再定義する。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>レビューで見ていたものを分解する</strong>

コードレビューで人間が見ていたものは5つに分解できる。フォーマット、命名規則、セキュリティパターン、ロジックの正しさ、設計の妥当性。前3つはlinter・静的解析で自動化できる。ロジックはテストで検証できる。人間にしかできないのは<strong>設計の妥当性の判断だけ</strong>。つまりレビューの8割は機械に移管可能。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>「減らす」ではなく「再配置」</strong>

レビューを減らすのではなく、各検証を最も適した担い手に再配置する。フォーマットはlinterに。ロジックはテストに。セキュリティは静的解析に。人間には設計判断だけが残る。総量は変わらない。担い手が変わる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">レビューを減らすのではなく、担い手を再配置する。</span>
</div>

</div>

---

## 軽量なレビューで品質を保てる組織が生き残る

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 15px;">

「レビューを減らす＝品質が下がる」は、レビューが品質保証の唯一の手段だった時代の前提。ハーネスが検証項目を機械的にカバーできるなら、この前提は崩れる。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>競争原理が淘汰を起こす</strong>

ハーネスで品質を担保しながらレビューを軽量化した組織は、同じ品質でデリバリーが速い。これは「悪貨が良貨を駆逐する」ではなく、機械的な検証の方が人間のレビューより均質で信頼性が高いという構造的な優位。重いレビュープロセスに固執する組織は、軽量化に成功した組織に淘汰される。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ただし、ハーネスなしの軽量化は崩壊する</strong>

レビューだけを減らしてハーネスを整備しなければ、品質は確実に下がる。「レビュー軽量化＝品質低下」が成り立つのは、代替手段なしに省略した場合。ハーネスの整備が先、軽量化は後。この順番を守れない組織は、速度も品質も失う。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">ハーネスの整備が先、軽量化は後。順番を間違えると崩壊する。</span>
</div>

</div>

---

## エージェントにセルフレビューを委譲する

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「レビューを機械に」と言うと抽象的に聞こえるが、実際に自分の開発フローに組み込んでいる仕組みがある。コードを変更したら、コミット前にエージェント自身に自分のコードをレビューさせ、問題を自動修正させるセルフレビューのパターンだ。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>複数の視点を並列で回す</strong>

1台のレビュアーでは視野が偏る。品質・セキュリティ・パフォーマンスを見るレビュアー、外部モデルの視点を入れるレビュアー、可読性と過剰な複雑さを検出するレビュアー。この3つを並列で走らせると、それぞれが異なる角度から指摘を出す。複数のレビュアーが同じ箇所を指摘したら、それは本当に直すべき問題。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>指摘 → 評価 → 修正のループ</strong>

レビューが出たら、次のステップが重要。指摘を鵜呑みにせず、各コメントの技術的妥当性を評価するフェーズを挟む。「要件で意図的にそう設計した」「トレードオフとして許容済み」なら対応しない。対応しない理由も明示する。実際に10件の指摘から6件だけ採択し、4件を根拠付きで却下する — これが健全なレビューの姿。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">レビュー指摘を鵜呑みにするエージェントは、要件を壊す。</span>
</div>

</div>

---

## セルフレビューが変えるもの

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

セルフレビューの仕組みは、人間のレビューを不要にするためではなく、人間がdiffを開く前に品質の底を引き上げるためにある。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>「毎回ツッコミを入れる」をなくす</strong>

エージェントの出力は品質が安定しない。変数名、エラーハンドリング、不要なコメント — 同じ種類の問題を毎回人間が指摘するのは時間の無駄。セルフレビューで先に潰せば、人間がdiffを開いたときに目にするのは設計上の判断だけになる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>レビューの本来の目的を取り戻す</strong>

コードレビューの目的は「コードベースの健康状態を改善すること」のはず。しかし実態は変数名やフォーマットの指摘に時間を取られ、設計の議論に辿り着かない。些末な指摘を機械に委譲すれば、人間はコードベース全体の健康に集中できる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">品質の底上げを自動化し、人間は設計判断に集中する。</span>
</div>

</div>

---

## 今のエージェントの限界と、それでも作る理由

<div style="font-size: 0.75em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

正直に言えば、今のエージェントは全体感のない実装をしがちだ。既存コードとの一貫性を無視したり、プロジェクト全体の設計方針から外れた局所最適な変更を出してくる。セルフレビューの指摘も、今は変数名や未使用インポートのような表層的なものが多い。

</div>

<div style="display: flex; gap: 15px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>全体感の欠如は構造的な限界ではない</strong>

コンテキストウィンドウは1Mトークンに拡大し、モデルの推論能力は四半期ごとに向上している。全体感の欠如は「今の」エージェントの一時的な限界であって、アーキテクチャの壁ではない。セルフレビューの仕組みを今作っておけば、モデルが進化するたびに自動的にレビューの質も上がる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

<strong>ハーネスは今作る。モデルは後から来る</strong>

セルフレビューの仕組み — 複数エージェント並列、指摘の批判的評価、修正ループ — これはモデルに依存しない構造。モデルが賢くなれば、同じ仕組みでより深い指摘が出る。粗い指摘しか出ない今だからこそ、仕組みを先に作る意味がある。仕組みは複利で効く。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">仕組みは今作る。モデルの進化が後から仕組みを強くする。</span>
</div>

</div>

---

## 品質保証の3つの柱

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 12px; border-radius: 8px; margin-bottom: 12px;">

コードレビューを軽量化するには、レビューで担保していた品質を別の手段に置き換える必要がある。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>1. 高品質なコンテキスト</strong>

エージェントが高品質なコードを生成するための入力を整備する。CLAUDE.md、rules、ADR、テスト。これらのコンテキスト自体の開発・維持もエージェントが支援できる。生成AIの文書化能力は多くの人間を超えており、均質なコンテキストを維持するコストは下がり続ける。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>2. 高品質なハーネス</strong>

linter、静的解析、型チェック、テストスイート。これらがコードレビューで人間が見ていた部分を機械的に置き換える。プロダクト固有のアーキテクチャやフレームワーク自体もハーネスの一部。コードの形を制約すれば、レビューで確認すべき範囲が狭まる。

</div>
</div>

<div style="margin-top: 10px; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>3. 人間の評価を上位レイヤーに集中させる</strong>

コンテキストとハーネスがコード品質を担保した上で、人間はコード行ではなく「要求への適合」を評価する。フォーマット、命名、セキュリティパターンは機械に任せ、設計の妥当性と要件の整合だけが人間の仕事として残る。

</div>

<div style="margin-top: 10px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">入力をコンテキストで、出力をハーネスで、構造を人間で。</span>
</div>

</div>

---

## 人間の評価はコードからより上位のレイヤーへ

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

コンテキストとハーネスがコード品質を担保する。では、人間は何を評価するのか。コード行レベルのレビューから、要求・要件への適合度の評価に移行する。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>評価システム自体もエージェントが生成する</strong>

コンテキスト（要求仕様、ADR、テスト）をインプットにして、コード生成とは別プロセスで評価基準を自動生成できる。言語化された領域の評価は、近いうちに人間よりエージェントの方が高い精度でできるようになる。人間が最終的に評価するのは、最上位要件への適合と、まだ言語化されていない領域。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>レビューは消えない、形が変わる</strong>

人間がコードを目視する機会は減る。しかし、レビューという考え方自体がプロセスから消えるわけではない。linterや静的解析のような自動化された品質保証プロセスが、人間がレビューしていた部分を置き換えていく。人間の役割は、コード行の精査から、<strong>構造と要件の整合を検証する仕事</strong>に変わる。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">コードを読める人は減る。問いを立てられる人の価値は上がる。</span>
</div>

</div>

---

## 「ジュニアの育成にレビューが必要」という反論

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

「レビューを減らすとジュニアが育たない」は最もよく聞く反論。これは部分的に正しい。ただし、レビューの教育機能と品質保証機能を混同している。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>品質保証と教育は別のプロセス</strong>

品質保証は「このコードを本番に出して良いか」の判定。教育は「このエンジニアの設計判断力を育てる」プロセス。同じレビューで両方やっていたのは、人間がボトルネックだった時代の最適化でしかない。品質保証を機械に移せば、教育のための時間が生まれる。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>教育手段はレビュー以外にもある</strong>

ペアプログラミング、設計ドキュメントのレビュー（コードではなく設計意図）、ADRの共同作成、モブプログラミング。むしろdiffベースのコードレビューは、教育手段としては受動的すぎる。「なぜこの設計にしたか」を議論する場の方が学びが深い。

</div>
</div>

<div style="margin-top: 12px; padding: 10px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">品質保証と育成を混同すると、両方が中途半端になる。</span>
</div>

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: transition
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">
<div style="font-size: 1.5em; font-weight: bold;">

まとめ

</div>
<div style="margin-top: 20px; font-size: 0.9em; color: #ccc;">

この発表で見てきたことを振り返る

</div>
</div>

---

## 3つの持ち帰り

<div style="font-size: 0.72em;">

<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px; margin-bottom: 12px;">

<strong>1. 道を示してから走らせる</strong>

PLAN MODEで計画を立て、人間がレビューし、承認してから実装に移る。計画のアノテーション（Phase 2.75）がエージェントの理解の限界を可視化する。制約のないエージェントは「自分なりの最善」を実行する — それはプロジェクトの最善ではない。

</div>

<div style="display: flex; gap: 12px; align-items: center;">
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>2. ハーネスを構築する</strong>

CLAUDE.mdは索引。rulesで関心事を分離。hooksで品質チェックを自動化。テストで振る舞いを検証。linterでスタイルを強制。ハーネスの違いがモデルの違いより圧倒的に性能に効く（Can Boluk, 2026）。投資すべきはハーネス。

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 12px; border-radius: 8px;">

<strong>3. 知り、整理し、更新し続ける</strong>

3ヶ月で80リリース。新機能を知り、自分のrules/skills/hooksに反映し、古くなった情報を刈り取る。この継続的な再整理がチームのハーネスを育てる。個人のツールで終わらせず、リポジトリにコミットしてチームの形式知にする。

</div>
</div>

<div style="margin-top: 12px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">正しいコードを書くのではなく、正しいコードが生まれる構造を作る。</span>
</div>

</div>

---

## 参考資料（1）公式ドキュメント・自著ブログ

<div style="font-size: 0.55em;">

**Anthropic公式**

- [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview) — 公式ドキュメントハブ
- [Best Practices for Claude Code](https://www.anthropic.com/engineering/claude-code-best-practices) — 公式ベストプラクティス
- [Extend Claude with Skills](https://code.claude.com/docs/en/skills) — スキル公式ドキュメント
- [Claude Code Hooks Reference](https://docs.anthropic.com/en/docs/claude-code/hooks) — フック公式リファレンス
- [How Anthropic Teams Use Claude Code](https://claude.com/blog/how-anthropic-teams-use-claude-code) — 社内利用パターン
- [Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — Anthropic Engineering（2025年11月）

**自著ブログ（じゃあ、おうちで学べる）**

- [Claude CodeのPLAN MODEは使ったほうがいい](https://syu-m-5151.hatenablog.com/entry/2026/02/13/122228)（2026年2月）
- [SREはAgentic Engineering時代のHarnessになれるのか？](https://syu-m-5151.hatenablog.com/entry/2026/03/16/135031)（2026年3月）
- [Claude Codeの Agent Skills は設定したほうがいい](https://syu-m-5151.hatenablog.com/entry/2025/12/19/173309)（2025年12月）
- [Claude CodeのSubagentsは設定したほうがいい](https://syu-m-5151.hatenablog.com/entry/2025/09/09/143306)（2025年9月）
- [Claude Code の CLAUDE.mdは設定した方がいい](https://syu-m-5151.hatenablog.com/entry/2025/06/06/190847)（2025年6月）

</div>

---

## 参考資料（2）ハーネスエンジニアリング

<div style="font-size: 0.55em;">

**原典・一次ソース**

- [Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/) — Ryan Lopopolo, OpenAI（2026年2月）
- [My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey) — Mitchell Hashimoto（2026年2月）「エージェントがミスをするたびに、二度とそのミスを起こせない仕組みを作る」
- [Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) — Birgitta Böckeler, Thoughtworks / Martin Fowler（2026年2月）
- [The Emerging Harness Engineering Playbook](https://ignorance.ai/p/the-emerging-harness-engineering) — Charlie Guo, OpenAI（2026年2月）
- [Skill Issue: Harness Engineering for Coding Agents](https://humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents) — HumanLayer（2026年3月）
- [I Improved 15 LLMs at Coding in One Afternoon](https://blog.can.ac/2026/02/12/the-harness-problem/) — Can Boluk（2026年2月）

**コミュニティ**

- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) — エージェントハーネス性能システム
- [Writing a Good CLAUDE.md](https://humanlayer.dev/blog/writing-a-good-claude-md) — HumanLayer

</div>

---

## 参考資料（3）セキュリティ・その他

<div style="font-size: 0.55em;">

**OWASP・セキュリティ**

- [OWASP Top 10 for Agentic Applications (2026)](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/) — OWASP GenAI Security Project（2025年12月）
- [ToxicSkills: Malicious AI Agent Skills](https://snyk.io/blog/toxicskills-malicious-ai-agent-skills-clawhub/) — Snyk（2025年）公開スキルの36%にプロンプトインジェクション
- [IDEsaster: 30+ Vulnerabilities in AI IDEs](https://maccarita.com/posts/idesaster/) — Ari Marzouk（2025年12月）24 CVEs
- [Claude Code Security](https://www.anthropic.com/news/claude-code-security) — Anthropic（2026年2月）
- [Lessons from OWASP Top 10 for Agentic Applications](https://auth0.com/blog/owasp-top-10-agentic-applications-lessons/) — Auth0

**評価・品質**

- [Eval-Driven Development](https://evaldriven.org/) — EDD Manifesto
- [Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/) — LangChain
- [Building Effective AI Coding Agents for the Terminal](https://arxiv.org/abs/2603.05344) — arXiv（2026年3月）

**関連ツール**

- [Oxlint 1.0](https://voidzero.dev/) — ESLint比50-100倍高速、Rust製linter
- [Biome v2.0](https://biomejs.dev/) — フォーマッター+linter統合
- [Lefthook](https://github.com/evilmartians/lefthook) — Git hooksマネージャー
- [Hurl](https://hurl.dev/) — HTTP APIテスト（トークン効率最高）

</div>

---

<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 150px; margin-left: 80px; padding-left: 0; max-width: 70%;">

# ありがとうございました

### @nwiizo

</div>
