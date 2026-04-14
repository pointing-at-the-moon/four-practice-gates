# NIAN 專屬新增條款
# NIAN-Specific Provisions for *Mindful of the Buddha*

> 本檔以 `dharma-research/PAPER_STYLE.md` v2.1 為基礎，新增以下 NIAN 專屬條款。
> 凡未列出的條款，一律沿用 v2.1 原文（署名、引用原則、論文結構、分析框架等），
> 但 **§3.7 RAG 流程已於 v2.2-NIAN 改寫**（見下方 §3.7 / §3.7.1）。

---

## NIAN-A：雙軌並行引用 Dual-Track Citation (Pāli + Chinese)

凡涉阿含／Nikāya 層，同時引 T-number（漢譯）與 PTS 編號（Pāli），讓讀者看見同一修法在兩傳統中的不同軌跡。

```markdown
格式範例：
- 漢譯：《中阿含經》卷X，《大正藏》第1冊，No. T0026。
- Pāli：MN X, Majjhima Nikāya, PTS edition.
```

**適用範圍**：Part I（P01–P04）為核心適用區；Part II–VIII 視引用內容決定是否啟動雙軌。

**原則**：漢譯為主體引文（符合中文讀者需求），Pāli 為學術對照（符合西方讀者期待）。兩者不必逐字對譯，但必須指向同一經文段落。

**操作備註（v2.2-NIAN）**：Chat Opus 現可直接 grep `~/Projects/tripitaka/vaults/vault-pali/`，Pāli 端查證與漢譯端同等便利，雙軌引用不再是額外成本。

---

## NIAN-B：西方讀者優先 Framing Western-Reader-First Framing

每篇引言段落須明確回應一個**西方佛教現場問題**，而非台灣念佛人熟悉的教義問題。

**有效的西方現場問題範例**：
- McMindfulness：正念被商品化後，念佛（recollection of the Buddha）還剩什麼？
- 治療化（therapeuticization）：把念佛當心理治療工具，丟失了什麼？
- 去宗教化（secularization）：抽掉「佛」這個對象，「念」還能獨立運作嗎？
- AI 靈性化（AI spiritualization）：用 AI 輔助念佛，邊界在哪裡？

**反面範例（不要這樣開頭）**：
- ❌「淨土宗自東晉慧遠以來...」（中國佛教史敘事，非西方讀者痛點）
- ❌「念佛法門殊勝難思...」（教內勸修語氣，非學術分析）

---

## NIAN-C：跨宗派取材紀律 Cross-Sectarian Discipline

- **Part V（P12–P19）** 八部大乘經：**不預設任何宗派立場**。每部經以經文自身呈現念佛面向，不用淨土、禪、天台任何一家的詮釋框架。
- **Part VI（P20–P24）** 才正式處理宗派分流，屆時兩線交織並行。
- **Part VII（P25–P29）** 華嚴收攝：以經文（T0278/T0279/T0293）為主，法藏／澄觀疏論為輔。

---

## NIAN-D：S5 楞伽段落避讓 Laṅkāvatāra Segment Avoidance

T0670/T0671/T0672《楞伽經》在兩本書中使用**不同段落**：

| 書 | 使用段落 | 用途 |
|----|---------|------|
| **S5（八部曲）** | 「心如工伎兒，意如和伎者，五識為伴侶，妄想觀伎眾」 | 第六意識 kōan 材料 |
| **NIAN P17** | 「諸佛心第一」、念佛即念心、達摩印心脈絡 | 如來藏／念心雙證 |

**寫 P17 時須嚴格避開 S5 預留的觀伎眾段落**，否則會劇透未寫的 S5 內容。

**v2.2-NIAN 補充**：直接 grep vault 時，若搜尋 T0670 命中觀伎眾段落，須主動跳過，不得引入 P17 草稿。此為寫作紀律，非技術限制。

---

## NIAN-E：GRETIL 授權隔離 GRETIL License Isolation

