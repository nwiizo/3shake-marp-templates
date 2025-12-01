# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Marp-based presentation template repository for 3-SHAKE Inc. It provides branded templates, themes, and tools for creating consistent, professional presentations using Markdown.

## Common Development Commands

### Development Server
```bash
# Start local server with live reload (default port 8080)
npm start

# Start with preview mode
npm run preview
```

### Building Presentations
```bash
# Convert to PDF (if marp is installed locally)
marp slides/[presentation].md --pdf --allow-local-files

# Convert to PDF (using npx)
npx @marp-team/marp-cli@latest slides/[presentation].md --pdf --allow-local-files

# Convert to PowerPoint
marp slides/[presentation].md --pptx --allow-local-files

# Convert to HTML
marp slides/[presentation].md --html --allow-local-files
```

### Watch Mode (Live Reload)
```bash
# Watch mode with automatic browser opening
marp slides/[presentation].md --html --allow-local-files --watch & open slides/[presentation].html

# Example for specific presentation
marp slides/2025/claude-code-beyond.md --html --allow-local-files --watch & open slides/2025/claude-code-beyond.html
```

### Neovim Integration (marp.nvim)
```bash
# Setup marp.nvim plugin (lazy.nvim)
{
  'nwiizo/marp.nvim',
  ft = "markdown",
  config = function()
    require("marp").setup {
      marp_command = "/path/to/marp",
      server_mode = false
    }
  end,
}

# Key commands within Neovim:
# :MarpWatch - Start live preview
# :MarpStop - Stop watching
# :MarpExport [format] - Export to various formats
# :MarpTheme [theme] - Switch themes
```

### Testing Mermaid Diagrams
```bash
# Preview with Mermaid support
npm start -- --html
```

## High-Level Architecture

### Directory Structure Purpose
- **`templates/`**: Base presentation templates to copy when creating new presentations
- **`themes/`**: CSS themes that define visual styling and layout rules
- **`slides/`**: Actual presentations, organized by year
- **`assets/images/`**: Shared images, organized by year and presentation

### Template Overview

| Template | Purpose | Features |
|----------|---------|----------|
| **3shake-standard-template.md** | Full-featured | Self-intro, company intro, hiring, layout examples, code blocks |
| **basic.md** | Simple | Agenda, sections, summary - basic structure |
| **with-logo.md** | Standard + image layouts | Left/right image placement, 2-column layouts |
| **background-template.md** | Minimal | Minimum structure for quick start |

### Common Template Features

All templates include:
- **Frontmatter**: `math: mathjax`, `mermaid: true` enabled
- **Auto logo display**: Logo automatically placed at bottom-left on content slides
- **Title/ending slides**: Unified dark theme design
- **CSS classes**:
  - `.info-box` - Information box styling
  - `.highlight-blue/.highlight-green/.highlight-yellow` - Text highlighting
  - `.reference-right` - Right-aligned reference styling
  - `.hidden` - Hidden elements
  - `.author-info` - Author information styling

