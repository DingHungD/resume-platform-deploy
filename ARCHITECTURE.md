# 🚀 Resume RAG AI Platform - 全端系統架構說明書

## 1. 專案願景
本專案旨在建立一個具備使用者隔離、高併發處理能力與語意搜尋功能的履歷分析系統。透過 RAG 技術解決 LLM 幻覺問題，並實現非同步的檔案處理流程。

## 2. 系統架構圖 (System Topology)



## 3. 模組職責 (Repositories)

| 儲存庫名稱 | 技術棧 | 核心職責 |
| :--- | :--- | :--- |
| **resume-platform-deploy** | Docker Compose, Nginx | 環境變數管理、網路拓樸、服務編排。 |
| **resume-platform-frontend** | Next.js, Tailwind CSS | 聊天介面、管理者後台、權限 UI。 |
| **resume-platform-backend** | FastAPI, SQLAlchemy | API 路由、JWT 驗證、RAG 檢索邏輯、意圖過濾。 |
| **resume-platform-worker** | Celery, Redis, PyMuPDF | 檔案解析、Embedding 計算、Metadata 提取。 |

## 4. 資料模型與流轉 (Data Flow)

### 4.1 資料庫設計核心 (PostgreSQL + PGVector)
- **向量儲存**: 使用 `pgvector` 擴充，維度預計為 1536 (OpenAI `text-embedding-3-small`)。
- **權限隔離**: 所有 `Resumes` 與 `Chat_History` 均綁定 `user_id`，確保多租戶資料安全。

### 4.2 非同步處理流程 (Async Pipeline)
1. 用戶上傳 -> **Backend** (存檔) -> 發送 Task ID 到 **Redis**。
2. **Worker** 監聽 Redis -> 解析內容 -> **LLM 提取元數據** -> 存入 `Resume_Metadata` 與 `Document_Chunks`。
3. 狀態更新為 `completed`，前端透過輪詢或 WebSocket 取得進度。

## 5. 核心邏輯規約 (Logic Rules)

### 5.1 意圖過濾器 (Intent Guardrail)
在 RAG 檢索前，系統會過濾「非專業諮詢問句」。
- **邏輯**: 使用 LLM 或正規表達式判斷問句是否與「個人背景、技能、經歷」相關。
- **目的**: 節省 Token 消耗，防止 API 濫用。

### 5.2 RAG 策略
- **檢索**: 先結構化過濾 (Metadata)，再進行語意搜尋 (Vector Search)。
- **回答**: 結合檢索到的 Chunk 與 System Prompt 產出回答。

## 6. 環境相依性 (Dependencies)
- **核心 API**: OpenAI API (LLM & Embedding).
- **資料庫**: PostgreSQL 16+ w/ PGVector.
- **快取**: Redis 7.0+.

## 7. 如何重啟專案 (Re-start Guide)
1. 進入 `resume-platform-deploy`。
2. 配置 `.env` (參考 `.env.example`)。
3. 執行 `docker-compose up -d`。