vault-pali 中的 Visuddhimagga 檔案為 CC BY-SA 4.0（GRETIL），已標 `publication_safe: false`。

**規則**：NIAN book 引 Visuddhimagga 需用 Max 自譯或公有領域版本，不可直接引用 GRETIL 文本到出版物中。Paper 階段可用作研究參考，但 book 階段需替換。

**v2.2-NIAN 補充**：Chat Opus 直接存取 vault 後，讀取任何 Visuddhimagga 檔案前，**必須先檢查 frontmatter 的 `publication_safe` 欄位**。若為 `false`，僅用於研究理解，草稿中的對應引文須標 `[待替換：GRETIL 隔離]`，交 Code Opus 於 book 階段處理。

---

## NIAN 雙 Claude 分工（v2.2-NIAN 更新）Dual-Claude Division of Labor

自 v2.2-NIAN 起，RAG 擁有權從 Code Opus 移交給 Chat Opus：

| 角色 | 職責 |
|------|------|
| **Chat Opus** | 全部智識工作、RAG（直接 grep vault）、引文查證、章節起草、雙語改寫、passdown 撰寫 |
| **Code Opus** | git 操作、commit、punct_fix.py、build.sh、GitHub push、passdown 歸檔、批次腳本 |

**關鍵轉變**：Chat Opus 不再「請 Code Opus 檢索後帶回」；Chat 自己負責所有經文查證。Code Opus 從檢索執行者變為純工程執行者。

---

# 以下為 dharma-research PAPER_STYLE.md v2.1 原文（§3.7 已於 v2.2-NIAN 改寫）

---

# 指月研究論文風格指南
# Point-to-the-moon Paper Style Guide

Version: 2.2-NIAN | 2026-04

---

## 一、署名規範 Attribution

所有論文統一署名：

```
作者 Author: 釋慧鏡 (Shi Huijing)
Project: 指月 Point-to-the-moon
授權 License: CC BY-NC-SA 4.0
```

不使用個人法名、寺院頭銜、宗派傳承。法不屬於任何人，研究以內容本身說話。

> 正法眼藏，不立文字——法的價值在法本身，不在說法者的身份。

---

## 二、引用原則 Reference Policy

### 2.1 佛教典籍：僅用大正藏原典

**必須引用：** 大正藏（Taishō Tripiṭaka）原典，標明冊號與編號。

```
格式：《經論名》，造/譯者，《大正藏》第X冊，No. XXXX。
範例：《成唯識論》，護法等菩薩造，唐玄奘譯，《大正藏》第31冊，No. 1585。
```

所有經論引文須經 CBETA 電子佛典集成校對，並附 CBETA wiki-link（如有）。

**不引用：** 近現代中文佛教學者的詮釋性著作。原因：

- 近現代漢傳佛教學術圈存在宗派立場爭議，引用任何一家都可能引發非學術性的爭論
- 本研究直接以原典為依據，讀者可自行驗證，不需要經過任何中間詮釋者
- 這也是「諸法實相」開源精神的體現——回到源頭，如實呈現

**例外：** 古代祖師的注疏（如窺基《成唯識論述記》、澄觀《華嚴經疏》）屬於經典傳統的一部分，可以引用，視同原典層級。

### 2.2 現代學術著作：英文為主

現代學科（心理學、神經科學、物理學、哲學、複雜系統理論等）的引用，以英文同行評審學術著作為主。

```
格式：Author, A. B. "Title." Journal/Book, Publisher, Year.
範例：Vaswani, A. et al. "Attention Is All You Need." Advances in Neural Information Processing Systems 30, 2017.
```

原因：

- 心理學（CBT/ACT）、神經科學、AI 研究等領域的原始文獻幾乎全部以英文發表
- 英文學術著作有明確的同行評審機制，品質可驗證
- 面向國際讀者，英文引用直接可查

### 2.3 引用的三層分類

參考文獻統一分為三類：

