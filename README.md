# 資管專題規格書生成 Skill (fju-im-spec-writer)

🇬🇧 [English](README.en.md) | 🇹🇼 繁體中文

給 Claude 用的 Agent Skill，依照輔仁大學資訊管理學系《系統分析與設計》課程老師的規範，協助生成/修改畢業專題規格書(SA文件)。

> **重要聲明**:這個工具是輔助排版與內容組織，**不會捏造專題內容**(如問卷數據、訪談結果、系統邏輯)，所有實質內容(需求分析、User Story細節、資料庫設計等)仍需自己提供或確認，skill只負責依照老師規範把內容組織成正確的格式，並在資訊不足時明確標記「待確認」。
>
> **關於`examples/`資料夾**:本地版skill包含透過指導教授取得的歷屆優秀範例(真實學生的畢業專題)，用於讓Claude對照真實案例的格式與寫作手法，這份檔案**不屬於本public repo的一部分**(見`.gitignore`)，因為內容版權/隱私屬於原作團隊，若clone這個repo,`examples/`資料夾會是空的,不影響skill其他功能。

## 這個skill做什麼

- 依老師規範的章節結構(第一~四章、附錄)生成內容
- 套用 User Story、資料庫設計、介面藍圖等固定格式
- 生成後自動對照品質檢查清單,抓出常見的邏輯不一致問題(如問題陳述與系統範圍對不上)
- 支援單人或雙人協作,用共用的進度表追蹤章節狀態

## 目錄結構

```
fju-im-spec-writer/
├── SKILL.md                      # 核心邏輯(觸發時機、流程、規則)
├── README.md / README.en.md      # 說明文件
├── LICENSE
├── CHANGELOG.md
├── templates/
│   ├── 111_SA.docx           # 111-113學年度SA範本
│   └── 114_SA.docx           # 114學年度後SA範本
├── reference/
│   ├── chapter_requirements.md   # 各章節規則、常見扣分點
│   ├── style_guide.md            # 各種表格/故事卡的精確格式
│   ├── database_design_rules.md  # 資料庫設計正規化規範
│   ├── format_pattern_guide.md   # 段落該用文字/表格/條列的對照表
│   ├── document_formatting.md    # 字型/字級/頁碼/目錄等文件層級格式(從範本XML解析)
│   ├── content_synthesis_guide.md # 逐段落來源優先策略與老師隱性偏好分析
│   └── review_checklist.md       # 生成後品質自我檢查清單
├── examples/
│   └── (可放匿名化的範例輸出;若有透過指導教授取得的歷屆優秀範例,建議如111/114範本一樣排除於public repo外,見.gitignore)
└── state/
    ├── project_context.md        # 專題背景知識庫(一次訪談,長期沿用)
    └── project_state.md          # 章節進度追蹤表(協作用)
```

## 安裝方式(Claude.ai)

1. Settings → Capabilities,確認「Code execution and file creation」已開啟
2. Customize → Skills → 點選「+」→「+ Create skill」
3. 把整個 `fju-im-spec-writer/` 資料夾壓縮成 ZIP(資料夾本身是ZIP的根目錄,不要多包一層)上傳
4. 上傳後在Skills清單中開啟這個skill

免費、Pro、Max方案皆可使用，個人上傳的custom skill僅自己帳號可見;若是Team/Enterprise方案,可在Skills設定中分享給組員。

## 適用範圍

這個skill的規則是依照**特定老師、特定課程**的規範萃取，若你不是輔大資管系該課程的學生，規則不適用，但你可以參考這個skill的架構方式(SKILL.md + reference + templates + state)，自行替換成你自己老師/課程的規範。

## 版本

見 CHANGELOG.md

## 致謝與參考

這個skill的部分設計思路參考自以下兩個開源專案(皆為MIT授權),特此致謝:

- **[obra/superpowers](https://github.com/obra/superpowers)**(MIT License)——啟發了「動手寫之前先透過提問把資訊釐清」(brainstorming)與「小單位反覆生成→驗證,不累積錯誤」(TDD式紅燈綠燈循環)的工作流程設計
- **[mattpocock/skills](https://github.com/mattpocock/skills)**(MIT License)——啟發了「一次性深度訪談、建立長期沿用的背景知識庫」(`grill-with-docs`)的設計,對應本skill的`state/project_context.md`機制

本skill沒有直接複製這兩個專案的程式碼或檔案內容,僅參考其設計理念並重新實作於完全不同的應用領域(學術規格書生成 vs 軟體工程開發流程)。

## 授權

本skill(SKILL.md、reference/、README等原創內容)採MIT License,見 LICENSE。範本文件(templates/)與歷屆範例(examples/)版權屬於原課程與範例文件作者,不隨此授權開放,詳見 LICENSE 底部說明與 `.gitignore`。
