---
name: it-news-digest
description: "ユーザーが「IT情報を収集して」「海外のニュースを調べて」「最新のAI情報を集めて」と言ったとき、またはループの自動継続で /it-news-digest が呼ばれたときに必ずこのスキルを使う。WebSearchで海外IT・AI最新情報を3カテゴリ並列収集し、D:\\it-news-digest\\topics\\ にMarkdownで保存してGitHubにpushする。"
---

# IT News Digest

海外IT・AI最新情報を自動収集してGitHubにプッシュするスキル。
WebSearchで3カテゴリを並列収集し、topics/フォルダにMarkdownで保存・git commit・git pushを繰り返す。

---

## いつこのスキルを使うか

- 「IT情報を収集して」「海外のニュースを調べて」と言われたとき
- 「最新AI情報を集めて」「it-news-digestを実行して」と言われたとき
- ScheduleWakeupによる自動継続で `/it-news-digest` が呼ばれたとき
- 「情報収集を再開して」と言われたとき

**使わないケース:**
- 単純な質問への回答のみでよい場合
- 日本国内ニュースのみを求めている場合

---

## ワークフロー

### Step 1: 収集状況を確認する

```bash
ls D:/it-news-digest/topics/
```

今日の日付（YYYY-MM-DD）で `overseas*.md` が何番まで作成済みかを確認し、次の番号（N+1）を決定する。

### Step 2: 未収集カテゴリから3つ選ぶ

以下のローテーションから本日まだ収集していないものを3つ選択：

- AI Models（LLM新モデル・ベンチマーク）
- AI Tools（開発者向けツール・エージェントフレームワーク）
- Cloud & Infra（AWS/GCP/Azure新機能・コスト動向）
- Security（脆弱性・PQC・インシデント）
- Open Source（注目リポジトリ・フレームワーク）
- Startups & Funding（資金調達・買収）
- AI Policy（各国規制・政策）
- Hardware & Chips（GPU・半導体）
- Enterprise SaaS（業務ソフトAI統合）
- Robotics & 自動運転
- Web3 & Blockchain
- Developer Experience（AIコーディングツール）
- Biotech & ヘルスケアIT
- Space Tech
- Fintech & 決済
- Education Tech
- Climate Tech & GreenIT
- Gaming & XR

### Step 3: WebSearchを3件並列実行

選んだ3カテゴリのWebSearchを**同時に**呼び出す（並列実行）。クエリに今日の日付（年月）を含める。

### Step 4: ファイルを作成して保存

`D:\it-news-digest\topics\YYYY-MM-DD_overseasN.md` として保存。

フォーマット:
```markdown
# 海外 IT・AI 情報まとめ Vol.N — YYYY-MM-DD
収集カテゴリ: カテゴリA / カテゴリB / カテゴリC

## [絵文字] カテゴリA
（表・箇条書きで整理）
### ソース
- [タイトル](URL)

## [絵文字] カテゴリB
...

## [絵文字] カテゴリC
...

## 📌 Vol.N 3行まとめ
1. **見出し**: 内容
2. **見出し**: 内容
3. **見出し**: 内容
```

### Step 5: git commit & push

```bash
git -C D:/it-news-digest add .
git -C D:/it-news-digest commit -m "feat: YYYY-MM-DD 海外IT Vol.N — カテゴリA/B/C

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
git -C D:/it-news-digest push
```

### Step 6: ScheduleWakeupで次サイクルへ

```
ScheduleWakeup(
  delaySeconds: 270,
  reason: "270秒後に次カテゴリの海外IT情報収集サイクルを実行",
  prompt: "/it-news-digest"
)
```

---

## エラー対処

| エラー | 対処法 |
|--------|--------|
| git push 失敗 | `git -C D:/it-news-digest remote add origin https://github.com/liuxi4048-crypto/it-news-digest.git` |
| git config 未設定 | `git -C D:/it-news-digest config user.email "liuxi4048@gmail.com"` を実行 |
| 全カテゴリ収集済み | `daily/YYYY+1-MM-DD.md` を新規作成して翌日分へ |