```markdown
## 參考文獻

### 一、佛教原典 Primary Buddhist Sources
（大正藏原典 + 古代祖師注疏）

### 二、現代學術著作 Modern Scholarship
（英文同行評審學術著作）

### 三、經論索引 Sūtra & Śāstra Index
（表格形式，列出本文引用的所有經論簡稱、全名、大正藏編號、造/譯者）
```

---

## 三、論文結構 Paper Structure

### 3.1 標題區 Header Block

```markdown
# 中文主標題

**中文副標題**

*English Main Title:
English Subtitle*

---

**作者 Author:** 釋慧鏡 (Shi Huijing)
**日期 Date:** 2026年X月
**授權 License:** CC BY-NC-SA 4.0
**版本 Version:** 1.0

---
```

### 3.2 摘要 Abstract

雙語呈現，先中後英。摘要應包含：核心命題、論證路線、主要結論。

**三句話結構**（每句可展開為 2-3 行，總計中文 200-300 字，英文 150-250 words）：

1. **問題**：本文要回答什麼問題，為什麼這個問題重要
2. **論證**：本文如何論證（用什麼經典依據、什麼分析框架）
3. **結論**：核心發現是什麼，對修行或理解有什麼意義

```markdown
## 摘要 Abstract

（中文摘要，200-300字，三句話結構）

（English abstract, 150-250 words）

**關鍵詞 Keywords:** 中文詞1、中文詞2 / English term 1, English term 2
```

### 3.3 正文章節 Body Sections

主節用中文數字（一、二、三），子節用阿拉伯數字（2.1, 2.2, 2.3）。

```markdown
## 一、問題意識

正文……

## 二、核心論證

### 2.1 第一層論證

正文……

### 2.2 第二層論證

正文……
```

理由：主節的中文數字保持學術中文的傳統感，子節的阿拉伯數字便於跨語言引用和精確指涉。

### 3.4 必備章節 Required Sections

每篇論文必須包含以下章節（順序固定）：

| 順序 | 章節 | 說明 |
|------|------|------|
| 1 | 問題意識 / 引言 | 為什麼這個問題重要，本文要回答什麼。**第一段必須有鉤子**（見下方說明） |
| 2 | 經典依據 | 大正藏原典引文，建立義理基礎 |
| 3 | 核心論證 | 本文的原創分析，可分多節 |
| 4 | 跨學科會通 | 現代學科的結構性類比。須有深度（見下方說明），否則寧可不寫 |
| 5 | 中道校正 | 防止過度詮釋，標明命題的邊界與限制 |
| 6 | 結論 | 核心論點精要 + **修行指引** + **開放問題**（見下方說明） |
| 7 | 參考文獻 | 三層分類（見上） |
| 8 | 經論索引 | 表格形式，列出本文引用的所有經論 |
| 9 | 系列論文交叉引用索引 | P04 起，標明與其他論文的義理關聯 |
| 10 | 腳注 | 集中在文末，以「## 腳注」為標題 |
| 11 | Footer | CC BY-NC-SA 4.0 + CBETA 校對 + GitHub（3 行） |

**中道校正**是本研究的方法論特色，每篇必須有。至少包含：
- 防止義理面的過度延伸
- 防止跨學科類比的還原主義
- 標明「啟發性類比」(heuristic) vs「理論等價」(reductive) 的邊界

### 3.4.1 問題意識的鉤子 Opening Hook

問題意識的**第一段**必須有一個鉤子（hook），讓讀者立刻產生共鳴。**NIAN 的目標讀者是西方英語佛教讀者**——鉤子必須回應西方佛教現場問題（見 NIAN-B 條款）。

**有效的鉤子類型：**
- **日常場景**：「You sit down to meditate and reach for your phone instead—sound familiar?」
- **反直覺問題**：「What if 'mindfulness of the Buddha' has nothing to do with what you think mindfulness means?」
- **修行困惑**：「Why does recollection practice feel so different from bare attention?」

**避免的開頭：** 直接從定義或學術背景開始。

