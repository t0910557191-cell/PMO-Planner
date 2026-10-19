# PMO Planner

個人用的工作台：幫你**記下來、排優先順序、到期提醒、追蹤別人的回覆、看指標燈號、累積成果**。純前端，不用安裝，放上 GitHub Pages 就能用。

這個工具不是拿來分析公司機密的。所有內容只存在你自己的瀏覽器，適合記錄自己的待辦、進度和能放在這裡的數字。

## 功能

**每天用**
- **今天**：一個輸入框快速記下任務、想法、等待回覆或想問主管的事；自動挑出「今天先做這三件」；到期、該追的、例行事項、指標預警都集中在這一頁。
- **任務**：用「重要／緊急」四象限檢視，或依優先分數排序。分數 = 重要性 × 3 ＋ 緊急程度 × 2 ＋ 力氣調整（小 +1、大 −1）。
- **等待回覆**：跟誰要了什麼、哪天該追，到期提醒；一鍵「收到了」或「+3 天」。
- **例行事項**：每週、每月、每季固定要做的事，按「完成這次」自動算下一次日期。

**看全局**
- **總覽**：每個負責單位的待辦、等回覆、專案數與指標燈號。
- **單位頁**：一個單位的所有東西放在一起，並能產生**週報草稿**（本週完成、專案進度、指標、等待中的資料、想問主管的事）。
- **指標**：領先與落後指標、BSC 四構面、目標與燈號門檻、趨勢小圖。把領先指標連到它影響的落後指標，轉黃或轉紅時會預警。
- **專案**：里程碑、風險、KPI、會議紀錄。

**記錄與成長**
- **問題清單**：想問主管的事先記，整理好再一起問。
- **成果紀錄**：做了什麼、帶來什麼結果、用到哪些能力（流程數位化、KPI／OKR、數據分析、成本／管理會計、生成式 AI 應用等），可以加一句英文，匯出 Markdown 用在考核或履歷。
- **慢半拍儀式**：每天三個溝通練習、睡前三行、週回顧、力道表。

**工具**
- **AI 諮詢**：支援 Claude，以及任何 OpenAI 相容格式的服務（OpenAI、Azure OpenAI、Google Gemini、公司內部閘道、本機 Ollama）。可選擇附上某個單位的摘要。
- **Google 日曆**：里程碑、任務截止日、追回覆的日子同步成整天事件；也可匯出 .ics。
- **瀏覽器提醒**：頁面開著時，到期的事會跳通知。
- **API 連接**：存常用端點、一鍵呼叫、把備份推送到自己的 webhook。
- **備份與匯出**：JSON 完整備份與還原、CSV（任務、等待回覆、指標、成果、專案、里程碑、風險、KPI）、.ics。

**單位隨時增減**：單位可以新增、改名，也可以「停用」（資料保留、從選單隱藏），之後再恢復。

---

## ⚠️ 資料安全

1. **程式碼可以放 GitHub，你的資料不要。** 你在工具裡輸入的內容只存在瀏覽器裡，不會被 commit。不要把匯出的檔案（`pmo-backup-*.json`、`pmo-*.csv`、`*.ics`、`成果紀錄-*.md`）放進 repo。
2. **不要輸入公司機密。** 營收、成本、人事資料等不要記在這裡。指標請記你自己的工作進度、件數或百分比。
3. **AI 諮詢會把你附上的資料送到你選的服務。** 公司有核准的 AI 服務時，優先用公司的。不確定時選「不附資料」，只問方法和寫法。
4. **金鑰只存在這台電腦的瀏覽器**，不會進備份檔。換電腦要重新輸入。
5. **定期備份。** 清除瀏覽資料、換電腦或用無痕模式都會讓資料不見。超過 7 天沒備份，右上角會提醒你。

---

## 安裝

### GitHub Pages

1. 在 GitHub 建一個新的 repository，例如 `strategy-pmo`。
2. 上傳 `index.html`、`README.md`、`LICENSE`、`.gitignore`、`.nojekyll` 這五個檔案。
3. **Settings → Pages**，Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾 `/ (root)`，按 Save。
4. 一兩分鐘後會出現網址：`https://你的帳號.github.io/strategy-pmo/`。

免費帳號的 Pages 需要 repo 是 public。別人打開網址只會看到範例資料，看不到你的內容。

### 在自己電腦上

直接雙擊 `index.html` 就能用，只有 Google 日曆登入需要用網址開啟：

```bash
python3 -m http.server 8080
# 然後打開 http://localhost:8080
```

---

## Google 日曆設定（一次，約 10 分鐘）

1. 到 [Google Cloud Console](https://console.cloud.google.com/) 建立專案。
2. **APIs & Services → Library**，啟用 **Google Calendar API**。
3. **OAuth consent screen**：User type 選 External，填 App name 和 email，在 **Test users** 加入你自己的 Google 帳號。
4. **Credentials → Create Credentials → OAuth client ID**，類型選 **Web application**，在 **Authorized JavaScript origins** 加入：
   - GitHub Pages：`https://你的帳號.github.io`
   - 本機：`http://localhost:8080`
5. 複製 Client ID，貼到工具的「設定」。
6. 建議在 Google 日曆另外建一個「PMO Planner」日曆，把它的日曆 ID 貼到設定。
7. 到「Google 日曆」頁按「連接 Google」，再按同步。

同步規則：再同步一次會更新既有事件；在日曆上刪掉的事件下次會重建；在工具裡刪掉的項目不會自動刪日曆事件。授權範圍是 `calendar.events`，約一小時過期，到時再按一次連接即可。

---

## AI 服務設定

到「設定 → AI 服務」選擇類型：

| 服務 | 端點網址 | 驗證方式 |
|---|---|---|
| Claude | （內建） | Claude API key |
| OpenAI | `https://api.openai.com/v1/chat/completions` | Bearer |
| Google Gemini | `https://generativelanguage.googleapis.com/v1beta/openai/chat/completions` | Bearer |
| Azure OpenAI | `https://<資源>.openai.azure.com/openai/deployments/<部署>/chat/completions?api-version=…` | api-key |
| Ollama（本機） | `http://localhost:11434/v1/chat/completions` | 不需要 |

設定頁有「快速套用」按鈕。從瀏覽器直接呼叫時，服務必須允許跨網域（CORS）；公司的 Azure 或內部閘道請先問 IT 是否開放。

---

## 檔案

```
index.html     整個工作台（樣式和程式都在這一個檔案裡）
README.md      說明
LICENSE        授權
.gitignore     擋掉匯出的資料檔
.nojekyll      讓 GitHub Pages 照原樣發佈
```

## 授權

MIT
