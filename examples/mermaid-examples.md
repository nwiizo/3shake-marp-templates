---
marp: true
theme: 3shake-standard-theme
paginate: true
html: true
mermaid: true
math: katex
style: |
  :root {
    --logo-url: url("./assets/images/3shake-logo.png");
  }
  .mermaid {
    display: flex !important;
    justify-content: center !important;
    margin: 2em auto !important;
    background: transparent !important;
  }
  .mermaid svg {
    height: auto !important;
    max-width: 100% !important;
  }
  /* 自動サイズ調整用のクラス */
  .mermaid-auto-xxxs {
    max-width: 10% !important;
  }
  .mermaid-auto-xxs {
    max-width: 15% !important;
  }
  .mermaid-auto-xs svg {
    max-width: 20% !important;
  }
  .mermaid-auto-sm svg {
    max-width: 35% !important;
  }
  .mermaid-auto-md svg {
    max-width: 50% !important;
  }
  .mermaid-auto-lg svg {
    max-width: 65% !important;
  }
  .mermaid-auto-xl svg {
    max-width: 80% !important;
  }
  /* レスポンシブ対応 */
  @media (max-width: 1280px) {
    .mermaid-auto-xxxs svg {
      max-width: 10% !important;
    }
    .mermaid-auto-xxs svg {
      max-width: 15% !important;
    }
    .mermaid-auto-xs svg {
      max-width: 20% !important;
    }
    .mermaid-auto-sm svg {
      max-width: 35% !important;
    }
    .mermaid-auto-md svg {
      max-width: 50% !important;
    }
    .mermaid-auto-lg svg, .mermaid-auto-xl svg {
      max-width: 85% !important;
    }
  }
---

# Mermaidダイアグラムの例

## 基本的なフローチャート
<pre class="mermaid mermaid-auto-xs">
flowchart TD
    A([開始]) --> B{条件分岐}
    B -->|Yes| C[処理1]
    B -->|No| D[処理2]
    C --> E([終了])
    D --> E
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#f9f,stroke:#333,stroke-width:2px
</pre>

---

# シーケンス図の例

<pre class="mermaid mermaid-auto-s">
sequenceDiagram
    participant User as ユーザー
    participant Front as フロントエンド
    participant Back as バックエンド
    participant Database as DB

    User->>Front: リクエスト送信
    Front->>Back: API呼び出し
    Back->>Database: データ取得
    Database-->>Back: 結果返却
    Back-->>Front: レスポンス
    Front-->>User: 画面表示
</pre>

---

# ガントチャートの例

<pre class="mermaid">
gantt
    title プロジェクトスケジュール
    dateFormat  YYYY-MM-DD
    
    section 計画フェーズ
    要件定義    :2025-01-01, 30d
    設計        :2025-02-01, 45d
    
    section 開発フェーズ
    実装        :2025-03-15, 60d
    テスト      :2025-05-15, 30d
    
    section リリースフェーズ
    デプロイ    :2025-06-15, 15d
    運用開始    :2025-07-01, 7d
</pre>

---

# クラス図の例

<pre class="mermaid mermaid-auto-xxs">
classDiagram
    class User {
        +String name
        +String email
        +login()
        +logout()
    }
    class Order {
        +int orderId
        +Date orderDate
        +process()
    }
    User "1" --> "*" Order: places
</pre>

---

# ERD（Entity Relationship Diagram）の例

<pre class="mermaid mermaid-auto-xxs">
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_ITEM : contains
    ORDER_ITEM }|--|| PRODUCT : references
</pre>

---

# マインドマップの例

<pre class="mermaid">
flowchart TD
    A((プロジェクト管理)) --> B(スコープ)
    A --> C(スケジュール)
    A --> D(リソース)
    A --> E(品質)
    B --> B1(要件定義)
    B --> B2(優先順位付け)
    C --> C1(マイルストーン)
    C --> C2(進捗管理)
    D --> D1(人員配置)
    D --> D2(予算管理)
    E --> E1(テスト計画)
    E --> E2(レビュー)
</pre>

<script type="module">
import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.4.1/dist/mermaid.esm.min.mjs';

// 初期化
mermaid.initialize({ 
  startOnLoad: true,
  theme: 'default',
  fontSize: 16
});

// ダイアグラムのサイズを自動調整する関数
function autoSizeMermaidDiagrams() {
  // すべてのMermaidダイアグラムを取得
  const diagrams = document.querySelectorAll('.mermaid');
  
  diagrams.forEach(diagram => {
    // SVG要素が生成されるまで待機
    const observer = new MutationObserver((mutations, obs) => {
      const svg = diagram.querySelector('svg');
      if (svg) {
        // 監視を停止
        obs.disconnect();
        
        // ダイアグラムの種類を判定
        let diagramType = '';
        let sizeClass = '';
        
        // ダイアグラムのテキストコンテンツを取得
        const content = diagram.textContent.toLowerCase();
        
        // ダイアグラムの種類と複雑さに基づいてサイズクラスを決定
        if (content.includes('flowchart') || content.includes('graph')) {
          // ノード数に基づいてサイズを決定
          const nodeCount = (content.match(/\[|\]|\(|\)|\{|\}/g) || []).length;
          if (nodeCount < 10) {
            sizeClass = 'mermaid-auto-xs';
          } else if (nodeCount < 20) {
            sizeClass = 'mermaid-auto-sm';
          } else {
            sizeClass = 'mermaid-auto-md';
          }
        } else if (content.includes('sequencediagram')) {
          // 参加者の数に基づいてサイズを決定
          const participantCount = (content.match(/participant/g) || []).length;
          sizeClass = participantCount <= 3 ? 'mermaid-auto-md' : 'mermaid-auto-lg';
        } else if (content.includes('gantt')) {
          // セクション数に基づいてサイズを決定
          const sectionCount = (content.match(/section/g) || []).length;
          sizeClass = sectionCount <= 2 ? 'mermaid-auto-lg' : 'mermaid-auto-xl';
        } else if (content.includes('classDiagram')) {
          // クラス数に基づいてサイズを決定
          const classCount = (content.match(/class /g) || []).length;
          sizeClass = classCount <= 2 ? 'mermaid-auto-sm' : 'mermaid-auto-md';
        } else if (content.includes('erdiagram')) {
          // エンティティ数に基づいてサイズを決定
          sizeClass = 'mermaid-auto-sm';
        } else {
          // その他のダイアグラム
          sizeClass = 'mermaid-auto-md';
        }
        
        // サイズクラスを適用
        diagram.classList.add(sizeClass);
      }
    });
    
    // DOM変更の監視を開始
    observer.observe(diagram, { childList: true, subtree: true });
  });
}

// ページ読み込み完了後に実行
window.addEventListener('load', () => {
  // 少し遅延させて実行（Mermaidのレンダリング完了を待つ）
  setTimeout(autoSizeMermaidDiagrams, 500);
});
</script>