### 3.4.2 跨學科會通的深度要求 Interdisciplinary Depth

跨學科會通不是「點到為止的類比」。如果要寫，必須有三層結構：

1. **相似處**：佛法的 X 概念與現代的 Y 理論在什麼維度上相似
2. **根本差異**：但它們在什麼層面上不同（通常是：目的不同、本體論不同、修行 vs 治療）
3. **不可還原聲明**：明確說明這是「啟發性類比」(heuristic)，不是「理論等價」(reductive)

**字數門檻：** 至少 500 字。如果找不到有深度的類比，**直接砍掉整個章節**。

### 3.4.3 結論的三層結構 Conclusion Structure

結論不是摘要的重複。必須包含三層：

1. **精要重述**（2-3 句）：本文的核心發現
2. **修行指引**（so what for practice）：這個義理對日常修行有什麼具體啟發
3. **開放問題**：留一個問題給下一篇論文或讀者自己思考

### 3.5 腳注 Footnotes

使用 `[^n]` 格式。腳注用於：
- 經文引用的出處標註
- 學術異說的標注
- 跨體系引用的說明
- 術語的精確定義

不用腳注做一般性的補充說明——那些內容應該寫進正文。

**位置規則：** 腳注定義全部集中在文末，以 `## 腳注` 為標題。**不得散落在各章節內。**

**格式規則：**

```markdown
[^1]: 《經論名》卷X，「引文原文片段」，引文脈絡說明，《大正藏》第X冊，No. XXXX。
```

### 3.6 經文引用格式 Sūtra Quotation Format

經文引用使用 blockquote，引用後標明出處腳注：

```markdown
> 「初發心時，便成正覺，知一切法，真實之相。」[^1]

[^1]: 《大方廣佛華嚴經》卷十七，初發心功德品第十七，《大正藏》第10冊，No. 278。
```

梵文術語首次出現時附梵文原文，格式：中文（梵文），如：作意（manaskāra）。
**Pāli 術語同理**，格式：中文（Pāli），如：隨念（anussati）。

---

## 三之二、寫作方法論 Writing Methodology

### 3.7 RAG 先行原則 RAG-First Principle（v2.2-NIAN 改寫）

每篇論文開寫前，**必須先由 Chat Opus 直接 grep vault 進行系統性查詢**，完成所有經文定位與引文查證，方可進入草稿階段。

**技術背景的轉變：**
- v2.1 以前：Chat Opus 提出查詢 → Code Opus 以 sub-agent 平行 grep → 結果回傳 → Chat 撰寫
- v2.2-NIAN 起：Chat Opus 透過 Filesystem MCP 直接存取 `~/Projects/tripitaka/vaults/`，grep 與 view 在同一回合內完成

**零幻覺原則不變**：所有引文來自 vault markdown 實際內容，不得憑記憶重建 T-number 或經文段落。

**Vault 路徑（當前掛載）：**
- 漢譯大藏經：`~/Projects/tripitaka/vaults/vault-taisho/`
- Pāli Nikāya：`~/Projects/tripitaka/vaults/vault-pali/`
- Fallback（vault 未收錄時）：`/Users/master/Projects/tripitaka/xml-p5-master/`（CBETA XML-P5 完整下載）

### 3.7.1 Vault 存取紀律 Vault Access Discipline（v2.2-NIAN 新增）

直接存取權限較高，紀律須同步收緊：

