# Resume RAG Platform - Deployment

這是 Resume RAG 系統的部署中心。

## 快速啟動
1. 複製環境變數設定：`cp .env.example .env` 並填入金鑰。
2. 確保其他三個 Repo (`frontend`, `backend`, `worker`) 已在同級目錄。
3. 啟動服務：`docker-compose up -d --build`

## 服務架構
- **Nginx**: Port 80 (入口)
- **PostgreSQL**: Vector DB
- **Redis**: Message Broker for Celery