# AI-Powered Resume RAG Platform (Deployment)

## 📌 項目概述
本項目是一個基於 **RAG (Retrieval-Augmented Generation)** 技術的智能履歷分析平台。它結合了向量資料庫與大語言模型，旨在協助開發者或人資快速從大量 PDF 履歷中檢索精確資訊。

- **核心語言**: Python (FastAPI), JavaScript (React), SQL.
- **它的作用**: 自動解析 PDF 履歷、向量化內容、並透過對話介面進行精準人才篩選。
- **為什麼有用**: 解決傳統關鍵字搜索無法理解語意（例如「具備雲端架構經驗」）的問題，提供語意層級的匹配。

---

## 🛠️ 安裝要求
在開始之前，請確保您的開發環境已安裝：
- **Docker**: 24.0.0+ 
- **Docker Compose**: V2 (不再需要 `version` 聲明)
- **Git**: 用於管理多倉庫結構

---

## 📖 安裝與配置文檔
本專案採用多倉庫架構，請確保目錄結構如下：
```text
/resume-platform
  ├── resume-platform-deploy/   (本倉庫：編排與配置)
  ├── resume-platform-backend/  (後端 API 與 AI 邏輯)
  ├── resume-platform-frontend/ (前端 UI 介面)
  └── resume-platform-worker/   (異步解析與 Embedding 處理)
```
- [後端詳細配置連結](https://github.com/DingHungD/resume-platform-backend/blob/main/requirements.txt)

- [前端詳細配置連結]()

## 🚀 使用概述 (快速啟動)

1. **環境變數設定**:
   在 `resume-platform-deploy` 根目錄創建 `.env`，並參考 `.env.example` 填入資料庫與 API Key。

2. **啟動全系統**:
   ```bash
   docker-compose up -d --build
   ```

3. **初始化資料庫表格 (Alembic):**
   由於後端代碼掛載於容器內，請執行以下指令完成遷移：
    ```bash
   docker-compose exec backend alembic upgrade head
   ```
4. **訪問路徑:**
- Frontend: http://localhost:3000
- API Docs (Swagger): http://localhost:8000/docs

## 🔍 故障排除與測試提示
- 資料庫擴充報錯: 若遇到 vector type does not exist，請確認 init-db/init.sql 已正確加載。

- 容器啟動失敗: 執行 docker-compose logs -f [service_name] 查看具體報錯。

- 網路通訊: 所有服務均在 resume_network 內，內部通訊請使用服務名稱（如 db, redis）。

- 詳細的技術架構與設計決策請參考 [ARCHITECTURE.md。](https://github.com/DingHungD/resume-platform-deploy/blob/main/ARCHITECTURE.md)

## 🔗 更多資源
- [FastAPI 官方文檔](https://fastapi.tiangolo.com/)
- [pgvector 使用指南](https://github.com/pgvector/pgvector)
- [OpenAI API 參考](https://developers.openai.com/api/docs)