1. **先 grep 後 view**：不得對經文檔案盲目 full-read。先用 `grep -rn` 定位段落，再以 `view` 配合 line range 讀取上下文。
2. **引文必經眼驗證**：草稿中任何 T-number、任何經文引文，**必須在當次 session 實際打開對應 vault 檔案確認過**。不得依賴記憶、不得依賴過去 session 的快取、不得依賴 userMemories 中的摘要。
3. **查詢紀錄寫入 passdown**：每篇論文的主要 grep 查詢（關鍵詞、命中檔案、採用段落）須列於該篇 passdown 的「RAG queries」章節，供 Code Opus 日後審計與重現。
4. **Frontmatter 必檢**：讀取 vault 檔案後，先看 frontmatter 的 `publication_safe` 欄位。`false` 者僅供研究理解，不得直接引入草稿（見 NIAN-E）。
5. **Fallback 觸發條件**：vault 未命中時，才可降至 `xml-p5-master` 的原始 CBETA XML 檔。XML 讀取需回寫成 vault markdown 或至少在 passdown 標注來源路徑。
6. **雙軌查證（NIAN-A 配套）**：涉阿含／Nikāya 段落，漢譯端（vault-taisho）與 Pāli 端（vault-pali）須同時查證，兩邊都要實際打開確認。

> **原則**：vault 直連不是省事，是換一種負責。Code Opus 不再幫你把關，Chat Opus 自己就是把關者。

### 3.8 雙語同步 Bilingual Dual-Track

每篇論文同時產出中文版與英文版：

- **中文版**為主體，英文版為學術改寫（非逐字直譯）
- 兩版共享相同的章節結構、腳注編號、經論索引
- 英文版的經文引用保留中文原文，附英文翻譯
- NIAN 字數預算：中文 6,000–8,000 字/篇，英文 4,500–6,000 字/篇

**v2.2-NIAN 備註**：Pāli vault 已直接可查，NIAN-A 雙軌引用的 Pāli 端操作成本降至與漢譯端相當。

### 3.9 系列論文交叉引用 Cross-Reference Index

從 P04 起，每篇論文末尾附「系列論文交叉引用索引」。

### 3.10 經論索引表 Sūtra & Śāstra Index Table

每篇論文末尾必須附經論索引表。索引表必須與腳注實際引用完全吻合。

### 3.11 白話生活譬喻 Vernacular Analogy Pattern

每篇核心論證段落應包含至少一個生活化的白話譬喻。

### 3.12 寫作語氣規範 Tone & Voice Rules

**一核心一論文：** 每篇只論證一個核心命題。
**中文版禁用隨意英文。**
**尊重讀者，不貼標籤。**

---

## 四、分析框架標記 Analytical Conventions

| 標記 | 意義 | 使用場景 |
|------|------|---------|
| ✅ 義理可立 | 經論依據充分，邏輯自洽 | 核心命題判定 |
| ⚠️ 假說比喻 | 啟發性類比，非理論等價 | 跨學科會通 |
| ❌ 邏輯有誤 | 常見誤解，需要校正 | 中道校正 |

---

## 五、文件命名與目錄結構 File Naming & Directory Structure

```
papers/
├── part1_foundations_pali/
│   ├── P01_what-anussati-actually-means.md         ← 英文版
│   ├── P01_隨念字義與作意結構.md                    ← 中文版
│   └── ...
├── part2_earliest_chinese/
│   └── ...
└── ...
```

命名格式：`P{編號}_{主題關鍵詞}.md`。中文主題不超過 20 字，英文用 kebab-case。

---

## 六、版本與更新 Versioning

論文以 `Version: X.Y` 標記。
- 小修（typo、格式調整）：Y +1
- 重大修訂（義理修正、新增論證）：X +1

---

## 版本紀錄

- v1.0 (2026-03)：初版 style guide（dharma-research）
- v2.0 (2026-04)：擴展至完整方法論指南
- v2.1 (2026-04)：新增 S8 專屬條款（dharma-research only）
- v2.1-NIAN (2026-04)：新增 NIAN 專屬條款 A–E，調整 hook/framing 為西方讀者
- **v2.2-NIAN (2026-04)：RAG 流程改寫——Chat Opus 直接存取 vault（Filesystem MCP），§3.7 全面改寫，新增 §3.7.1 Vault 存取紀律，新增「NIAN 雙 Claude 分工」章節，NIAN-A/D/E 附 v2.2 操作補充**

---

*本指南本身也遵循「諸法實相」的精神：開放、透明、歡迎質疑。*