### Theme System
The theming system uses CSS custom properties and class-based styling:
- `3shake-theme.css`: Primary theme with brand colors (#4AADDD, #0a1929, #ECBE30)
- Auto-injects company logo via CSS `::before` pseudo-elements on `section:not(.title)`
- Supports custom classes for different slide layouts (`title`, `dark`, etc.)

### Mermaid Integration
Mermaid diagrams are supported with responsive sizing through custom CSS classes:
- `.mermaid-xs`: Extra small diagrams (200px max height)
- `.mermaid-sm`: Small diagrams (300px max height)
- `.mermaid-md`: Medium diagrams (500px max height)
- `.mermaid-lg`: Large diagrams (700px max height)
- `.mermaid-xl`: Extra large diagrams (900px max height)

### Citation System
Uses superscript notation with automatic formatting:
```markdown
Important claim^[1]^
```
Renders with proper styling for academic/professional citations.

## Key Technical Details

### Marp Configuration
- **HTML enabled**: Required for Mermaid diagrams and advanced formatting
- **Local files allowed**: Necessary for accessing theme CSS and images
- **Theme inheritance**: Custom themes extend Marp's default theme

### Editor Integration
The repository supports multiple editors:

**VS Code Integration:**
- Enable HTML in Markdown preview
- Configure Marp preview settings
- Set up proper file associations

**Neovim Integration (marp.nvim):**
- Live preview with `:MarpWatch`
- Export functionality with `:MarpExport [format]`
- Theme switching with `:MarpTheme [theme]`
- Auto-cleanup on buffer close
- Support for HTML, PDF, PPTX, PNG, JPEG formats

### Asset Management
Images are organized by year and presentation to maintain clarity:
```
assets/images/2025/[presentation-name]/[image-name].png
```

### 画像の貼り方（Marp）

Marpで画像を表示する際、`<div>` タグの中にMarkdown形式の画像（`![alt](path)`）を入れると表示されないことがある。**HTMLの `<img>` タグを使用する**。

**推奨: HTMLのimgタグを使用**
```markdown
<div style="text-align: center;">
<img src="../../assets/images/2025/presentation-name/image.png" alt="説明" style="max-width: 80%; height: auto;">
</div>
```

**NG: div内でMarkdown画像記法**
```markdown
<div style="background-color: #f5f5f5; padding: 15px;">

![画像](../../assets/images/2025/presentation-name/image.png)

</div>
```

**画像配置の手順:**
1. 画像ディレクトリを作成: `mkdir -p assets/images/2025/[presentation-name]/`
2. 画像を配置: `mv /path/to/image.png assets/images/2025/[presentation-name]/`
3. スライドで参照: `<img src="../../assets/images/2025/[presentation-name]/image.png" ...>`

## Development Patterns

### Creating New Presentations
1. Copy appropriate template from `templates/`
2. Place in `slides/[year]/[presentation-name].md`
3. Create corresponding image directory if needed: `assets/images/[year]/[presentation-name]/`
4. Update theme path in frontmatter: `theme: ../../themes/3shake-theme.css`
5. Customize required sections:
   - Title slide (title, subtitle, event, author)
   - Self-introduction (if using standard/with-logo template)
   - Content slides
   - Ending slide (author, contact)

### Theme Customization
When modifying themes:
- Brand colors are defined as CSS variables in `:root`
- Logo placement is handled by `section:not(.title)::before`
- Custom slide layouts use class-based selectors
- Title slides use `_class: title dark` directive

### Standard Slide Layouts

#### Title/Ending Slide Pattern
```markdown
<!--
_backgroundColor: #0a1929
_color: white
_class: title dark
-->

![bg](../../assets/images/3shake-background-full.png)

<img src="../../assets/images/3shake-logo.png" alt="3-SHAKE logo" style="position: absolute !important; top: 100px !important; left: 100px !important; width: 240px !important; height: auto !important; z-index: 9999 !important;">

<div class="title" style="text-align: left; margin-top: 100px; margin-left: 20px; padding-left: 0; max-width: 70%;">

# Title

### Subtitle

</div>

<div class="author-info" style="text-align: left; padding-left: 0; text-indent: 0;">
2025/XX/XX Event Name</br>
@your_name Duration
</div>
```

#### 2-Column Layout
```markdown
## Section Title

<div style="display: flex; gap: 40px;">
<div style="flex: 1;">

**Left Column**
- Item 1
- Item 2

</div>
<div style="flex: 1;">

**Right Column**
- Item A
- Item B

</div>
</div>
```

#### Image + Text Layout
```markdown
## Title

<div style="display: flex; gap: 40px;">
<div style="width: 35%;">
<img src="path/to/image.png" alt="description" style="width: 100%; height: fit-content;">
<div style="font-size: 0.7em; text-align: left; margin-top: 5px;">
Image description or source
</div>
</div>

<div style="flex: 1;">
Main content</br></br>

1. **Point 1**
2. **Point 2**
</div>
</div>
```

#### Background Image (Left/Right)
```markdown
<!-- Left background image -->
![bg left:30% fit](path/to/image.jpg)
## Title

Content

<!-- Right background image -->
![bg right:30% 80%](path/to/image.png)
## Title

Content
```

#### Reference Right-Aligned
```markdown
<div class="reference-right">
Reference: Source name or URL
</div>
```

### Mermaid Diagram Best Practices
- Always wrap diagrams in HTML divs with sizing classes
- Use HTML comments to hide Marp directives from Mermaid
- Test diagrams in preview mode before building
- Consider using images instead for complex diagrams (better quality)

## Presentation Creation Best Practices

### Content Strategy
- **Follow the narrative arc**: Introduction -> Problem -> Solution -> Conclusion
- **One message per slide**: Keep each slide focused on a single concept
- **Use concrete examples**: Replace abstract concepts with specific data and real-world examples
- **Audience-appropriate content**: Adjust technical depth based on audience expertise

### Visual Design Principles
- **Leverage contrast**: Use color, size, and positioning to highlight key information
- **Embrace white space**: Avoid cramming too much content; prioritize readability
- **Maintain consistency**: Standardize fonts, colors, and layouts throughout
- **Strategic image use**: Combine text with relevant visuals for better retention

### Color Scheme for Slide Boxes
カラフルな背景色は安っぽく見えるため、シンプルな階層構成を使用する：

**3段階の強調レベル:**
- **通常**: `#f5f5f5`（薄いグレー）- 通常のボックス、セクション背景
- **強（スライド内で強い）**: `#e0e0e0`（濃いグレー）- 💡ヒントボックスなど
- **最強（本当に強いメッセージ）**: `#e0e0e0` + `color: #e65100`（オレンジ文字）+ `font-size: 1.3em`

```markdown
<!-- 通常のボックス -->
<div style="background-color: #f5f5f5; padding: 15px; border-radius: 8px;">
内容
</div>

<!-- 強（ヒントボックス等） -->
<div style="background-color: #e0e0e0; padding: 10px; border-radius: 5px; text-align: center;">
💡 <strong>スライド内で強調したいメッセージ</strong>
</div>

<!-- 最強（パンチライン） -->
<div style="background-color: #e0e0e0; padding: 15px; border-radius: 8px; text-align: center; font-size: 1.3em;">
<span style="color: #e65100; font-weight: bold;">最も伝えたいメッセージ</span>
</div>
```

**避けるべき色**:
- `#ffebee` (ピンク), `#e8f5e9` (緑), `#fff3e0` (オレンジ背景), `#fff0f5` (ラベンダー)
- `#e3f2fd` (薄い青), `#fff3cd` (黄色) などの複数色の組み合わせ
- 背景色で強調するのではなく、テキスト色で強調する

### Highlight Usage
```markdown
<span class="highlight-blue">Important keywords</span>
<span class="highlight-green">Positive content</span>
<span class="highlight-yellow">Warnings/notes</span>
```

### Performance and Organization
- **Optimize image sizes**: Use appropriately sized images for web delivery
- **Maintain file structure**: Follow `assets/images/year/presentation-name/` convention
- **Choose appropriate themes**: Select themes that match presentation context
- **Test across formats**: Verify appearance in HTML, PDF, and PPTX outputs
- **HTML/CSS flexibility**: Leverage direct HTML and CSS for advanced layouts beyond standard Markdown

### Time Management Guidelines
- **1-2 minutes per slide**: Calculate total slides based on presentation duration
- **Include buffer time**: Account for Q&A and potential technical issues
- **Practice with timing**: Rehearse presentation to validate pacing

### Engagement Strategies
- **Repeat key messages**: Reinforce important points multiple times
- **Interactive elements**: Include polls, questions, or discussion prompts
- **Backup materials**: Prepare appendix slides for detailed Q&A responses

## Presentation Flow and Transition Best Practices

### セクション間の繋ぎスライドを必ず入れる

プレゼンテーションでセクションが変わる際、唐突に新しいトピックに入らない。**繋ぎスライド**を入れて文脈を繋げる。

**繋ぎスライドの構成要素:**
1. **前セクションの要約/確認**: 「〜はわかった」「〜が決まった」
2. **次セクションへの疑問/課題提起**: 「では、〜は？」
3. **予告**: 次に何を紹介するかを明示

**繋ぎスライドのテンプレート（濃い背景版）:**
```markdown
---

<!--
_backgroundColor: #0a1929
_color: white
-->

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 80%; text-align: center;">

<div style="font-size: 1.5em; font-weight: bold;">

前セクションの確認。では、次の問い？

</div>

<div style="font-size: 0.9em; margin-top: 30px; color: #aaa;">

補足説明や文脈<br/>
→ 次に紹介する内容の予告

</div>

</div>

---
```

**繋ぎスライドのテンプレート（通常背景版）:**
```markdown
---

## 前セクションの確認、次への問い

### サブタイトル（課題や疑問）

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**よくある悩み/課題**

- 悩み1
- 悩み2
- 悩み3

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**解決策の予告**

次に紹介する内容の概要

→ 具体的に何を紹介するか

</div>
</div>

</div>

---
```

### 繋ぎが必要な典型的パターン

| パターン | 繋ぎの例 |
|---------|---------|
| Why → What | 「価値はわかった。では、何を？」 |
| What → How | 「ネタは見つかった。では、どう？」 |
| How → Practice | 「書き方はわかった。あとは実践」 |
| 概念説明 → 具体的手法 | 「でも、〜だけじゃダメ」→ 手法の紹介へ |
| 書き方 → 公開の不安 | 「書けた。でも、公開が怖い」 |
| 失敗パターン → ツール活用 | 「〜を使えばいいのでは？」→ 正しい使い方へ |

### 唐突な主張を避ける

**NG例:**
- いきなり「〇〇とは」で始まる（なぜその話をするのか文脈がない）
- 前のスライドと関係なく新しいトピックに入る
- 読者/聴衆が「なぜ今この話？」と思う展開

**OK例:**
- 前スライドで課題を提示 → 「その解決策として〇〇を紹介」
- 「よくある悩み」を先に見せる → 「それを解決する方法」
- 「〜という疑問に答えておく」と前置きしてから本題へ

### 目次（今日お話しすること）との整合性

目次スライドの内容と実際のスライド構成を必ず一致させる。構成を変更したら目次も更新する。

**チェックポイント:**
- 目次の各項目がスライド内で実際にカバーされているか
- スライドの順番と目次の順番が一致しているか
- 繋ぎスライドを追加した場合、目次の表現も適切か

### スライド分割のルール

1. **溢れたら分割する**（圧縮・削除ではなく）
2. **フォントサイズは0.75emを基準**に（コンパクトな場合は0.7em）
3. **1スライド1メッセージ**を守る
4. **繋ぎスライドは別カウント**として扱う（内容スライドではない）

### 「この発表で解決できること」スライドの重要性

プレゼンテーションの冒頭（目次の後、本編の前）に「この発表で解決できること」を入れる。

**含めるべき要素:**
- 対象者の悩み（「こんな悩みを持っていませんか？」）
- 持ち帰れるもの（「この30分で得られること」）
- 明確な目標（「この発表を聞いた人が〜する」）

```markdown
## この発表で解決できること

### サブタイトル

<div style="font-size: 0.75em;">

<div style="display: flex; gap: 20px; margin-top: 15px;">
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**こんな悩みを持っていませんか？**

- 悩み1
- 悩み2
- 悩み3

</div>
<div style="flex: 1; background-color: #f5f5f5; padding: 15px; border-radius: 8px;">

**この発表で持ち帰れるもの**

- 得られること1
- 得られること2
- 得られること3

</div>
</div>

<div style="margin-top: 15px; padding: 12px; background-color: #e0e0e0; border-radius: 5px; text-align: center;">
<span style="color: #e65100; font-weight: bold;">目標：〜</span>
</div>

</div>
```

## プレゼンテーション構成の原則

### 全体構成の設計パターン

プレゼンテーションは以下の構造で設計する：

```
1. タイトル
2. 自己紹介（必要に応じて）
3. 目次（今日お話しすること）
4. この発表で解決できること ← 重要
5. 本編（セクション × N）
   - 各セクション間に繋ぎスライドを入れる
6. まとめ
7. 終了スライド
```

### 説得力のある主張の構成

**主張を通すための3ステップ:**

1. **反論を先に潰す**: 聴衆が持ちそうな「でも〜」を先回りして否定
2. **損失を示す**: やらないことのコスト・機会損失を直視させる
3. **利益を示す**: やることで得られるリターンを具体的に提示

**テンプレート例（「なぜ〇〇すべきか」の構成）:**

```markdown
## スライド1: 「〇〇しない理由」を全部潰す

❌「時間がない」 → 反論
❌「難しそう」 → 反論
❌「自分には早い」 → 反論

## スライド2: 「〇〇しない」という選択のコスト

- 失われるもの1（学習効果、機会、etc）
- 失われるもの2
- 失われるもの3
- 失われるもの4

## スライド3: 〇〇がもたらす3つの価値

- 価値1（具体的なリターン）
- 価値2
- 価値3
```

### コンテンツの配置原則

**情報の種類と配置場所:**

| 情報の種類 | 配置場所 | 理由 |
|-----------|---------|------|
| Why（なぜやるべきか） | 本編の最初 | 動機づけが先 |
| What（何をするか） | Whyの後 | 動機が固まってから具体論 |
| How（どうやるか） | Whatの後 | 何をするか決まってから方法論 |
| 種類・分類 | Howの冒頭 | 方法を学ぶ前に選択肢を知る |
| 具体的テクニック | 種類の後 | 種類を選んでから詳細へ |
| 注意点・落とし穴 | テクニックの後 | 実践前に知っておくべきこと |
| 実践編 | 本編の最後 | 知識が揃ってから行動へ |

**配置を間違えやすいパターン:**

| NG | OK | 理由 |
|----|-----|------|
| Whyの中に種類・分類 | Howの冒頭に種類・分類 | 種類は「どうやるか」の一部 |
| いきなり具体的手法 | 課題提示→解決策として手法 | 文脈なしの手法は唐突 |
| 最後に「なぜやるべきか」 | 最初に「なぜやるべきか」 | 動機づけは最初に必要 |

### 聴衆の心理に寄り添う構成

**聴衆が各段階で持つ疑問:**

1. **冒頭**: 「この発表、自分に関係ある？」→ 解決できることを明示
2. **Why**: 「本当にやる必要ある？」→ 反論を潰し、損失と利益を示す
3. **What**: 「で、具体的に何するの？」→ ネタの見つけ方、発想法
4. **How**: 「どうやればいいの？」→ 種類、型、テンプレート
5. **実践前**: 「失敗したくない」→ 注意点、チェックリスト
6. **最後**: 「で、何から始める？」→ 具体的な次のアクション

**各疑問に答えるスライドを用意する。**

### 外部知識・フレームワークの導入パターン

書籍や理論を紹介する際の構成：

```
1. 課題の提示（「〜だけじゃダメ」「〜が必要」）
2. 解決策の予告（「〇〇というフレームワークを紹介」）
3. 著者/出典の紹介（信頼性の担保）
4. フレームワークの概要
5. 各ステップの詳細（本題への応用を含む）
6. まとめ（本題との関連を再確認）
```

**NG: いきなり「〇〇氏とは」で始める**
**OK: 課題→解決策として〇〇氏の理論を紹介→人物紹介**

### レビュー時のチェックリスト

プレゼンテーション完成後、以下を確認：

**構成チェック:**
- [ ] 目次と実際のスライド構成が一致しているか
- [ ] 各セクション間に繋ぎスライドがあるか
- [ ] 「この発表で解決できること」が冒頭にあるか
- [ ] Why → What → How の順番になっているか
- [ ] 種類・分類はHowセクションに配置されているか

**主張チェック:**
- [ ] 「なぜやるべきか」で反論を潰しているか
- [ ] やらないことのコストを示しているか
- [ ] やることのリターンを示しているか

**流れチェック:**
- [ ] 唐突に新しいトピックに入っている箇所はないか
- [ ] 外部知識の導入前に課題提示があるか
- [ ] 聴衆の疑問に順番に答えているか

**表現チェック:**
- [ ] 年号が古くなっていないか（例: 2024年→2025年）
- [ ] 繋ぎスライドに予告が含まれているか
- [ ] 各スライドが1メッセージになっているか
