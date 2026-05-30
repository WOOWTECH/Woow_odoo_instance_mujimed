# CindyTech — Paperclip AI ESGTimes 18-Agent 編輯團隊操作手冊

> **平台**：Paperclip CMP + OpenClaw Gateway + Odoo 18
> **適用對象**：ESGTimes 內容操作員、品質審查員、營運管理員
> **最後更新**：2026-05-29
> **維護團隊**：WOOW Tech 沃科技

---

## 目錄

| 編號 | 章節名稱 | 說明 |
|------|---------|------|
| 1 | [系統總覽與平台說明](#1-系統總覽與平台說明) | 網址、帳號、18 Agent 團隊總覽、系統架構圖、內容生產線流程 |
| 2 | [角色快速入門指南](#2-角色快速入門指南) | 三種角色的操作導覽與任務對照表 |
| 3 | [基礎架構](#3-基礎架構) | K8s namespace、Pod/Service 列表、Gateway 配置、資料庫 |
| 4 | [登入與儀表板](#4-登入與儀表板) | Paperclip UI 登入流程、首頁導覽、Agent 側邊欄 |
| 5 | [Agent 管理](#5-agent-管理) | 建立、編輯、刪除 Agent，Adapter 設定，Instructions 維護 |
| 6 | [Issue 管理](#6-issue-管理) | 建立 Issue、指派 Agent、追蹤狀態、Comment 管理 |
| 7 | [Routines 排程與 Goals 目標](#7-routines-排程與-goals-目標) | 定時排程設定、月度目標追蹤、自動化觸發 |
| 8 | [管理層 — M1, M2, M3](#8-管理層--m1-m2-m3) | 總編輯長、內容主編、營運主編的操作指南 |
| 9 | [研究層 — R1 ~ R5](#9-研究層--r1--r5) | 五位研究員的使用方式與素材包格式 |
| 10 | [寫作層 — W1 ~ W7](#10-寫作層--w1--w7) | 七位寫手的風格差異、SEO 規則、產出管理 |
| 11 | [品質審查層 — Q1, Q2](#11-品質審查層--q1-q2) | 評分機制、退回流程、審核通過標準 |
| 12 | [同步層 — S1 Odoo 同步](#12-同步層--s1-odoo-同步) | Odoo 同步助手操作、手動/自動觸發、防重複機制 |
| 13 | [端到端工作流程教學](#13-端到端工作流程教學) | 從素材搜集到 Odoo 歸檔的完整流程演練 |
| 14 | [系統管理](#14-系統管理) | Pod 維護、Gateway 重啟、資料庫備份、故障排除 |
| 15 | [常見問題排除（FAQ）](#15-常見問題排除faq) | 常見錯誤訊息、解決方案、聯絡支援 |

---

## 1. 系統總覽與平台說明

本章介紹 ESGTimes 18-Agent 編輯團隊的整體系統環境，包括各平台網址、預設帳號、Agent 團隊架構、系統架構圖以及內容生產線流程。

---

### 步驟 1：系統網址一覽

| 平台 | 網址 | 用途 |
|------|------|------|
| Paperclip UI | `https://cindytech1-paperclip.woowtech.io` | Agent 編排與管理（CMP 主控台） |
| Odoo 18 | `https://cindytech1-odoo.woowtech.io` | 企業 ERP，文章歸檔與任務管理 |
| Nerve UI | `https://cindytech1-nerve.woowtech.io` | OpenClaw Gateway 監控介面 |
| Hermes WebUI | `https://cindytech1-hermes.woowtech.io` | Hermes Agent 對話介面 |

> **注意**：所有網址皆透過 Cloudflare Tunnel 提供 HTTPS 加密存取，無需 VPN。Paperclip UI 是日常操作的主要介面，Odoo 用於查閱同步後的文章歸檔記錄。

---

### 步驟 2：預設帳號

| 平台 | 帳號 | 密碼 | 備註 |
|------|------|------|------|
| Paperclip UI | `admin@cindytech.com` | `admin` | 管理者帳號，擁有所有 Agent 存取權限 |
| Odoo 18 | `admin` | `admin` | 管理者帳號，DB 名稱：`cindytech` |

> **安全提醒**：正式上線前請務必修改預設密碼。Paperclip 帳號可在「Settings → Users」中變更，Odoo 帳號可在「Settings → Users & Companies」中變更。

---

### 步驟 3：18 Agent 團隊總覽

ESGTimes 編輯團隊共 18 個 AI Agents，分為五個功能層：

#### 管理層（3 個）

| 代號 | 名稱 | 圖示 | 角色說明 |
|------|------|------|---------|
| M1 | 總編輯長 | :crown: | 制定內容策略、審核重大議題方向、維護品牌一致性 |
| M2 | 內容主編 | :clipboard: | 日常排程管理、指派任務給寫手、初步品質把關 |
| M3 | 營運主編 | :bar_chart: | 電子報排程、社群排程、Odoo 同步觸發、數據報告彙整 |

#### 研究層（5 個）

| 代號 | 名稱 | 圖示 | 監控領域 |
|------|------|------|---------|
| R1 | 政策法規研究員 | :scroll: | ISSB / GRI / TCFD / CSRD / SEC + 金管會 / 環境部 |
| R2 | 企業案例研究員 | :office: | GreenBiz / Bloomberg / CDP + 台灣企業 ESG 動態 |
| R3 | 數據趨勢研究員 | :chart_with_upwards_trend: | IPCC / IEA / PwC / McKinsey + 台灣在地數據 |
| R4 | 國際媒體掃描員 | :globe_with_meridians: | Carbon Brief / ESG Today / Eco-Business |
| R5 | 台灣在地線人 | :taiwan: | 金管會 / 環境部 / 證交所 / 天下 / 遠見 / 經濟日報 |

#### 寫作層（7 個）

| 代號 | 名稱 | 圖示 | 寫作風格 | 月產量目標 |
|------|------|------|---------|-----------|
| W1 | 新聞記者風 | :newspaper: | 倒三角結構、中立客觀 | 30 篇/月 |
| W2 | 知識型教師風 | :teacher: | 親切教學、比喻類比、步驟式引導 | 20 篇/月 |
| W3 | 學術分析師風 | :mortar_board: | 深度論據、多元角度、學術引用 | 15 篇/月 |
| W4 | 說故事風 | :book: | 場景敘事、情感連結、人物刻畫 | 15 篇/月 |
| W5 | 意見領袖風 | :loudspeaker: | 明確觀點、犀利評論、行業洞察 | 10 篇/月 |
| W6 | 數據新聞風 | :bar_chart: | 數據驅動、圖表建議、視覺化敘事 | 5 篇/月 |
| W7 | 簡潔速讀風 | :zap: | 3 分鐘可讀、條列精華、重點摘要 | 5 篇/月 |

> **月產量合計**：W1(30) + W2(20) + W3(15) + W4(15) + W5(10) + W6(5) + W7(5) = **100 篇/月**

#### 品質審查層（2 個）

| 代號 | 名稱 | 圖示 | 職責 |
|------|------|------|------|
| Q1 | 內容品質審查員 | :mag: | 事實正確性、數據來源查核、繁中品質、結構完整性（1-10 分） |
| Q2 | ESG 專業顧問 | :briefcase: | 術語使用、法規引用正確性、國際標準合規、觀點平衡性（1-10 分） |

#### 同步層（1 個）

| 代號 | 名稱 | 圖示 | 職責 |
|------|------|------|------|
| S1 | Odoo 同步助手 | :arrows_counterclockwise: | 監控 done 狀態的 Issues，自動同步到 Odoo 18 專案任務 |

---

### 步驟 4：系統架構圖

ESGTimes 18-Agent 編輯團隊的系統架構如下：

```
┌─────────────────────────────────────────────────────────────┐
│                    Cloudflare Tunnel                         │
│  cindytech1-paperclip.woowtech.io → paperclip-svc:3100     │
│  cindytech1-odoo.woowtech.io      → odoo-svc:8069          │
│  cindytech1-nerve.woowtech.io     → nerve-svc:3080         │
│  cindytech1-hermes.woowtech.io    → hermes-webui-svc:8787  │
└──────────────────────────┬──────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│              K8s Namespace: openclaw-tenant-1                │
│                                                              │
│  ┌──────────────┐    ws://openclaw-gateway-svc:18789         │
│  │  Paperclip   │──────────────►┌────────────────────┐       │
│  │  CMP UI      │               │  OpenClaw Gateway  │       │
│  │  (port 3100) │◄──────────────│  + Nerve UI        │       │
│  └──────┬───────┘               │  (port 18789/3080) │       │
│         │                       └─────────┬──────────┘       │
│         │                                 │                  │
│         │ SQL                    MiniMax M2.7 API             │
│         ▼                                 │                  │
│  ┌──────────────┐               ┌─────────▼──────────┐       │
│  │ paperclip-db │               │   MiniMax Cloud    │       │
│  │ PostgreSQL   │               │   (外部 LLM API)   │       │
│  │ (port 5432)  │               └────────────────────┘       │
│  └──────────────┘                                            │
│                                                              │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    │
│  │  openclaw-db │    │ hermes-agent │    │ hermes-webui │    │
│  │  PostgreSQL  │    │ (port 8642)  │    │ (port 8787)  │    │
│  │  + pgvector  │    └──────────────┘    └──────────────┘    │
│  └──────────────┘                                            │
│                                                              │
│  ┌────────────────┐  ┌───────────────┐  ┌──────────────┐     │
│  │ hermes-        │  │ hermes-redis  │  │ cloudflared  │     │
│  │ postgresql     │  │ Redis 7       │  │ (tunnel)     │     │
│  │ PostgreSQL 15  │  │ (port 6379)   │  └──────────────┘     │
│  └────────────────┘  └───────────────┘                       │
└──────────────────────────────────────────────────────────────┘
```

核心資料流：

1. **Paperclip CMP** 透過 WebSocket 連線到 **OpenClaw Gateway**（`ws://openclaw-gateway-svc:18789`）
2. **OpenClaw Gateway** 將 Agent 的 System Prompt + 使用者任務發送給 **MiniMax M2.7** 模型
3. MiniMax 回傳生成結果，Gateway 將結果回寫到 Paperclip Issue Comment
4. **S1 Odoo 同步助手** 監控已完成的 Issues，透過 XML-RPC API 同步到 **Odoo 18**

> **重要**：所有服務運行在同一個 K8s namespace `openclaw-tenant-1` 中。OpenClaw Gateway Pod 包含兩個核心元件：openclaw-gateway（port 18789）和 Nerve UI（port 3080）。Agent 資料持久化於 PVC `/mnt/openclaw-agents`。

![系統架構總覽](images/ch01_01_architecture-overview.png)

---

### 步驟 5：內容生產線流程

ESGTimes 的內容生產線採用四階段流水線架構，月產量目標為 **100 篇文章**：

```
 研究層              寫作層              品質審查層           同步層
 Research            Writing             QA Review           Sync
 R1 ~ R5             W1 ~ W7             Q1, Q2              S1

 ┌─────────┐        ┌─────────┐        ┌─────────┐        ┌─────────┐
 │ R1 政策  │───┐    │ W1 新聞  │───┐    │ Q1 內容  │───┐    │ S1 Odoo │
 │ R2 企業  │───┤    │ W2 教學  │───┤    │    品質  │───┤    │    同步  │
 │ R3 數據  │───┼──► │ W3 學術  │───┼──► │    審查  │───┼──► │    助手  │
 │ R4 國際  │───┤    │ W4 故事  │───┤    │         │   │    │         │
 │ R5 在地  │───┘    │ W5 觀點  │───┤    │ Q2 ESG  │───┘    └─────────┘
 │          │        │ W6 數據  │───┤    │    專業  │           │
 │          │        │ W7 速讀  │───┘    │    顧問  │           │
 └─────────┘        └─────────┘        └─────────┘           ▼
                                                         ┌─────────┐
  素材蒐集             文章撰寫            品質評分          │ Odoo 18 │
  Issue 建立           Issue 完成          ≥ 7分 通過        │ 專案任務 │
                                          < 7分 退回        │ + Chatter│
                                                         └─────────┘
```

#### 流程說明

| 階段 | 負責 Agent | 輸入 | 輸出 | 說明 |
|------|-----------|------|------|------|
| 1. 素材研究 | R1 ~ R5 | 主題方向（由 M1/M2 指定） | 素材包（結構化研究資料） | 五位研究員平行蒐集不同領域的 ESG 資訊 |
| 2. 文章撰寫 | W1 ~ W7 | 素材包 + 主題指令 | 完整文章（符合 SEO 9 條規則） | 七位寫手以不同風格平行撰寫 |
| 3. 品質審查 | Q1, Q2 | 文章全文 | 評分表（1-10 分）+ 建議 | 雙重審查：內容品質 + ESG 專業度 |
| 4. Odoo 同步 | S1 | Done 狀態的 Issue | Odoo Task + Chatter | 自動同步已完成且通過審查的文章 |

> **退回機制**：Q1 或 Q2 評分低於 7 分的文章，Issue 狀態將被改為 `in_progress` 並在 Comment 中標記「退回修改」+ 改善建議，原寫手需重新修改後再次提交審查。

> **月度目標**：100 篇/月（W1:30 + W2:20 + W3:15 + W4:15 + W5:10 + W6:5 + W7:5）

![內容生產線流程圖](images/ch01_02_content-pipeline.png)

---

### 步驟 6：公司資訊

| 項目 | 值 |
|------|-----|
| 公司名稱 | 撰寫文章自動化 |
| Company ID | `260d546d-46a2-463e-bcc0-23715a5bebeb` |
| 平台 | Paperclip CMP（Content Management Platform） |
| Agent 總數 | 18 |
| AI 模型 | MiniMax M2.7（透過 OpenClaw Gateway） |
| Odoo 專案名稱 | Paperclip AI - ESGTimes |
| 目標產出 | 100 篇 ESG 文章/月 |

> **注意**：Company ID 是 Paperclip 系統中用於識別公司的唯一 UUID。所有 API 呼叫和資料庫操作都以此 ID 作為公司篩選條件。如需透過 API 操作，請確保在請求中包含此 Company ID。

---

## 2. 角色快速入門指南

本章依據三種使用者角色，提供快速入門導覽，幫助您快速找到相關操作章節。

---

### 步驟 1：角色定義

#### 角色 A：內容操作員

| 項目 | 說明 |
|------|------|
| 主要職責 | 建立 Issue、指派給 Research/Writing Agents、閱讀產出結果 |
| 日常工作 | 依照編輯日曆建立素材蒐集和文章撰寫任務 |
| 核心操作 | 建立 Issue → 指派 Agent → 追蹤進度 → 閱讀產出 |
| 相關章節 | [第 6 章：Issue 管理](#6-issue-管理)、[第 9 章：研究層](#9-研究層--r1--r5)、[第 10 章：寫作層](#10-寫作層--w1--w7) |

#### 角色 B：品質審查員

| 項目 | 說明 |
|------|------|
| 主要職責 | 觸發 Q1/Q2 Agent 審查文章、閱讀評分結果、處理退回案件 |
| 日常工作 | 對已完成的文章執行品質審查，確保達到商用標準 |
| 核心操作 | 觸發審查 → 閱讀評分 → 通過/退回 → 追蹤修改 |
| 相關章節 | [第 6 章：Issue 管理](#6-issue-管理)、[第 11 章：品質審查層](#11-品質審查層--q1-q2) |

#### 角色 C：營運管理員

| 項目 | 說明 |
|------|------|
| 主要職責 | 監控 Odoo 同步狀態、執行同步腳本、檢查流水線健康度 |
| 日常工作 | 確保文章正確同步到 Odoo、處理同步失敗、監控系統指標 |
| 核心操作 | 檢查同步 → 手動/自動觸發 → 驗證 Odoo 記錄 → 系統監控 |
| 相關章節 | [第 12 章：同步層](#12-同步層--s1-odoo-同步)、[第 14 章：系統管理](#14-系統管理) |

---

### 步驟 2：你想做什麼？

使用以下對照表，依據您想完成的任務快速跳轉到對應章節：

| 我想做什麼？ | 操作說明 | 前往章節 |
|-------------|---------|---------|
| 登入 Paperclip 系統 | 使用 admin 帳號登入 Paperclip UI | [第 4 章](#4-登入與儀表板) |
| 建立一個新的寫作任務 | 建立 Issue 並指派給 W1~W7 寫手 | [第 6 章](#6-issue-管理) |
| 請研究員蒐集 ESG 素材 | 建立 Issue 並指派給 R1~R5 研究員 | [第 9 章](#9-研究層--r1--r5) |
| 讓寫手撰寫一篇新聞稿 | 建立 Issue 並指派給 W1 新聞記者風 | [第 10 章](#10-寫作層--w1--w7) |
| 審查一篇已完成的文章 | 觸發 Q1/Q2 Agent 對文章評分 | [第 11 章](#11-品質審查層--q1-q2) |
| 查看文章的品質分數 | 閱讀 Issue Comment 中的評分表 | [第 11 章](#11-品質審查層--q1-q2) |
| 將文章同步到 Odoo | 觸發 S1 Agent 的 Heartbeat | [第 12 章](#12-同步層--s1-odoo-同步) |
| 設定自動排程 | 建立 Routine 定時觸發 Agent | [第 7 章](#7-routines-排程與-goals-目標) |
| 修改 Agent 的 System Prompt | 進入 Agent 的 Instructions tab 編輯 | [第 5 章](#5-agent-管理) |
| 查看月度產出目標 | 檢查 Goals 面板的達成率 | [第 7 章](#7-routines-排程與-goals-目標) |
| 制定本週的編輯主題 | 建立 Issue 指派給 M1 總編輯長 | [第 8 章](#8-管理層--m1-m2-m3) |
| 檢查系統是否正常運作 | 查看 Pod 狀態與 Gateway 連線 | [第 14 章](#14-系統管理) |
| Agent 執行失敗怎麼辦 | 查閱故障排除指南 | [第 15 章](#15-常見問題排除faq) |
| 執行端到端流程測試 | 按照完整流程教學操作 | [第 13 章](#13-端到端工作流程教學) |
| 手動執行 Odoo 同步腳本 | 執行 `sync-paperclip-to-odoo.py` | [第 12 章](#12-同步層--s1-odoo-同步) |

> **提示**：如果您是第一次使用本系統，建議先閱讀[第 4 章](#4-登入與儀表板)了解登入流程，再依據您的角色前往對應章節。

---

## 3. 基礎架構

本章說明 ESGTimes 18-Agent 編輯團隊的 Kubernetes 基礎架構，包括 namespace、Pod/Service 列表、OpenClaw Gateway 配置以及資料庫結構。

---

### 步驟 1：K8s Namespace

所有服務部署在同一個 Kubernetes namespace 中：

| 項目 | 值 |
|------|-----|
| Namespace | `openclaw-tenant-1` |
| 標籤 | `app.kubernetes.io/part-of: openclaw-paas` |
| 租戶標籤 | `tenant: tenant-1` |

```bash
# 查看 namespace
kubectl get namespace openclaw-tenant-1

# 查看 namespace 中所有資源
kubectl get all -n openclaw-tenant-1
```

---

### 步驟 2：Pod 列表

| Pod 名稱 | 映像檔 | 用途 | 持久化儲存 |
|----------|--------|------|-----------|
| `paperclip` | paperclip:latest | Paperclip CMP 主應用程式 | — |
| `paperclip-db` | postgres:15 | Paperclip 專用 PostgreSQL（agents、issues 資料） | PVC |
| `openclaw-gateway` | openclaw-custom:latest | OpenClaw AI Gateway + Nerve UI（雙容器） | PVC: `openclaw-agents-pvc` (1Gi) |
| `openclaw-db` | pgvector/pgvector:pg16 | OpenClaw Gateway 專用 PostgreSQL + pgvector | PVC: `openclaw-db-pvc` (10Gi) |
| `cloudflared` | cloudflare/cloudflared:latest | Cloudflare Tunnel 代理 | — |
| `hermes-agent` | hermes-agent-custom:latest | Hermes Agent 服務（gateway 模式） | PVC: `hermes-home-pvc` |
| `hermes-postgresql` | postgres:15 | Hermes 專用 PostgreSQL | PVC: `hermes-postgresql-pvc` |
| `hermes-redis` | redis:7-alpine | Hermes 快取服務 | PVC: `hermes-redis-pvc` |
| `hermes-webui` | ghcr.io/nesquena/hermes-webui:latest | Hermes Web 介面 | emptyDir |
| `setup-wizard` | openclaw-setup-wizard:latest | OpenClaw 初始設定精靈 | — |

```bash
# 查看所有 Pod 狀態
kubectl get pods -n openclaw-tenant-1

# 查看 Pod 詳細資訊
kubectl describe pod <pod-name> -n openclaw-tenant-1
```

![K8s Pod 列表](images/ch03_01_k8s-pods.png)

---

### 步驟 3：Service 列表

| Service 名稱 | Port | 類型 | 對應 Pod | 說明 |
|-------------|------|------|---------|------|
| `paperclip-svc` | 3100 | ClusterIP | paperclip | Paperclip CMP API 與 Web UI |
| `paperclip-db-svc` | 5432 | ClusterIP | paperclip-db | Paperclip 資料庫連線 |
| `openclaw-gateway-svc` | 18789 | ClusterIP | openclaw-gateway | OpenClaw Gateway WebSocket API |
| `nerve-svc` | 3080 | ClusterIP | openclaw-gateway | Nerve UI 監控介面 |
| `openclaw-db-svc` | 5432 | ClusterIP | openclaw-db | OpenClaw 資料庫連線 |
| `hermes-agent-svc` | 8642, 9119 | ClusterIP | hermes-agent | Hermes Gateway (8642) + Dashboard (9119) |
| `hermes-postgresql-svc` | 5432 | ClusterIP | hermes-postgresql | Hermes 資料庫連線 |
| `hermes-redis-svc` | 6379 | ClusterIP | hermes-redis | Hermes Redis 快取 |
| `hermes-webui-svc` | 8787 | ClusterIP | hermes-webui | Hermes Web 介面 |
| `setup-wizard-svc` | 18790 | ClusterIP | setup-wizard | 初始設定精靈 |

```bash
# 查看所有 Service
kubectl get svc -n openclaw-tenant-1

# 測試 Service 連線（從 namespace 內部）
kubectl run test-curl --rm -it --image=curlimages/curl -n openclaw-tenant-1 -- \
  curl -s http://paperclip-svc:3100/health
```

![Service 列表](images/ch03_02_k8s-services.png)

---

### 步驟 4：OpenClaw Gateway 詳細配置

OpenClaw Gateway 是 18 個 Agents 執行 AI 任務的核心引擎。

#### 容器架構

`openclaw-gateway` Pod 包含以下元件：

| 元件 | Port | 說明 |
|------|------|------|
| openclaw-gateway | 18789 | AI Gateway 主程式，接收 WebSocket 連線並轉發到 LLM |
| Nerve UI | 3080 | Gateway 監控介面，顯示 Agent 狀態與對話紀錄 |

#### AI 模型

| 項目 | 值 |
|------|-----|
| 預設模型 | MiniMax M2.7 |
| 模型識別碼 | `minimax/MiniMax-M2.7` |
| 認證方式 | API Key（透過 K8s Secret `openclaw-secrets` 注入） |

#### 持久化儲存（PVC）

所有 Agent 資料持久化於 PVC，掛載路徑為 `/mnt/openclaw-agents`：

| 目錄 | 用途 |
|------|------|
| `/mnt/openclaw-agents/` | Agent 主目錄（symlink 到 `~/.openclaw/agents`） |
| `/mnt/openclaw-agents/_workspace/` | 預設工作空間（main agent 使用） |
| `/mnt/openclaw-agents/_workspace-{agentId}/` | 各 Agent 專屬工作空間（共 17 個，如 `_workspace-r1-policy`） |
| `/mnt/openclaw-agents/_managed-skills/` | 已安裝的技能 |
| `/mnt/openclaw-agents/_memory/` | Agent 記憶資料 |
| `/mnt/openclaw-agents/_cron/` | 排程任務設定 |
| `/mnt/openclaw-agents/_config/` | 應用程式設定 |
| `/mnt/openclaw-agents/_openclaw.json` | Gateway 主設定檔，包含 19 個 Agent 定義（main + test-poke + 17 ESGTimes agents） |

> **注意**：Pod 重啟後所有 PVC 資料保留，包括 Agent 設定、記憶、技能和排程。Gateway 主設定檔（`openclaw.json`）直接 symlink 到 PVC，透過 Web GUI 修改設定會立即持久化。

---

### 步驟 5：Adapter 配置與 1:1 Agent Mapping

每個 Paperclip Agent 都有專屬的 OpenClaw Agent，實現 **1:1 獨立 workspace 架構**。這確保每個 Agent 在 Gateway 中擁有獨立的對話空間、記憶和設定。

#### 連線參數

| 項目 | 值 |
|------|-----|
| Adapter 類型 | `openclaw_gateway` |
| WebSocket URL | `ws://openclaw-gateway-svc:18789` |
| Token | `cindytech` |
| Scopes | `operator.admin, operator.read, operator.write, operator.pairing` |
| Paperclip API URL | `http://paperclip-svc:3100`（Gateway 回寫結果用） |

#### 1:1 Agent Mapping（Paperclip ↔ OpenClaw）

每個 Paperclip Agent 的 `adapter_config` 中設有 `agentId`，對應到 OpenClaw Gateway 中的獨立 Agent：

| Paperclip Agent | OpenClaw agentId | Workspace |
|----------------|-----------------|-----------|
| 總編輯長 M1 | `m1-editor-chief` | `_workspace-m1-editor-chief` |
| 內容主編 M2 | `m2-content-editor` | `_workspace-m2-content-editor` |
| 營運主編 M3 | `m3-ops-editor` | `_workspace-m3-ops-editor` |
| 政策法規研究員 R1 | `r1-policy` | `_workspace-r1-policy` |
| 企業案例研究員 R2 | `r2-enterprise` | `_workspace-r2-enterprise` |
| 數據趨勢研究員 R3 | `r3-data` | `_workspace-r3-data` |
| 國際媒體掃描員 R4 | `r4-international` | `_workspace-r4-international` |
| 台灣在地線人 R5 | `r5-taiwan` | `_workspace-r5-taiwan` |
| 新聞記者風寫手 W1 | `w1-news` | `_workspace-w1-news` |
| 知識型教師風寫手 W2 | `w2-teacher` | `_workspace-w2-teacher` |
| 學術分析師風寫手 W3 | `w3-analyst` | `_workspace-w3-analyst` |
| 說故事風寫手 W4 | `w4-storyteller` | `_workspace-w4-storyteller` |
| 意見領袖風寫手 W5 | `w5-opinion` | `_workspace-w5-opinion` |
| 數據新聞風寫手 W6 | `w6-data-news` | `_workspace-w6-data-news` |
| 簡潔速讀風寫手 W7 | `w7-quickread` | `_workspace-w7-quickread` |
| 內容品質審查員 Q1 | `q1-quality` | `_workspace-q1-quality` |
| ESG 專業顧問 Q2 | `q2-esg-expert` | `_workspace-q2-esg-expert` |

> **重要**：在 OpenClaw Console（Nerve UI）的 Agents 頁面中，可以從下拉選單選擇任何一個 Agent 進行獨立對話。每個 Agent 保持各自的角色性格和設置。

#### Adapter Config JSON 範例

```json
{
  "url": "ws://openclaw-gateway-svc:18789",
  "role": "operator",
  "scopes": ["operator.admin", "operator.read", "operator.write", "operator.pairing"],
  "headers": {"x-openclaw-token": "cindytech"},
  "waitTimeoutMs": 120000,
  "sessionKeyStrategy": "issue",
  "devicePrivateKeyPem": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----",
  "autoPairOnFirstConnect": true,
  "paperclipApiUrl": "http://paperclip-svc:3100",
  "agentId": "r1-policy"
}
```

| 配置項目 | 說明 |
|---------|------|
| `agentId` | 對應到 OpenClaw Gateway 中的 Agent ID（1:1 mapping 的關鍵） |
| `paperclipApiUrl` | Paperclip API 的內部 K8s URL（必須設定，否則 Agent 會用 localhost 導致連線失敗） |
| `devicePrivateKeyPem` | Ed25519 private key（每個 Agent 獨立） |
| `autoPairOnFirstConnect` | 首次連線自動與 Gateway 配對 |
| `sessionKeyStrategy` | `issue` = 每個 Issue 一個獨立 session |

#### Ed25519 Device Key

每個 Agent 擁有獨立的 Ed25519 device private key，用於與 OpenClaw Gateway 建立安全配對：

```bash
# 產生 Agent 專用 Ed25519 key
openssl genpkey -algorithm Ed25519 -out agent_M1.pem
```

> **安全提示**：Ed25519 private key 儲存在 Paperclip DB 的 `agents` 表中（`adapter_config` 欄位）。請勿將 key 外洩或在不安全的環境中傳輸。

---

### 步驟 5a：在 OpenClaw Console 與個別 Agent 對話

除了透過 Paperclip Issue 觸發 Agent 外，也可以在 OpenClaw Console（Nerve UI）直接與每個 Agent 對話：

#### 操作步驟

1. 開啟 OpenClaw Console：`https://cindytech1-openclaw.woowtech.io`
2. 在 Password 欄位輸入 `cindytech`，點擊 **Connect**
3. 進入 Chat 頁面
4. 在頂部的 **Agent 下拉選單**中選擇要對話的 Agent（例如 `r1-policy`、`w4-storyteller`）
5. 在底部的對話框輸入訊息，按 Enter 送出
6. Agent 會以對應的角色性格回覆

![OpenClaw Agent 選單](images/ch05_07_adapter_config_fields.png)

#### 驗證範例

在 OpenClaw Console 中分別向 3 個 Agent 詢問「你是誰？」的回覆：

| Agent | 回覆摘要 |
|-------|---------|
| `w4-storyteller` | 「溫暖而尖銳，像個靠譜的同行者而不是一台打字機器」 |
| `r3-data` | 「數據趨勢研究員，挖掘 ESG 數據與趨勢情報」 |
| `m1-editor-chief` | 列出 3 個 ESG 重點主題（ISSB 準則、碳費審議會、AI×ESG）|

> **提示**：OpenClaw Console 適合用於快速測試 Agent 的回應品質、調試 System Prompt、或進行非正式的 ESG 內容諮詢。正式的文章生產流程請使用 Paperclip Issue。

---

### 步驟 6：Cloudflare Tunnel 路由

所有外部存取透過 Cloudflare Tunnel 提供安全 HTTPS 連線：

| 外部域名 | 內部目標 | 說明 |
|----------|---------|------|
| `cindytech1-paperclip.woowtech.io` | `paperclip-svc:3100` | Paperclip CMP 主介面 |
| `cindytech1-odoo.woowtech.io` | `odoo-svc:8069` | Odoo 18 ERP |
| `cindytech1-nerve.woowtech.io` | `nerve-svc:3080` | OpenClaw Nerve 監控介面 |
| `cindytech1-hermes.woowtech.io` | `hermes-webui-svc:8787` | Hermes Agent Web 介面 |

```bash
# 查看 Cloudflare Tunnel Pod 狀態
kubectl get pod -l app=cloudflared -n openclaw-tenant-1

# 查看 Tunnel 日誌
kubectl logs -l app=cloudflared -n openclaw-tenant-1 --tail=50
```

> **注意**：Cloudflare Tunnel 由 `cloudflared` Pod 提供，Tunnel Token 透過 K8s Secret `cf-secrets` 注入。路由設定在 Cloudflare Dashboard 的 Zero Trust → Access → Tunnels 中管理。

![Cloudflare Tunnel 路由圖](images/ch03_03_cloudflare-tunnel.png)

---

### 步驟 7：資料庫架構

系統使用三個獨立的 PostgreSQL 資料庫：

| 資料庫 | Pod | Service | 用途 | 版本 |
|--------|-----|---------|------|------|
| `paperclip` | paperclip-db | `paperclip-db-svc:5432` | 儲存 Agents、Issues、Comments、Routines、Goals | PostgreSQL 15 |
| `openclaw` | openclaw-db | `openclaw-db-svc:5432` | 儲存 Gateway 設定、對話紀錄、向量記憶 | PostgreSQL 16 + pgvector |
| `hermes` | hermes-postgresql | `hermes-postgresql-svc:5432` | 儲存 Hermes Agent 資料 | PostgreSQL 15 |

#### Paperclip DB 核心表格

| 表格名稱 | 說明 |
|----------|------|
| `agents` | Agent 基本資料（名稱、Instructions、adapter_config、Ed25519 key） |
| `issues` | Issue 任務（標題、描述、狀態、指派的 Agent） |
| `issue_comments` | Issue 的 Comment（Agent 回覆的文章內容、審查評分） |
| `routines` | 排程任務設定 |
| `goals` | 月度目標與進度追蹤 |

```bash
# 連線到 Paperclip DB
kubectl exec -it deploy/paperclip-db -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip

# 查詢所有 Agents
SELECT id, name, adapter_type FROM agents
WHERE company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb';
```

> **備份提醒**：建議每日對 `paperclip` 和 `openclaw` 資料庫執行 pg_dump 備份。詳細備份流程請參閱[第 14 章：系統管理](#14-系統管理)。

![資料庫架構圖](images/ch03_04_database-architecture.png)
# Paperclip AI × ESGTimes 操作手冊 — Part 2

## 平台操作：登入、Agent、Issue、排程

---

# 第四章：登入與儀表板

本章說明如何登入 Paperclip CMP 平台，以及儀表板的各項功能導覽。

---

### 步驟 1：開啟 Paperclip CMP

在瀏覽器中開啟以下網址：

| 項目 | 值 |
|------|-----|
| URL | `https://cindytech1-paperclip.woowtech.io` |
| 建議瀏覽器 | Chrome / Edge（最新版） |

![開啟 Paperclip CMP 登入頁面](images/ch04_01_open_cmp.png)

---

### 步驟 2：登入

使用以下帳號密碼登入系統：

| 項目 | 值 |
|------|-----|
| Email | `admin@cindytech.com` |
| Password | `admin` |

輸入完畢後點選 **Login** 按鈕。

![輸入帳號密碼登入](images/ch04_02_login.png)

> **注意**：首次登入後建議立即更改密碼，以確保系統安全。

---

### 步驟 3：儀表板導覽

登入成功後進入儀表板（Dashboard）。Paperclip CMP 採用深色主題（Dark Theme）介面，主要區域說明如下：

**頂部列（Top Bar）**

| 元素 | 說明 |
|------|------|
| 公司名稱 | 顯示目前所在公司，ESGTimes 對應的公司名稱為「撰寫文章自動化」 |
| 搜尋列 | 可搜尋 Issue、Agent 等資源 |

**左側邊欄（Sidebar）**

| 區段 | 項目 | 說明 |
|------|------|------|
| 頂部功能 | New Issue | 快速建立新 Issue |
| 頂部功能 | Dashboard | 回到儀表板首頁 |
| 頂部功能 | Inbox | 查看通知與訊息 |
| **WORK** | Issues | 所有任務列表 |
| **WORK** | Routines | 排程任務管理 |
| **WORK** | Goals | 目標追蹤 |
| **PROJECTS** | （依公司設定） | 專案分類 |
| **AGENTS** | 18 個 Agent 列表 | 列出所有已建立的 Agent，包含圖示 |
| **COMPANY** | Org | 組織設定 |
| **COMPANY** | Skills | 技能管理 |

![儀表板總覽 — 側邊欄與主要區域](images/ch04_03_dashboard_overview.png)

![儀表板 — AGENTS 區段展開，顯示 18 個 Agent](images/ch04_04_dashboard_agents.png)

---

### 步驟 4：Inbox 通知

點選側邊欄的 **Inbox** 進入通知中心。此處會顯示系統事件，包括：

- Agent Heartbeat 結果（成功 / 失敗）
- Agent 執行失敗的錯誤訊息
- Issue 狀態變更通知
- Agent 回覆的新留言

**Inbox 頁籤說明**

| 頁籤 | 說明 |
|------|------|
| Mine | 與你直接相關的通知 |
| Recent | 最近的所有通知 |
| Unread | 尚未讀取的通知 |
| All | 全部通知歷史紀錄 |

![Inbox 通知頁面，顯示 Heartbeat 結果與各頁籤](images/ch04_05_inbox.png)

---

### 步驟 5：切換公司

若帳號下有多個公司，可透過左上角的公司下拉選單進行切換。

| 項目 | 說明 |
|------|------|
| 下拉位置 | 左上角，點選目前公司名稱即可展開 |
| ESGTimes 公司名稱 | 「撰寫文章自動化」 |

![切換公司下拉選單](images/ch04_06_switch_company.png)

> **注意**：請確認目前所在公司為「撰寫文章自動化」，否則將看不到 ESGTimes 的 18 個 Agent。

---

# 第五章：Agent 管理

本章說明如何瀏覽、設定、執行及監控 Agent。ESGTimes 共有 18 個 Agent，皆透過 OpenClaw Gateway 連接 MiniMax M2.7 模型。

---

### 步驟 1：瀏覽 Agent 列表

在左側邊欄的 **AGENTS** 區段中，可以看到所有 18 個 Agent 的清單，每個 Agent 旁邊會顯示對應的圖示。

**完整 Agent 清單**

| 代號 | 名稱 | 層級 |
|------|------|------|
| M1 | 總編輯長 | 管理層 |
| M2 | 內容主編 | 管理層 |
| M3 | 營運主編 | 管理層 |
| R1 | 政策法規研究員 | Research 層 |
| R2 | 企業案例研究員 | Research 層 |
| R3 | 數據趨勢研究員 | Research 層 |
| R4 | 國際媒體掃描員 | Research 層 |
| R5 | 台灣在地線人 | Research 層 |
| W1 | 新聞記者風寫手 | Writing 層 |
| W2 | 知識型教師風寫手 | Writing 層 |
| W3 | 學術分析師風寫手 | Writing 層 |
| W4 | 說故事風寫手 | Writing 層 |
| W5 | 意見領袖風寫手 | Writing 層 |
| W6 | 數據新聞風寫手 | Writing 層 |
| W7 | 簡潔速讀風寫手 | Writing 層 |
| Q1 | 內容品質審查員 | 品質層 |
| Q2 | ESG 專業顧問 | 品質層 |
| S1 | Odoo 同步助手 | 支援層 |

![Agent 列表 — 側邊欄顯示所有 18 個 Agent](images/ch05_01_agent_list.png)

---

### 步驟 2：Agent 詳情頁

點選任一 Agent 即可進入其詳情頁面。頁面頂部顯示：

| 欄位 | 說明 |
|------|------|
| Name | Agent 名稱（如「政策法規研究員」） |
| Role | Agent 角色代號（如「R1」） |
| Title | Agent 職稱描述 |

頁面下方包含三個主要頁籤：

| 頁籤 | 說明 |
|------|------|
| Runs | 執行歷史紀錄（Heartbeat 結果） |
| Instructions | 系統提示詞（System Prompt） |
| Settings | Adapter 與連線設定 |

![Agent 詳情頁 — 頂部資訊](images/ch05_02_agent_detail_top.png)

![Agent 詳情頁 — 頁籤區域](images/ch05_03_agent_detail_tabs.png)

---

### 步驟 3：查看 Instructions（System Prompt）

點選 **Instructions** 頁籤，可查看該 Agent 的完整系統提示詞（System Prompt）。此提示詞定義了 Agent 的：

- 身份與角色定義
- 監控來源（Research Agent）或寫作風格（Writing Agent）
- 輸出格式規範
- 工作規則與禁止事項
- SEO 格式規則（Writing Agent 共用 9 條規則）

![Instructions 頁籤 — 顯示完整 System Prompt](images/ch05_04_instructions.png)

> **注意**：System Prompt 是 Agent 行為的核心。修改前請仔細閱讀並備份原始內容，錯誤的 Prompt 可能導致 Agent 產出品質下降。

---

### 步驟 4：編輯 Instructions

在 Instructions 頁籤中，可以直接在文字編輯區修改 System Prompt 的內容。修改完成後點選 **Save** 按鈕儲存。

常見修改情境：

| 情境 | 修改內容 |
|------|---------|
| 調整寫作風格 | 修改寫手 Agent 的風格描述 |
| 增加監控來源 | 在 Research Agent 的 Prompt 中新增資料來源 |
| 調整輸出格式 | 修改文章結構或字數要求 |
| 加入新規則 | 新增品質要求或禁止事項 |

![編輯 Instructions — 直接修改 System Prompt](images/ch05_05_edit_instructions.png)

---

### 步驟 5：查看 Adapter 配置

點選 **Settings** 頁籤，可查看 Agent 的 Adapter 連線配置。所有 ESGTimes Agent 皆使用 **OpenClaw Gateway** adapter。

**Adapter 配置欄位說明**

| 欄位 | 說明 | 範例值 |
|------|------|--------|
| `url` | OpenClaw Gateway WebSocket 連線位址 | `ws://openclaw-gateway-svc:18789` |
| `token` | 驗證 Token | `cindytech` |
| `scopes` | 權限範圍 | `operator.admin, operator.read, operator.write, operator.pairing` |
| `devicePrivateKeyPem` | Agent 專屬的 Ed25519 私鑰（PEM 格式） | 每個 Agent 獨立產生 |
| `paperclipApiUrl` | Paperclip API 回連位址 | Paperclip 內部 URL |
| `autoPairOnFirstConnect` | 首次連線時是否自動配對 | `true` |
| `sessionKeyStrategy` | Session Key 策略 | 依系統設定 |
| `waitTimeoutMs` | 等待逾時時間（毫秒） | 依系統設定 |

![Settings 頁籤 — Adapter 類型與基本設定](images/ch05_06_adapter_settings.png)

![Settings 頁籤 — 完整配置欄位](images/ch05_07_adapter_config_fields.png)

> **注意**：`devicePrivateKeyPem` 為每個 Agent 獨立的 Ed25519 金鑰，切勿複製同一把金鑰給不同 Agent，否則會導致連線衝突。

---

### 步驟 6：執行 Heartbeat

在 Agent 詳情頁中，點選 **Run Heartbeat** 按鈕可手動觸發 Agent 執行。Heartbeat 會讓 Agent「醒來」並檢查是否有待處理的工作。

| 項目 | 說明 |
|------|------|
| 按鈕位置 | Agent 詳情頁頂部 |
| 執行效果 | Agent 連線至 OpenClaw Gateway，檢查並處理待辦事項 |
| 等待時間 | 通常 30–120 秒內完成 |

![點選 Run Heartbeat 按鈕](images/ch05_08_run_heartbeat.png)

---

### 步驟 7：查看 Heartbeat 結果

切換至 **Runs** 頁籤，可查看所有 Heartbeat 執行的歷史紀錄。

**Runs 欄位說明**

| 欄位 | 說明 |
|------|------|
| Status | 執行結果狀態 |
| Duration | 執行耗時 |
| Error | 若失敗，顯示錯誤詳情 |
| Timestamp | 執行時間 |

**狀態分類**

| 狀態 | 圖示 | 說明 |
|------|------|------|
| `succeeded` | 綠色 | 執行成功完成 |
| `failed` | 紅色 | 執行過程中發生錯誤 |
| `timed_out` | 黃色 | 執行超時未完成 |

![Runs 頁籤 — 執行歷史列表](images/ch05_09_runs_history.png)

![Runs — succeeded 狀態詳情](images/ch05_10_run_succeeded.png)

![Runs — failed 狀態與錯誤訊息](images/ch05_11_run_failed.png)

---

### 步驟 8：Agent 狀態說明

每個 Agent 在系統中有以下四種運行狀態：

| 狀態 | 英文 | 說明 |
|------|------|------|
| 等待中 | `idle` | Agent 處於休眠狀態，等待新任務或下次 Heartbeat |
| 執行中 | `running` | Agent 正在處理任務，與 OpenClaw Gateway 通訊中 |
| 錯誤 | `error` | Agent 執行失敗，需檢查 Runs 頁籤中的錯誤訊息排除問題 |
| 已暫停 | `paused` | Agent 被手動暫停，不會回應 Heartbeat 或新任務 |

![Agent 狀態圖示與說明](images/ch05_12_agent_states.png)

> **注意**：所有 18 個 Agent 共用同一個 OpenClaw Gateway（MiniMax M2.7 模型）。同時觸發太多 Agent 可能導致 Gateway 過載。建議一次不超過 5 個 Agent 同時執行。

---

# 第六章：Issue 管理

本章說明 Issue 的概念、建立方式、Agent 指派、生命週期管理，以及如何查看 Agent 的回覆產出。

---

### 步驟 1：Issue 是什麼

在 Paperclip CMP 中，**Issue 是工作任務的基本單位**。每一個需要 Agent 處理的工作，都以 Issue 的形式建立並追蹤。

**Issue 的組成要素**

| 要素 | 說明 | 範例 |
|------|------|------|
| Identifier | 系統自動編號 | `CMP-42` |
| Title | 任務標題 | 「撰寫台灣碳權交易市場分析文章」 |
| Description | 任務描述與需求 | 包含主題、角度、參考資料等 |
| Status | 目前狀態 | `todo` / `in_progress` / `done` / `blocked` |
| Assignee | 負責的 Agent | 如「W1 新聞記者風寫手」 |
| Comments | 留言串 | Agent 的回覆、產出文章、審查評分等 |

Issue 是你與 Agent 溝通的主要管道：透過 Issue 下達工作指令，Agent 會在 Comments 中回覆結果。

---

### 步驟 2：建立新 Issue

1. 點選側邊欄頂部的 **New Issue** 按鈕
2. 在彈出的表單中填寫：
   - **Title**：任務標題（簡潔明確描述任務）
   - **Description**：任務描述（詳細說明需求、角度、參考資料等）

![點選 New Issue 按鈕](images/ch06_01_new_issue_button.png)

![填寫 Issue 標題與描述](images/ch06_02_new_issue_form.png)

> **建議**：Description 愈詳細，Agent 的產出品質愈高。建議包含：主題方向、目標讀者、參考資料連結、特殊要求等。

---

### 步驟 3：指派 Agent

在 Issue 頁面中，從 **Assignee** 下拉選單選擇要負責此任務的 Agent。

| 動作 | 說明 |
|------|------|
| 選擇 Assignee | 從下拉選單中選擇對應的 Agent |
| 自動觸發 | Agent 被指派後會自動醒來開始處理，無需手動 Heartbeat |
| 回應時間 | 約 30–120 秒內開始處理 |

**常見指派對應**

| 任務類型 | 建議指派 Agent |
|---------|---------------|
| 新聞稿撰寫 | W1 新聞記者風寫手 |
| 教學型文章 | W2 知識型教師風寫手 |
| 深度分析報告 | W3 學術分析師風寫手 |
| 故事型專題 | W4 說故事風寫手 |
| 評論觀點文 | W5 意見領袖風寫手 |
| 數據分析文 | W6 數據新聞風寫手 |
| 快報摘要 | W7 簡潔速讀風寫手 |
| 素材蒐集 | R1–R5 對應領域研究員 |
| 品質審查 | Q1 / Q2 審查員 |
| Odoo 同步 | S1 Odoo 同步助手 |

![指派 Agent — Assignee 下拉選單](images/ch06_03_assign_agent.png)

---

### 步驟 4：Issue 生命週期

Issue 從建立到完成，會經歷以下狀態轉換：

| 狀態 | 英文 | 說明 |
|------|------|------|
| 待辦 | `todo` | Issue 已建立但尚未開始處理 |
| 進行中 | `in_progress` | Agent 正在處理此 Issue |
| 完成 | `done` | Agent 已完成任務，產出已寫入 Comments |
| 阻塞 | `blocked` | Agent 無法完成任務（缺少資訊、權限不足等） |

**狀態流轉說明**

```
todo → in_progress → done
                  ↘ blocked（Agent 無法完成）
done → in_progress（品質審查退回，分數低於 7 分）
```

- 正常流程：`todo` → `in_progress` → `done`
- 審查退回：`done` → `in_progress`（Q1/Q2 審查員評分低於 7 分時，Issue 退回給原寫手修改）
- 阻塞情境：Agent 發現缺少必要資訊或遇到無法解決的問題時，標記為 `blocked`

![Issue 生命週期流程圖](images/ch06_04_issue_lifecycle.png)

---

### 步驟 5：查看 Agent 回覆

Agent 完成工作後，產出結果會以 **Comment** 的形式出現在 Issue 的留言串中。

**回覆內容因 Agent 類型而異：**

| Agent 類型 | 回覆內容 |
|-----------|---------|
| Research Agent（R1–R5） | 素材包：新聞摘要、數據引用、來源連結 |
| Writing Agent（W1–W7） | 完整 ESG 文章（含標題、摘要、正文、SEO 元素） |
| Quality Agent（Q1–Q2） | 品質評分表（1-10 分）與改善建議 |
| Management Agent（M1–M3） | 任務規劃、排程建議、狀態報告 |
| Sync Agent（S1） | 同步結果回報（成功 / 失敗） |

![Agent 回覆 — Issue Comment 中的文章產出](images/ch06_05_agent_reply.png)

![Agent 回覆 — 實際 ESG 文章範例](images/ch06_06_article_output.png)

---

### 步驟 6：Issue 狀態轉換

如需手動變更 Issue 狀態，可在 Issue 詳情頁中點選狀態欄位，從下拉選單選擇新狀態。

常見手動操作情境：

| 情境 | 操作 |
|------|------|
| Agent 未自動開始處理 | 手動將 `todo` 改為 `in_progress` 並重新指派 |
| 文章需要人工修改 | 將 `done` 改回 `in_progress` |
| 任務取消 | 將狀態改為 `done` 或刪除 Issue |
| 解除阻塞 | 補充資訊後將 `blocked` 改為 `todo` |

![手動變更 Issue 狀態](images/ch06_07_change_status.png)

---

### 步驟 7：搜尋與篩選 Issues

在 Issues 列表頁面中，可使用篩選功能快速找到特定 Issue。

**篩選條件**

| 篩選方式 | 說明 |
|---------|------|
| 依狀態 | 篩選 `todo` / `in_progress` / `done` / `blocked` |
| 依 Agent | 篩選特定 Agent 負責的 Issue |
| 依關鍵字 | 搜尋標題或描述中包含的關鍵字 |

![搜尋與篩選 Issues](images/ch06_08_filter_issues.png)

> **重要**：指派 Issue 給 Agent 後，Agent 會在約 30-120 秒內開始處理。不需要手動觸發 Heartbeat。

---

# 第七章：Routines 排程與 Goals 目標

本章說明如何設定定期排程任務（Routines），讓 Agent 自動執行工作，以及如何透過 Goals 追蹤月度產出目標。

---

### 步驟 1：什麼是 Routines

**Routines（排程）** 是設定好的定期重複任務。系統會依照指定的頻率與時間自動觸發 Agent 執行，無需人工介入。

**適用場景範例**

| 場景 | 說明 |
|------|------|
| Research Agent 每日掃描 | R1–R5 每天早上 08:00 自動搜集最新 ESG 資訊 |
| Writing Agent 每日撰稿 | W1–W7 每天上午 10:00 自動根據素材撰寫文章 |
| Management Agent 月度規劃 | M1 每月初制定當月內容策略 |
| 營運主編每日同步 | M3 每天 18:00 觸發 Odoo 同步 |

---

### 步驟 2：建立 Routine

1. 在側邊欄點選 **WORK** 區段下的 **Routines**
2. 點選 **New Routine** 或新增按鈕
3. 填寫排程設定：

| 欄位 | 說明 |
|------|------|
| Agent | 選擇要排程的 Agent |
| Frequency | 執行頻率：Daily / Weekly / Monthly |
| Time | 執行時間 |
| Description | 排程描述（可選） |

![建立 Routine — 選擇 Agent 與頻率](images/ch07_01_create_routine.png)

![建立 Routine — 設定執行時間](images/ch07_02_routine_time.png)

---

### 步驟 3：查看 Routine 執行歷史

建立 Routine 後，可在 Routines 列表中查看每次排程的執行紀錄，包含觸發時間、執行結果（成功 / 失敗）、耗時等資訊。

![Routine 執行歷史紀錄](images/ch07_03_routine_history.png)

---

### 步驟 4：建議的排程配置

以下是根據《ESGTimes AI Agent 大軍作戰手冊》建議的排程配置，確保各 Agent 協調運作，避免 Gateway 過載：

| 代號 | Agent 名稱 | 頻率 | 執行時間 | 說明 |
|------|-----------|------|---------|------|
| R1 | 政策法規研究員 | Daily | 08:00 | 每日掃描最新法規動態 |
| R2 | 企業案例研究員 | Daily | 08:00 | 每日蒐集企業 ESG 案例 |
| R3 | 數據趨勢研究員 | Daily | 08:00 | 每日整理數據與趨勢報告 |
| R4 | 國際媒體掃描員 | Daily | 08:00 | 每日掃描國際 ESG 新聞 |
| R5 | 台灣在地線人 | Daily | 08:00 | 每日追蹤台灣本地動態 |
| W1 | 新聞記者風寫手 | Daily | 10:00 | 根據素材撰寫新聞稿 |
| W2 | 知識型教師風寫手 | Daily | 10:00 | 根據素材撰寫教學文 |
| W3 | 學術分析師風寫手 | Daily | 10:00 | 根據素材撰寫深度分析 |
| W4 | 說故事風寫手 | Daily | 10:00 | 根據素材撰寫故事專題 |
| W5 | 意見領袖風寫手 | Daily | 10:00 | 根據素材撰寫評論觀點 |
| W6 | 數據新聞風寫手 | Daily | 10:00 | 根據素材撰寫數據分析 |
| W7 | 簡潔速讀風寫手 | Daily | 10:00 | 根據素材撰寫快報摘要 |
| M1 | 總編輯長 | Monthly | 月初 | 制定當月內容策略與主題 |
| M3 | 營運主編 | Daily | 18:00 | 觸發 Odoo 同步、整理日報 |
| P1 | 內容品質審查員 | Daily | 18:30 | 審查當日完成的文章 |
| S1 | Odoo 同步助手 | Daily | 18:30 | 同步已審核文章至 Odoo |

> **注意**：Research Agent（R1–R5）排在 08:00，Writing Agent（W1–W7）排在 10:00，確保寫手有最新的素材可參考。品質審查與同步排在 18:00–18:30，確保當日產出皆經審核後再同步。

> **注意**：同時段最多 5 個 Agent 並行。上表中 R1–R5 同時在 08:00 啟動（5 個），W1–W7 在 10:00 啟動（7 個，稍超過建議值）。實務上可將 W1–W7 分為兩批錯開 10 分鐘執行，以降低 Gateway 負載。

---

### 步驟 5：Goals 目標管理

**Goals** 功能可設定月度產出目標，追蹤 Agent 團隊的整體進度。

**建議月度目標**

| 目標 | 數值 | 說明 |
|------|------|------|
| 月產文章總數 | 100 篇 | 所有 Writing Agent 合計 |
| W1 新聞記者風 | 30 篇/月 | 最高產量寫手 |
| W2 知識型教師風 | 20 篇/月 | 第二高產量 |
| W3 學術分析師風 | 15 篇/月 | 深度分析文 |
| W4 說故事風 | 15 篇/月 | 故事專題 |
| W5 意見領袖風 | 10 篇/月 | 評論觀點 |
| W6 數據新聞風 | 5 篇/月 | 數據分析 |
| W7 簡潔速讀風 | 5 篇/月 | 快報摘要 |
| 品質通過率 | ≥ 85% | 首次審查即通過（7 分以上）的比例 |

**設定方式**

1. 在側邊欄點選 **WORK** 區段下的 **Goals**
2. 點選新增目標
3. 設定目標名稱、數值、追蹤週期（月）

![Goals 目標管理 — 設定月度目標](images/ch07_04_goals_setup.png)

![Goals 目標管理 — 進度追蹤儀表板](images/ch07_05_goals_progress.png)

> **建議**：每月初由 M1 總編輯長制定當月主題方向後，同步更新 Goals 目標，確保團隊方向一致。

---

## 本 Part 小結

| 章節 | 主題 | 關鍵重點 |
|------|------|---------|
| 第四章 | 登入與儀表板 | URL、帳密、側邊欄導覽、Inbox 通知、切換公司 |
| 第五章 | Agent 管理 | 18 個 Agent 列表、Instructions 編輯、Adapter 配置、Heartbeat 執行與監控 |
| 第六章 | Issue 管理 | Issue 建立、Agent 指派、生命週期、Agent 回覆查看、篩選搜尋 |
| 第七章 | Routines 與 Goals | 排程建立、建議時間配置、月度目標追蹤 |

> **下一步**：Part 3 將涵蓋 Odoo 同步、OpenClaw Gateway 技術細節、故障排除與進階配置。
# Part 3：Agent 各層詳解 — 管理、研究、寫作、審查、同步

> 本篇為 Paperclip AI ESGTimes 系統文件第三部分，詳細說明 18 位 AI Agent 的角色定義、System Prompt 重點、實際輸出範例與協作規則。涵蓋管理層（M1-M3）、研究層（R1-R5）、寫作層（W1-W7）、品質審查層（Q1-Q2）、以及同步層（S1）。

---

## Chapter 8：管理層 — M1, M2, M3

管理層共 3 位 Agent，負責內容策略、任務分配與營運管理。他們不直接產出文章，而是協調研究層和寫作層的工作流程，確保 ESGTimes 每月 100 篇文章的產出目標。

---

### 步驟 8.1：M1 總編輯長 👑

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | M1 |
| 名稱 | 總編輯長 |
| 圖示 | 👑 |
| 角色 | 制定內容策略、審核重大議題方向、品牌一致性 |
| 輸出格式 | 主題編號/主題名稱/優先級/建議W agent/背景說明 |

#### System Prompt 重點摘要

**身份定義**

M1 是 ESGTimes 的最高內容決策者，負責把握整體編輯方向。其核心職責包含：

1. 制定每週/每月的主題方向與重點議題
2. 審核重大議題的報導方向（避免品牌風險）
3. 確保各類文章的比例平衡（新聞、教學、深度、故事、評論）
4. 定期複盤各 Agent 的產出品質趨勢

**每月任務週期**

| 時間點 | 任務 | 輸出物 |
|--------|------|--------|
| 月初 | 發布「本月 ESG 重點議題清單」 | 5-10 個主題方向 |
| 月中 | 檢視文章產出進度與品質報告 | 進度報告 |
| 月末 | 產出「月度複盤報告」 | 各 Agent 產出統計與品質評分趨勢 |

**輸出格式規範**

```
主題編號：[YYYY-MM-XX]
主題名稱：[主題]
優先級：🔴 高 / 🟡 中 / 🟢 低
建議交給：[W1/W2/W3/W4/W5/W6/W7]
背景說明：[為什麼這個主題現在重要，2-3句]
參考素材來源：[R1/R2/R3/R4/R5]
```

**禁止事項**

- 不直接撰寫文章
- 不修改已通過 QA 的文章
- 不在沒有數據支撐的情況下調整策略
- 不介入具體寫作過程

#### 實際輸出範例（CMP-108）

以下為 CMP-108 測試中 M1 總編輯長產出的內容策略：

```
## ESGTimes 2026 年 6 月內容策略

### 本月重點議題清單

主題編號：2026-06-01
主題名稱：碳費第二階段徵收制度解析
優先級：🔴 高
建議交給：W1（新聞記者風）+ W2（知識型教師風）
背景說明：環境部預計 6 月公布碳費第二階段費率與適用範圍，
將影響全台超過 500 家企業，是本月最重要的政策議題。
參考素材來源：R1（政策法規研究員）、R5（台灣在地線人）

主題編號：2026-06-02
主題名稱：ISSB 準則全球採納進度追蹤
優先級：🔴 高
建議交給：W3（學術分析師風）
背景說明：ISSB IFRS S1/S2 已有超過 20 個司法管轄區宣布採納或對標，
台灣金管會亦規劃接軌時程，需深度分析對台灣企業的衝擊。
參考素材來源：R1、R4（國際媒體掃描員）

主題編號：2026-06-03
主題名稱：AI 永續治理與 ESG 數據管理
優先級：🟡 中
建議交給：W5（意見領袖風）+ W6（數據新聞風）
背景說明：AI 在 ESG 數據蒐集與分析的應用快速成長，
同時 AI 本身的碳足跡也成為新興議題。
參考素材來源：R3（數據趨勢研究員）

主題編號：2026-06-04
主題名稱：CBAM 碳邊境調整機制過渡期進展
優先級：🟡 中
建議交給：W1 + W7（簡潔速讀風）
背景說明：歐盟 CBAM 過渡期報告義務持續執行，台灣出口產業須密切關注，
特別是鋼鐵、鋁、水泥等高碳排產業。
參考素材來源：R1、R4

主題編號：2026-06-05
主題名稱：亞太碳市場發展與區域合作
優先級：🟢 低
建議交給：W4（說故事風）
背景說明：亞太地區多國正建立碳交易市場，
可從各國碳市場的故事切入，探討區域合作的可能性。
參考素材來源：R2（企業案例研究員）、R4
```

> **備註**：M1 的產出為策略指導文件，不直接產生文章。後續由 M2 內容主編將策略拆解為具體任務，再分派給對應的 Writing Agent。

![M1 總編輯長內容策略輸出](images/ch08_01_m1_content_strategy.png)

---

### 步驟 8.2：M2 內容主編 📋

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | M2 |
| 名稱 | 內容主編 |
| 圖示 | 📋 |
| 角色 | 日常排程、指派任務給寫手、初步品質把關 |
| 輸出格式 | 素材編號/分配給/分配原因/截稿時間/優先級 |

#### System Prompt 重點摘要

**身份定義**

M2 是 ESGTimes 的日常運營核心，負責將 Research Agent 產出的素材精準分配給最適合的 Writing Agent。

**核心職責**

1. 接收 Research Agents（R1-R5）產出的素材包
2. 根據素材內容和 M1 的主題方向，分配給最適合的 Writing Agent
3. 管理每日/每週的文章排程
4. 進行初步品質把關（格式檢查、字數確認）

**素材分配規則**

| 素材類型 | 指派 Agent | 說明 |
|----------|-----------|------|
| 即時新聞事件 | W1 新聞記者風 | 倒三角結構、中立客觀、800-1200字 |
| ESG 概念解釋 | W2 知識型教師風 | 親切教學、比喻類比、1000-1500字 |
| 需要深度分析的議題 | W3 學術分析師風 | 深度論據、多元角度、1500-2500字 |
| 有人物/企業故事的素材 | W4 說故事風 | 場景敘事、情感連結、1000-1800字 |
| 有爭議或觀點空間的議題 | W5 意見領袖風 | 明確觀點、犀利評論、800-1200字 |
| 數據密集的報告 | W6 數據新聞風 | 數據驅動、圖表建議、1000-1500字 |
| 需要懶人包的熱門議題 | W7 簡潔速讀風 | 3分鐘可讀、條列精華、400-700字 |

**每日工作時程**

| 時間 | 任務 |
|------|------|
| 09:00 | 彙整今日所有 Research Agent 產出的素材 |
| 09:30 | 根據分配規則分配素材給 Writing Agents |
| 17:00 | 彙整今日文章產出狀況 |

**工作規則**

1. 優先分配 M1 標記為高優先級的主題
2. 確保每個 Writing Agent 的每日工作量平衡
3. 同一天不給同一個 Writing Agent 超過 3 個任務
4. 緊急新聞（Research Agent 標記 🚨）優先分配給 W1

#### 實際輸出範例（CMP-122）

以下為 CMP-122 測試中 M2 內容主編產出的任務分配表：

```
## 本日任務分配表 — 2026/06/02

| # | 素材來源 | 素材主題 | 分配給 | 分配原因 | 優先級 | 截稿時間 |
|---|----------|----------|--------|----------|--------|----------|
| 1 | R1-0601 | 環境部碳費第二階段草案 | W1 新聞記者風 | 即時政策新聞，需倒三角報導 | 🔴 高 | 06/02 14:00 |
| 2 | R1-0601 | 碳費第二階段懶人包 | W7 簡潔速讀風 | 同主題需速讀版本搭配 | 🔴 高 | 06/02 16:00 |
| 3 | R2-0601 | 台積電 RE100 進度更新 | W4 說故事風 | 有企業轉型故事可挖掘 | 🟡 中 | 06/03 10:00 |
| 4 | R3-0601 | GSIA 永續投資報告 | W6 數據新聞風 | 數據密集，適合圖表呈現 | 🟡 中 | 06/03 14:00 |
| 5 | R4-0601 | CBAM 過渡期最新進展 | W3 學術分析師風 | 需深度分析台灣產業影響 | 🟡 中 | 06/04 10:00 |
| 6 | R5-0601 | 金管會永續揭露新規 | W2 知識型教師風 | ESG 揭露概念需教學解說 | 🟢 低 | 06/04 16:00 |

### 分配說明

- 碳費第二階段為 M1 指定 🔴 高優先級主題，安排 W1+W7 雙寫手同步產出
- W1 負責完整新聞報導（800-1200字），W7 負責速讀懶人包（400-700字）
- 台積電 RE100 案例有人物故事性，交由 W4 以場景敘事處理
- GSIA 報告數據量大，交由 W6 以數據新聞風格呈現，附圖表建議
```

> **備註**：M2 的分配結果會直接觸發對應 Writing Agent 的工作。每個分配單都會建立為 Paperclip Issue，assign 給對應的 Writing Agent。

![M2 內容主編任務分配表](images/ch08_02_m2_task_assignment.png)

---

### 步驟 8.3：M3 營運主編 ⚙️

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | M3 |
| 名稱 | 營運主編 |
| 圖示 | ⚙️ |
| 角色 | 電子報排程、社群排程、Odoo 同步觸發、數據報告 |
| 輸出格式 | 文章編號/預定發布時間/發布平台/狀態 |

#### System Prompt 重點摘要

**身份定義**

M3 是 ESGTimes 的營運後勤指揮官，負責文章從完成到發布的最後一哩路。

**核心職責**

1. 管理文章發布排程（WordPress 草稿 → 排程發布）
2. 規劃社群內容排程（LinkedIn、IG、電子報）
3. 觸發 Odoo 同步流程（done issues → Odoo）
4. 產出每週營運數據報告

**發布規則**

| 規則 | 說明 |
|------|------|
| 每日上限 | 最多 3 篇文章，避免讀者疲勞 |
| 重要文章 | 安排在週一或週三發布（流量高峰） |
| 電子報 | 每週一封，固定週四發送 |
| 發布時段 | 週一至週五 08:00-18:00，避開週末 |

**每日工作時程**

| 時間 | 任務 |
|------|------|
| 18:00 | 確認今日完成的文章狀態 |
| 18:30 | 設定明日預定發布的文章排程 |
| 每週五 | 產出本週營運報告 |

#### 實際輸出範例（CMP-123）

以下為 CMP-123 測試中 M3 營運主編產出的每週發布排程：

```
## ESGTimes 第 23 週發布排程（06/02 - 06/06）

### 週一 06/02 ── 流量高峰日
| 時段 | 文章 | 平台 | 狀態 |
|------|------|------|------|
| 08:30 | [CMP-201] 碳費第二階段徵收制度解析 | WordPress | 🟢 排程中 |
| 12:00 | [CMP-201] 社群摘要版 | LinkedIn | 🟢 排程中 |
| 14:00 | [CMP-205] 碳費懶人包 | WordPress | 🟢 排程中 |

### 週二 06/03
| 時段 | 文章 | 平台 | 狀態 |
|------|------|------|------|
| 09:00 | [CMP-202] 台積電 RE100 永續之路 | WordPress | 🟢 排程中 |
| 15:00 | [CMP-203] GSIA 永續投資數據解析 | WordPress | 🟡 草稿中 |

### 週三 06/04 ── 流量高峰日
| 時段 | 文章 | 平台 | 狀態 |
|------|------|------|------|
| 08:30 | [CMP-204] CBAM 碳邊境機制深度分析 | WordPress | 🟡 待 QA |
| 12:00 | [CMP-204] 社群摘要版 | LinkedIn | ⏳ 等待文章通過 |
| 16:00 | [CMP-206] ESG 揭露入門指南 | WordPress | 🟡 草稿中 |

### 週四 06/05 ── 電子報日
| 時段 | 文章 | 平台 | 狀態 |
|------|------|------|------|
| 08:00 | 本週電子報（含 5 篇摘要） | Email | 🟡 編輯中 |
| 10:00 | [CMP-207] ESG 評級機構比較 | WordPress | 🟡 草稿中 |

### 週五 06/06
| 時段 | 文章 | 平台 | 狀態 |
|------|------|------|------|
| 09:00 | [CMP-208] 亞太碳市場觀察 | WordPress | 🟡 待 QA |
| 17:00 | 本週營運報告 | 內部 | ⏳ 待產出 |

### 本週統計
- 預定發布文章：8 篇
- WordPress：6 篇
- LinkedIn：2 則社群摘要
- 電子報：1 期
- Odoo 同步排程：週五 18:00 批次同步
```

> **備註**：M3 的排程會根據 Q1/Q2 的審查結果動態調整。尚未通過品質審查的文章標記為「🟡 待 QA」，通過後自動轉為「🟢 排程中」。

![M3 營運主編每週排程](images/ch08_03_m3_weekly_schedule.png)

---

## Chapter 9：研究層 — R1 ~ R5

研究層共 5 位 Research Agent，各自負責不同的 ESG 資訊領域。他們的任務是監控國際與台灣在地的 ESG 資訊源，產出結構化的素材包（Material Package），交由寫作層的 Writing Agent 撰寫文章。

> **核心原則**：Research Agent 只負責找素材、整理素材，絕不撰寫文章、不加入個人觀點、不輸出沒有來源的資料。

---

### 步驟 9.1：R1 政策法規研究員 📜

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | R1 |
| 名稱 | 政策法規研究員 |
| 圖示 | 📜 |
| 監控領域 | ESG 相關法規與國際標準動態 |

#### 監控來源

**國際法規 / 標準**
- ISSB（國際永續準則委員會）：ifrs.org/groups/issb
- GRI 標準更新：globalreporting.org
- TCFD 框架：fsb-tcfd.org
- EU CSRD / ESRS 官方進度：ec.europa.eu
- SEC ESG 相關規則：sec.gov

**台灣在地法規**
- 金管會 ESG 相關公告：fsc.gov.tw
- 台灣證交所永續發展專區：twse.com.tw
- 環境部法規動態：moenv.gov.tw

#### 輸出格式

```
標題：[法規/標準名稱] [最新動態一句話摘要]
來源：[機構名稱] + [原始連結]
日期：[發布或更新日期]
重要程度：⭐⭐⭐⭐⭐ / ⭐⭐⭐⭐ / ⭐⭐⭐（依影響範圍評分）
適用對象：[哪類企業/投資人/產業受影響]
核心摘要：[3-5 點條列，每點不超過 2 句話]
建議寫作角度：[建議給哪類 Writing Agent，以及切入點]
```

**工作規則**

| 規則 | 說明 |
|------|------|
| 每次產出量 | 5-10 則素材 |
| 最低星等 | ≥ ⭐⭐⭐ 才輸出 |
| 新鮮度 | 只輸出 30 天內的最新資料 |
| 緊急標記 | 含「強制揭露」「罰則」「截止日期」的法規自動標記 🚨 |
| 合併規則 | 同一法規的細節更新合併為一則 |

#### 實際輸出範例（CMP-109）

```
## R1 政策法規研究員 — 2026 年 6 月素材包

### 素材 1
標題：ISSB IFRS S1/S2 全球實施指引更新 — 新增中小企業簡化揭露路徑
來源：ISSB 官方 + ifrs.org/groups/issb/implementation-guidance
日期：2026-05-22
重要程度：⭐⭐⭐⭐⭐
適用對象：所有上市公司、資產管理機構、ESG 顧問
核心摘要：
1. ISSB 發布中小企業 IFRS S1/S2 簡化揭露框架，降低合規門檻
2. 新增「氣候情境分析」範例模板，提供三種氣溫情境的標準化方法
3. 與 GRI 標準的互操作性指引已更新，減少企業雙重報告負擔
4. 台灣金管會預計參照此指引制定本土化版本
建議寫作角度：交給 W3（學術分析師風），從「全球永續揭露準則整合趨勢」切入，
比較 ISSB vs CSRD vs SEC 三大框架的異同

### 素材 2
標題：台灣環境部碳費第二階段費率草案預告 — 每噸 500 元起跳 🚨
來源：環境部 + moenv.gov.tw/公告
日期：2026-05-28
重要程度：⭐⭐⭐⭐⭐（緊急優先 🚨）
適用對象：年排碳量 2.5 萬噸以上企業、鋼鐵/水泥/石化業
核心摘要：
1. 碳費費率從第一階段的每噸 300 元調高至 500 元，預計 2027 年 1 月實施
2. 適用範圍從 287 家擴大至約 500 家企業
3. 新增「自主減量計畫優惠費率」，達標企業可享 60% 折扣
4. 碳費收入專款專用於低碳轉型基金
5. 公眾意見徵詢期至 2026 年 7 月底
建議寫作角度：交給 W1（新聞記者風）做即時報導 + W7（簡潔速讀風）做懶人包
```

![R1 政策法規研究員素材包](images/ch09_01_r1_policy_materials.png)

---

### 步驟 9.2：R2 企業案例研究員 🏢

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | R2 |
| 名稱 | 企業案例研究員 |
| 圖示 | 🏢 |
| 監控領域 | 企業 ESG 實踐案例與永續進展 |

#### 監控來源

**國際企業動態**
- GreenBiz：greenbiz.com
- Bloomberg Green：bloomberg.com/green
- Reuters Sustainability：reuters.com/sustainability
- Fortune ESG reporting
- CDP 評分公告：cdp.net

**台灣企業**
- 台灣企業永續獎（TCSA）：csr.cw.com.tw
- 天下 CSR 報告：csr.cw.com.tw
- 各大上市公司永續報告書發布新聞
- 台灣指標型企業（台積電、鴻海、台達電、富邦等）ESG 動態

**評等與排名**
- MSCI ESG 評等更新
- Sustainalytics 評等
- Dow Jones Sustainability Index（DJSI）公告
- Forbes ESG 相關排名

#### 輸出格式

```
公司名稱：[全名 + 所屬產業]
案例類型：[碳中和 / 供應鏈治理 / 社會責任 / 董事會多元 / 其他]
來源：[媒體/報告名稱] + [連結]
日期：[發布日期]
案例核心：[這間公司做了什麼？結果是什麼？用 3-5 句話說明]
亮點數據：[關鍵數字，例如：減碳 40%、女性董事佔比 50% 等]
故事潛力評分：⭐⭐⭐⭐⭐（是否適合寫成故事敘事文章）
建議文章類型：[故事敘事 / 深度分析 / 數據新聞 / 快訊]
```

**工作規則**

1. 優先找「有具體數字」的案例，避免只有宣示沒有成果的新聞
2. 台灣本地企業案例優先，其次為亞太，再次為歐美
3. 避免重複報導同一家公司（30 天內同公司最多 2 則）
4. 發現「漂綠爭議」或「ESG 評等被下調」的新聞也要蒐集，標記為「爭議案例」⚠️

#### 實際輸出範例（CMP-110）

```
## R2 企業案例研究員 — 本月企業 ESG 素材包

### 素材 1
公司名稱：台灣積體電路製造股份有限公司（台積電） / 半導體產業
案例類型：碳中和 / 供應鏈治理
來源：台積電 2025 永續報告書 + 天下 CSR 專題報導
日期：2026-05-15
案例核心：
1. 台積電宣布加入 RE100，承諾 2040 年全球營運 100% 使用再生能源
2. 已完成 SBTi（科學基礎減碳目標）審核，設定 2030 年減碳 25% 目標
3. 推動「供應鏈碳管理計畫」，要求前 100 大供應商 2025 年底前完成碳盤查
4. 建置台灣半導體業首座廠內太陽能系統，年發電量 2.5 億度
5. 投入 NT$100 億設立「綠色製造基金」支持供應鏈低碳轉型
亮點數據：
- RE100 承諾：2040 年 100% 再生能源
- SBTi 目標：2030 年減碳 25%（以 2020 為基準年）
- 供應鏈：前 100 大供應商碳盤查覆蓋率已達 87%
- 再生能源採購量：年增 45%
- 綠色製造基金：NT$100 億
故事潛力評分：⭐⭐⭐⭐⭐
建議文章類型：故事敘事（W4）— 從台積電如何帶動供應鏈轉型的角度切入
```

![R2 企業案例研究員素材包](images/ch09_02_r2_enterprise_cases.png)

---

### 步驟 9.3：R3 數據趨勢研究員 📊

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | R3 |
| 名稱 | 數據趨勢研究員 |
| 圖示 | 📊 |
| 監控領域 | ESG 數據報告與調查研究 |

#### 監控來源

**研究機構報告**
- IPCC 最新報告：ipcc.ch
- IEA 能源報告：iea.org
- PwC ESG 調查
- McKinsey Sustainability 報告
- Deloitte Global Millennial Survey
- EY ESG 相關研究

**財務與投資數據**
- Bloomberg NEF（新能源金融）
- Morningstar ESG 基金流向報告
- Global Sustainable Investment Alliance（GSIA）
- 綠色債券市場數據：climatebonds.net

**台灣在地數據**
- 環境部溫室氣體盤查統計
- 金管會上市公司永續報告統計
- 台灣工業總會永續調查

#### 輸出格式

```
報告名稱：[正式報告全名]
發布機構：[機構名稱 + 公信力評級：A/B/C]
發布日期：[年月]
報告連結：[URL]
核心數據 Top 5：
1. [數據點] → [意義/解讀]
2. [數據點] → [意義/解讀]
3. [數據點] → [意義/解讀]
4. [數據點] → [意義/解讀]
5. [數據點] → [意義/解讀]
數據新鮮度：[是否為最新版本？距上次更新幾個月？]
可視化潛力：⭐⭐⭐⭐⭐（適合做圖表的程度）
建議文章切角：[趨勢分析 / 台灣對比 / 產業排名 / 投資影響]
```

**工作規則**

1. 只接受有方法論說明的報告，避免「來路不明的統計」
2. 數據必須標明年份，超過 2 年的舊數據標記 [舊數據⚠️]
3. 如果找到台灣數據與國際數據可對比的，標記「對比潛力🔥」
4. 每次輸出至少 3 則報告摘要

#### 實際輸出範例（CMP-118）

```
## R3 數據趨勢研究員 — 本月重要報告素材包

### 報告 1
報告名稱：Global Sustainable Investment Review 2026
發布機構：GSIA（Global Sustainable Investment Alliance） / 公信力：A
發布日期：2026-04
報告連結：gsi-alliance.org/reports/2026
核心數據 Top 5：
1. 全球永續投資總額達 US$41.2 兆 → 較 2024 年增長 18%，增速首次超過傳統投資
2. 亞太地區永續投資佔比升至 12% → 落後歐美但增速最快（年增 35%）
3. ESG 整合策略仍佔最大比例（67%）→ 顯示 ESG 已從「加值」變「標配」
4. 排除篩選策略持續下降至 15% → 投資人從「避免壞公司」轉向「投資好公司」
5. 影響力投資年增 42% → 最快速增長的子類別，但基數仍小
數據新鮮度：最新版（2026 年 4 月發布），距上版 2 年
可視化潛力：⭐⭐⭐⭐⭐（趨勢折線圖 + 區域對比柱狀圖）
建議文章切角：趨勢分析 + 台灣對比（對比潛力🔥）

### 報告 2
報告名稱：Morningstar Global ESG Fund Flows Q1 2026
發布機構：Morningstar / 公信力：A
發布日期：2026-05
報告連結：morningstar.com/esg-fund-flows-2026q1
核心數據 Top 5：
1. 全球 ESG 基金 Q1 淨流入 US$12.8 億 → 連續 3 季正成長，走出 2023 低谷
2. 歐洲佔全球 ESG 基金資產 82% → 領先地位不變
3. 美國 ESG 基金連續 2 季淨流出 → 反 ESG 政策影響持續
4. 亞太 ESG 基金規模突破 US$500 億 → 日本、台灣、韓國為主要市場
5. 文章 8 合規基金（SFDR）佔歐洲新基金 45% → 法規驅動效果顯著
數據新鮮度：最新季度報告
可視化潛力：⭐⭐⭐⭐（資金流向河流圖）
建議文章切角：投資影響 — 台灣 ESG 基金市場機會

### 報告 3
報告名稱：BloombergNEF New Energy Outlook 2026
發布機構：Bloomberg NEF / 公信力：A
發布日期：2026-05
報告連結：bnef.com/insights/neo-2026
核心數據 Top 5：
1. 全球再生能源投資 2025 年達 US$5,800 億 → 首次超過化石燃料投資
2. 太陽能成本較 2010 年下降 92% → 已成全球最便宜電力來源
3. 全球電動車滲透率達 24% → 中國 45%、歐洲 28%、台灣 8%
4. 電池儲能裝置容量年增 110% → 解決再生能源間歇性問題
5. 預計 2050 年淨零路徑需額外 US$31 兆投資 → 目前缺口約 40%
數據新鮮度：最新年度報告
可視化潛力：⭐⭐⭐⭐⭐（成本下降曲線 + 投資趨勢圖 + 台灣對比🔥）
建議文章切角：台灣對比 — 台灣再生能源投資與全球的差距
```

![R3 數據趨勢研究員報告摘要](images/ch09_03_r3_data_trends.png)

---

### 步驟 9.4：R4 國際媒體掃描員 🌍

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | R4 |
| 名稱 | 國際媒體掃描員 |
| 圖示 | 🌍 |
| 監控領域 | 國際 ESG 媒體新聞篩選 |

#### 監控來源

**第一梯隊（每日必掃）**
- GreenBiz：greenbiz.com/news
- Carbon Brief：carbonbrief.org
- Climate Home News：climatechangenews.com
- ESG Today：esgtoday.com
- Responsible Investor：responsible-investor.com

**第二梯隊（每週掃描）**
- The Guardian Environment：theguardian.com/environment
- Financial Times Moral Money（訂閱制，找摘要）
- Bloomberg Green 免費內容
- Axios Generate newsletter

**亞太焦點**
- Eco-Business（亞太 ESG 媒體）：eco-business.com
- China Water Risk：chinawaterrisk.org
- Climate Tracker Asia

#### 篩選標準

只輸出台灣關聯度 🔴高 或 🟡中 的新聞，符合以下至少一項：

- 與台灣直接相關（台灣企業、供應鏈、氣候風險）
- 全球政策改變，台灣企業必須跟進的
- 重大國際排名或評等公布
- 科技突破或新解決方案
- 具強烈故事性的企業或人物案例

#### 輸出格式

```
新聞標題：[原文標題]
來源媒體：[媒體名] + [連結]
發布時間：[日期]
台灣關聯度：🔴高 / 🟡中
一句話摘要：[20字以內說明這則新聞的核心]
延伸報導潛力：[可以往哪個方向深化成 ESGTimes 的文章]
建議交給：[哪類 Writing Agent]
```

#### 實際輸出範例（CMP-111）

```
## R4 國際媒體掃描員 — 本週國際 ESG 新聞 Top 5

### 新聞 1
新聞標題：EU Reaches Deal on Corporate Sustainability Due Diligence Directive
來源媒體：ESG Today + esgtoday.com/eu-csddd-final-deal
發布時間：2026-05-25
台灣關聯度：🔴高
一句話摘要：歐盟供應鏈盡職調查指令定案，影響台灣出口企業
延伸報導潛力：分析台灣出口企業（電子、紡織）需如何應對 CSDDD 合規要求
建議交給：W1（即時新聞）+ W3（深度分析）

### 新聞 2
新聞標題：Apple Drops Three Suppliers Over Failure to Meet Clean Energy Targets
來源媒體：GreenBiz + greenbiz.com/apple-suppliers-clean-energy
發布時間：2026-05-23
台灣關聯度：🔴高
一句話摘要：Apple 因再生能源不達標淘汰 3 家供應商
延伸報導潛力：台灣 Apple 供應鏈（台積電、鴻海、和碩）的再生能源壓力
建議交給：W4（故事角度）+ W1（新聞報導）

### 新聞 3
新聞標題：Global Carbon Credit Market Hits $2B Milestone
來源媒體：Carbon Brief + carbonbrief.org/carbon-credits-milestone
發布時間：2026-05-26
台灣關聯度：🟡中
一句話摘要：全球自願碳權市場規模突破 20 億美元
延伸報導潛力：台灣碳權交易所定位與機會分析
建議交給：W6（數據新聞）

### 新聞 4
新聞標題：ASEAN Green Taxonomy Finalized After 3-Year Consultation
來源媒體：Eco-Business + eco-business.com/asean-taxonomy
發布時間：2026-05-24
台灣關聯度：🟡中
一句話摘要：東協綠色分類法定案，影響區域供應鏈布局
延伸報導潛力：台灣新南向企業在東協面臨的 ESG 合規新挑戰
建議交給：W3（學術分析）

### 新聞 5
新聞標題：World's Largest Direct Air Capture Plant Opens in Iceland
來源媒體：The Guardian + theguardian.com/dac-iceland
發布時間：2026-05-27
台灣關聯度：🟡中
一句話摘要：冰島啟用全球最大碳捕捉設施，年捕 36,000 噸
延伸報導潛力：碳捕捉技術現況與台灣應用可能性
建議交給：W2（教學解說）
```

![R4 國際媒體掃描結果](images/ch09_04_r4_international_scan.png)

---

### 步驟 9.5：R5 台灣在地線人 🇹🇼

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | R5 |
| 名稱 | 台灣在地線人 |
| 圖示 | 🇹🇼 |
| 監控領域 | 台灣本地 ESG 動態全方位追蹤 |

#### 監控來源

**政府與監管**
- 金管會新聞稿：fsc.gov.tw
- 環境部新聞稿：moenv.gov.tw
- 經濟部能源署：moeaea.gov.tw
- 國發會淨零轉型：ndc.gov.tw
- 證交所永續資訊：twse.com.tw

**台灣媒體 ESG 報導**
- 天下雜誌 CSR 專區：csr.cw.com.tw
- 遠見雜誌 ESG 頻道
- 經濟日報 ESG 版
- 工商時報 ESG 相關報導
- 聯合報 地球日 / 永續專題

**產業公會與協會**
- 台灣氣候聯盟
- 台灣企業永續協會（TCSB）
- 工業技術研究院（ITRI）低碳相關報告
- 中華民國全國工業總會

**社群與論壇**
- LinkedIn 台灣 ESG 從業者討論
- Dcard / PTT 相關討論（用於了解一般大眾認知）

#### 輸出格式

```
新聞標題：[標題]
來源：[媒體/機構] + [連結]
日期：[日期]
涉及產業：[半導體 / 金融 / 製造 / 零售 / 其他]
關鍵企業：[涉及哪些公司（如有）]
政策影響層級：[只影響個別企業 / 整個產業 / 全體上市公司]
核心摘要：[3 點條列]
獨家性評分：⭐⭐⭐⭐⭐（是否只有少數媒體報導，ESGTimes 有搶先機會）
```

**工作規則**

1. 每次輸出 5-10 則
2. 重大政策公告（如金管會新規）標記「重要公告 🚨」，立即輸出
3. 台灣企業永續獎、各類 ESG 排名公布日前後要加強掃描
4. 追蹤台灣「第一次做到」的 ESG 成就（台灣第一家...、亞洲首家...）
5. 記錄每月台灣 ESG 大事件，月底輸出一份「本月回顧」

#### 實際輸出範例（CMP-119）

```
## R5 台灣在地線人 — 本週台灣 ESG 動態 Top 5

### 動態 1 🚨 重要公告
新聞標題：金管會公布「上市櫃公司永續發展行動方案 3.0」
來源：金管會 + fsc.gov.tw/ch/home.jsp
日期：2026-05-26
涉及產業：全產業
關鍵企業：所有上市櫃公司
政策影響層級：全體上市公司
核心摘要：
1. 2027 年起全體上市公司須依 ISSB 準則揭露永續資訊，取消分階段實施
2. 新增「供應鏈碳管理」揭露要求，上市公司須揭露前 10 大供應商碳排
3. 永續報告書須經第三方確信，費用可列為合理營業支出
獨家性評分：⭐⭐⭐（多家媒體同步報導，但深度解析機會大）

### 動態 2
新聞標題：台達電獲選 DJSI 世界指數成分股，連續 13 年入選
來源：經濟日報 ESG 版
日期：2026-05-24
涉及產業：電子 / 製造
關鍵企業：台達電（2308）
政策影響層級：只影響個別企業
核心摘要：
1. 台達電在「電子設備、儀器與零組件」類別排名全球第一
2. 在氣候策略、環境報告與人權三項獲滿分
3. 預計 2030 年前達成碳中和目標，目前進度超前 15%
獨家性評分：⭐⭐⭐⭐（台灣媒體報導較少，可做深度解析）

### 動態 3
新聞標題：環境部核定首批碳權交易平台，3 家業者取得許可
來源：環境部 + moenv.gov.tw
日期：2026-05-27
涉及產業：金融 / 碳管理
關鍵企業：臺灣碳權交易所、中華民國碳交易協會
政策影響層級：整個產業
核心摘要：
1. 環境部正式核定 3 家碳權交易平台業者，預計 Q3 開始交易
2. 初期僅開放國內自願減量額度交易，國際碳權暫不納入
3. 交易平台須每季公布碳權價格指數與交易量統計
獨家性評分：⭐⭐⭐⭐⭐（首批核定，搶先報導機會大）

### 動態 4
新聞標題：遠見 ESG 企業永續獎 2026 年入圍名單公布
來源：遠見雜誌 ESG 頻道
日期：2026-05-25
涉及產業：全產業
關鍵企業：入圍企業含台積電、富邦金、中鋼、統一等 50 家
政策影響層級：只影響個別企業
核心摘要：
1. 2026 年共 280 家企業報名，50 家入圍決審
2. 新增「淨零轉型特別獎」，鼓勵具體減碳行動
3. 頒獎典禮預計 7 月舉辦，屆時可做系列報導
獨家性評分：⭐⭐（多家媒體已報導，但系列追蹤仍有價值）

### 動態 5
新聞標題：台灣首家碳中和中小企業誕生 — 桃園紡織廠達成里程碑
來源：天下雜誌 CSR + csr.cw.com.tw
日期：2026-05-28
涉及產業：紡織 / 製造
關鍵企業：XX 紡織（化名）
政策影響層級：只影響個別企業
核心摘要：
1. 桃園某紡織中小企業宣布達成營運碳中和，為台灣中小企業首例
2. 投入 3 年、花費約 NT$2,000 萬完成碳盤查、減碳與碳抵換
3. 經 SGS 第三方驗證，獲頒 PAS 2060 碳中和認證
獨家性評分：⭐⭐⭐⭐⭐（台灣第一家中小企業碳中和，獨家性極高！）
```

![R5 台灣在地線人動態掃描](images/ch09_05_r5_taiwan_local.png)

---

### 步驟 9.6：Research Agent 總覽對照表

| 項目 | R1 政策法規 📜 | R2 企業案例 🏢 | R3 數據趨勢 📊 | R4 國際媒體 🌍 | R5 台灣在地 🇹🇼 |
|------|---------------|---------------|---------------|---------------|-----------------|
| 監控焦點 | 法規與標準 | 企業實踐 | 數據報告 | 國際新聞 | 台灣動態 |
| 國際來源 | ISSB, GRI, TCFD, CSRD, SEC | GreenBiz, Bloomberg, CDP | IPCC, IEA, PwC, McKinsey, BNEF | GreenBiz, Carbon Brief, ESG Today | -- |
| 台灣來源 | 金管會, 證交所, 環境部 | TCSA, 天下CSR, 指標企業 | 環境部統計, 金管會統計 | Eco-Business (亞太) | 金管會, 環境部, 天下, 遠見, 經濟日報 |
| 每次產出量 | 5-10 則 | 不限 | ≥ 3 則 | 5-8 則 | 5-10 則 |
| 篩選門檻 | ≥ ⭐⭐⭐ | 有具體數字 | 有方法論 | 台灣關聯度 🔴/🟡 | 30天內 |
| 緊急標記 | 🚨 強制/罰則/截止日 | ⚠️ 漂綠爭議 | 🔥 台灣對比潛力 | -- | 🚨 重大政策公告 |
| 主要服務 W Agent | W1, W3 | W4, W3 | W6, W3 | W1, W3, W4 | W1, W2, W5 |

---

## Chapter 10：寫作層 — W1 ~ W7

寫作層共 7 位 Writing Agent，各有獨特的寫作風格。他們接收 Research Agent 產出的素材包，依據各自的文章結構模板撰寫文章。每月合計目標產出 100 篇文章。

---

### 步驟 10.0：SEO 格式規則（全部 7 個寫手共用）

所有 Writing Agent 的 System Prompt 末尾都附加以下 9 條 SEO 格式規則。這些規則只規範文章格式與結構，完全不干預各 Agent 的風格、語氣與核心寫作方式。

| 編號 | 規則名稱 | 說明 |
|------|----------|------|
| 規則 1 | 標題格式 | 直接帶主題與主關鍵字，使用：X是什麼？/ X懶人包 / X一次搞懂 / X完整解析，並帶出文章涵蓋的 2-3 個面向 |
| 規則 2 | 開頭前言 | 正文前 1-2 小段：(1) 主題為什麼重要 (2) 本文要介紹哪些內容 |
| 規則 3 | H2 主段結構 | 每個 H2 是獨立可閱讀的小主題，標題用問句或說明句（X是什麼？/ X怎麼分？/ X有哪些？） |
| 規則 4 | 段落排序 | 由淺到深：定義 → 背景 → 架構原則 → 流程步驟 → 分類等級 → 延伸應用 → 結尾 |
| 規則 5 | H2 內部 | 先寫引導句（總述），再展開條列（分述）。不直接從條列開始 |
| 規則 6 | 條列優先 | 流程、原則、構面、步驟、等級一律用數字編號或點列，不塞進敘述段落 |
| 規則 7 | 表格排列 | 對照型資訊用表格或接近表格的邏輯整理 |
| 規則 8 | 結尾段 | 正文後必須有尾段（結論整理/服務說明/收尾整理），不可直接收掉 |
| 規則 9 | 延伸閱讀 | 視情況附：延伸閱讀：[主題1] / [主題2] / [主題3] |

> **備註**：若需還原，只要刪除 System Prompt 中從「## SEO 格式規則」到區塊結束的內容即可。SEO 規則完全不影響各 Agent 的核心風格定義。

---

### 步驟 10.1：W1 新聞記者風 📰

| 欄位 | 內容 |
|------|------|
| 代號 | W1 |
| 名稱 | 新聞記者風 |
| 圖示 | 📰 |
| 月產量 | 30 篇 |
| 核心風格 | 倒三角結構、中立客觀 |
| 字數範圍 | 800-1,200 字 |

**文章結構模板**

1. 導言（最重要事實，1段，回答 Who/What/When）
2. 事件背景（2-3段，解釋為什麼這件事重要）
3. 利害關係人反應或引述（1-2段）
4. 數據佐證（1段）
5. 後續影響與展望（1段）
6. 編輯補充說明（1句，提供背景脈絡）

**標題公式**：[主體] + [動詞] + [核心事件] + [時間/範圍（選填）]

**禁止事項**：不寫個人觀點、不使用第一人稱、不使用驚嘆號於標題、不在沒有來源的情況下引用數字

#### 實際輸出範例（CMP-112）

```
## W1 新聞記者風 — CBAM 新聞報導

### 標題備選（3 個）
1. 歐盟 CBAM 碳邊境稅過渡期延長至 2027，台灣出口業爭取緩衝時間
2. CBAM 過渡期報告首輪結果出爐，台灣鋼鐵業碳揭露達標率僅 62%
3. 歐盟碳邊境調整機制最新進展：台灣出口產業須知的 5 項關鍵變化

### 正文

歐盟碳邊境調整機制（CBAM）過渡期第一輪報告結果近日公布，
涵蓋全球超過 5,000 家進口商的碳含量申報資料。其中，
台灣有 127 家出口企業提交報告，鋼鐵與鋁製品業者佔比最高。

CBAM 於 2023 年 10 月啟動過渡期，要求進口商申報產品的碳含量，
但暫不徵收費用。歐盟執委會原定 2026 年正式開徵，
但考量全球供應鏈準備不足，已宣布過渡期延長至 2027 年 6 月。

台灣經濟部工業局表示，國內受影響的出口企業約 300 家，
目前已完成碳盤查的比例為 78%，但符合 CBAM 報告格式要求的僅 62%。
工業局已編列 NT$5 億預算協助企業建置碳排計算系統。

根據歐盟執委會公布的數據，過渡期間全球申報的碳含量平均值
為每噸產品 1.8 噸 CO₂e，其中台灣鋼鐵業的碳密集度為 1.6 噸 CO₂e，
略低於全球平均，但高於歐盟本地生產的 1.2 噸 CO₂e。

業界預估，CBAM 正式開徵後，台灣鋼鐵出口至歐盟的成本將增加 8-12%。
台灣鋼鐵公會建議政府應加速推動國內碳定價機制，
以減少企業面臨的碳關稅衝擊。

（補充：CBAM 全名為 Carbon Border Adjustment Mechanism，
目前涵蓋鋼鐵、鋁、水泥、化肥、電力、氫 六大產業類別。）

### Meta Description（120字以內）
歐盟 CBAM 過渡期延長至 2027 年，台灣 127 家企業完成首輪碳含量申報，
鋼鐵業碳揭露達標率 62%。本文解析最新進展對台灣出口產業的影響。
```

![W1 新聞記者風 CBAM 報導](images/ch10_01_w1_news_cbam.png)

---

### 步驟 10.2：W2 知識型教師風 🎓

| 欄位 | 內容 |
|------|------|
| 代號 | W2 |
| 名稱 | 知識型教師風 |
| 圖示 | 🎓 |
| 月產量 | 20 篇 |
| 核心風格 | 親切教學、比喻類比 |
| 字數範圍 | 1,000-1,500 字 |

**文章結構模板**

1. 開場鉤子（提出讀者痛點或一個讓人意外的問題，1段）
2. 「這是什麼」（用1段給出清楚定義，附上比喻）
3. 「為什麼你需要懂這個」（2-3個理由）
4. 主體：步驟式說明或分點解析（3-7點，每點有小標題）
5. 常見錯誤或注意事項（2-3點）
6. 結尾行動呼籲（鼓勵讀者採取第一步）

**標題公式選項**
- 「[名詞]是什麼？[X]分鐘看懂」
- 「[X]步驟搞定[任務]」
- 「你一定搞錯的[X]件 ESG 事」

**禁止事項**：不寫過於學術的長句、不假設讀者已有 ESG 背景知識、不省略步驟間的邏輯連結、不用「如前所述」「綜上所述」等學術用語

#### 實際輸出範例（CMP-113）

```
## W2 知識型教師風 — ESG 入門指南

### 標題
ESG 是什麼？5 分鐘搞懂環境、社會、治理三大面向 — 從零開始的永續投資入門

### 正文

你可能已經在新聞上看過無數次「ESG」這三個字，
但老實說 — 你真的搞懂它在說什麼了嗎？

別擔心，你不是唯一感到困惑的人。根據一份調查，
台灣有超過 70% 的上班族聽過 ESG，但只有 23% 能說出它代表什麼。
今天，我們就用最白話的方式，帶你一次搞懂。

## ESG 到底是什麼？

簡單來說，ESG 就像是一張企業的「健康檢查報告」，
只不過它不只檢查財務數字，還檢查三個面向：

- **E（Environment 環境）**：這家公司對地球好不好？碳排多少？用水省不省？
- **S（Social 社會）**：這家公司對人好不好？員工權益、社區關係、多元包容做得如何？
- **G（Governance 治理）**：這家公司管理得好不好？董事會獨立嗎？有沒有貪腐風險？

換句話說，ESG 就是用「環境、社會、治理」三把尺來量一家企業，
看看它除了賺錢，還做了哪些「賺錢以外的事」。

## 為什麼你需要懂 ESG？

你可能會想：「我又不是企業主管，幹嘛懂這個？」
其實，ESG 已經跟你的生活息息相關：

1. **你的退休金正在投資 ESG**：台灣勞動基金已將 ESG 納入投資篩選
2. **你的工作機會受影響**：越來越多企業要求供應商做 ESG 報告
3. **你的消費選擇也是一票**：選擇環保產品，就是用鈔票投票

...（後續省略，完整文章約 1,200 字）
```

![W2 知識型教師風 ESG 入門](images/ch10_02_w2_esg_guide.png)

---

### 步驟 10.3：W3 學術分析師風 🔬

| 欄位 | 內容 |
|------|------|
| 代號 | W3 |
| 名稱 | 學術分析師風 |
| 圖示 | 🔬 |
| 月產量 | 15 篇 |
| 核心風格 | 深度論據、多元角度 |
| 字數範圍 | 1,500-2,500 字 |

**文章結構模板**

1. 摘要（Executive Summary）：核心論點一句話 + 3-5 個主要發現
2. 問題背景與現況
3. 核心分析（分 3-4 個面向，各有小標題）：現況說明 → 數據佐證 → 意義解讀
4. 反向觀點與侷限性（必須包含）
5. 結論與未來展望
6. 延伸閱讀方向

**禁止事項**：不下沒有論據的結論、不只陳述事實而無分析、不迴避爭議性議題、不寫超過60字的長句

#### 實際輸出範例（CMP-120）

```
## W3 學術分析師風 — CSRD vs ISSB 深度分析

### 標題
CSRD 與 ISSB 的角力與整合：全球永續揭露準則雙軌制的現況、衝突與未來走向

### 摘要（Executive Summary）

核心論點：CSRD 與 ISSB 正從「競爭」走向「互補」，
但企業短期內仍面臨雙重合規成本的挑戰。

主要發現：
- CSRD 採用「雙重重大性」原則，ISSB 採用「財務重大性」，兩者差異正在縮小
- 全球已有 23 個司法管轄區宣布採納或參考 ISSB 準則
- 歐盟 EFRAG 已發布 CSRD-ISSB 互操作性指引，減少 60% 重複揭露
- 台灣金管會傾向以 ISSB 為基礎，但保留 CSRD 特定揭露要求
- 企業合規成本預估：同時符合兩者需額外投入 15-25% 的報告預算

### 正文

## 一、從分歧到對話：雙軌制的誕生背景

全球永續揭露準則在 2023-2025 年間經歷了前所未有的制度碰撞。
歐盟以 CSRD（Corporate Sustainability Reporting Directive）為核心，
要求企業從「對環境和社會的影響」（inside-out）角度揭露；
而 ISSB 的 IFRS S1/S2 則聚焦「對企業財務價值的影響」（outside-in）。

這兩套系統的哲學差異，曾一度被認為是不可調和的矛盾。
然而，2025 年 EFRAG 與 ISSB 聯合發布的互操作性對照表（Interoperability Mapping），
標誌著雙軌制開始走向實質整合。
（來源：EFRAG, CSRD-ISSB Interoperability Guidance, 2025）

...（後續約 2,000 字，含數據表格與多元觀點分析）
```

![W3 學術分析 CSRD vs ISSB](images/ch10_03_w3_csrd_issb.png)

---

### 步驟 10.4：W4 說故事風 📖

| 欄位 | 內容 |
|------|------|
| 代號 | W4 |
| 名稱 | 說故事風 |
| 圖示 | 📖 |
| 月產量 | 15 篇 |
| 核心風格 | 場景敘事、情感連結 |
| 字數範圍 | 1,000-1,800 字 |

**文章結構模板**

1. 場景開場（In medias res：直接進入場景）
2. 問題浮現（故事背後的 ESG 挑戰）
3. 關鍵人物或企業的選擇（面對挑戰如何決定）
4. 轉折點（改變發生了什麼）
5. 影響與意義（對更大的圖像意味著什麼）
6. 結尾（呼應開場，留下思考空間）

**禁止事項**：不從定義或背景知識開始、不讓數據淹沒故事、不說教或強加道德判斷、不使用「值得關注」「意義深遠」等空洞詞彙、不捏造細節

#### 實際輸出範例（CMP-114）

```
## W4 說故事風 — 台南漁民與氣候變遷

### 標題
當海水溫度升高 2 度 — 台南七股漁民老陳的永續抉擇

### 正文

2025 年夏天，台南七股。清晨五點，陳文雄站在漁塭邊，
手裡握著水溫計。37.2 度。他皺了皺眉。

「以前夏天頂多 34 度，現在動不動就 37、38。」
61 歲的老陳養了一輩子虱目魚，卻在這兩年第一次覺得，
魚可能比他更早退休。

去年夏天，一場連續 12 天超過 38 度的熱浪，
讓老陳的漁塭死了三分之一的虱目魚。損失超過 80 萬元。
不只是他 — 整個七股地區有超過 200 戶養殖業者受災，
總損失估計達 2.3 億元。

這不是單一事件，而是一個正在發生的氣候現實。
根據中央氣象署資料，台南沿海地區近 10 年平均氣溫上升 1.8 度，
遠高於全球平均的 1.1 度。對靠天吃飯的養殖漁民來說，
每上升一度，都是一次無聲的威脅。

## 轉折

2025 年底，台南市農業局引進了一套由工研院開發的「智慧養殖監控系統」。
這套系統用 AI 分析水溫、溶氧量、pH 值的即時數據，
在魚群面臨風險前 6 小時發出預警。

老陳是第一批志願參加試驗的漁民。

「說實在的，我一開始不信那個機器。」
他笑著說，「但第一次收到預警，我半信半疑把水換了。
結果那天隔壁塭的魚死了一大片，我的全活了。」

從那天起，老陳成了七股「智慧養殖」的最佳代言人。
現在，已有 47 戶漁民加入計畫，覆蓋面積超過 150 公頃。

...（後續約 600 字，以老陳回望漁塭的場景作結）
```

![W4 台南漁民永續故事](images/ch10_04_w4_fisherman_story.png)

---

### 步驟 10.5：W5 意見領袖風 💡

| 欄位 | 內容 |
|------|------|
| 代號 | W5 |
| 名稱 | 意見領袖風 |
| 圖示 | 💡 |
| 月產量 | 10 篇 |
| 核心風格 | 明確觀點、犀利評論 |
| 字數範圍 | 800-1,200 字 |

**文章結構模板**

1. 挑釁式開場（反直覺觀點或讓人不舒服的問題，2-3句）
2. 我的核心主張（清楚一段，讓讀者知道本文立場）
3. 主流觀點是什麼（公平呈現，不稻草人化）
4. 為什麼我認為主流觀點有問題（核心論辯，2-3個論點）
5. 我建議的替代觀點或做法
6. 結語（留下餘味，不是結論，而是一個問題或呼籲）

**評論類型**：漂綠批判型 / 政策評析型 / 趨勢預測型 / 迷思破解型

**禁止事項**：不寫沒有立場的兩面討好文章、不讓批評淪為謾罵、不使用未經查證的說法、不把評論文寫成新聞報導

#### 實際輸出範例（CMP-115）

```
## W5 意見領袖風 — ESG 基金漂綠評論

### 標題
台灣 ESG 基金的漂綠幻覺：當「永續」淪為行銷標籤

### 正文

台灣 ESG 基金越賣越好，但 ESG 表現越做越假。
這不是進步，這是進步的幻覺。

讓我把話說清楚：台灣目前超過 80 檔標榜「ESG」的基金中，
至少有三分之一不配用這三個字。它們的 ESG 篩選標準之鬆散，
就像一間自助餐宣稱自己是「米其林餐廳」— 菜色很多，但品質存疑。

## 主流怎麼說？

業界的說法很漂亮：「台灣 ESG 基金規模已突破 NT$5,000 億，
年增長率 35%，顯示投資人對永續投資的高度認同。」
金管會也以此為政績，宣稱台灣永續金融發展「走在亞洲前列」。

## 問題在哪？

第一，定義混亂。台灣目前沒有法定的「ESG 基金」認定標準。
只要名稱掛上「永續」「ESG」「低碳」，就能吸引資金。
歐盟有 SFDR（永續金融揭露規則）將基金分為 Article 6/8/9 三級，
台灣呢？什麼都沒有。

第二，持股打臉。攤開某些「ESG 基金」的持股明細，
你會發現前十大持股跟一般台股基金幾乎一模一樣 —
台積電、鴻海、聯發科。請問，用不同的名字裝同一碗飯，
這算永續還是算話術？

第三，「排除」太少。多數台灣 ESG 基金的負面篩選清單
只排除「爭議性武器」和「菸草」— 恭喜，
這排除了台灣上市公司的大約 0.3%。這種篩選力度，
跟沒篩差不多。

...（後續約 400 字，提出替代建議與結語）
```

![W5 ESG 基金漂綠評論](images/ch10_05_w5_greenwashing.png)

---

### 步驟 10.6：W6 數據新聞風 📈

| 欄位 | 內容 |
|------|------|
| 代號 | W6 |
| 名稱 | 數據新聞風 |
| 圖示 | 📈 |
| 月產量 | 5 篇 |
| 核心風格 | 數據驅動、圖表建議 |
| 字數範圍 | 1,000-1,500 字 |

**文章結構模板**

1. 導言（最驚人或最反直覺的數字當開場，1段）
2. 數據全貌（數據來源、樣本、調查方法）
3. 核心發現 1：關鍵數字 + 為什麼重要
4. 核心發現 2：趨勢比較（今年 vs 去年 / 台灣 vs 國際）
5. 核心發現 3：值得注意的異常值或例外
6. 數據的侷限與注意事項（必須包含，1段）
7. 結論（這些數字對哪類讀者意味著什麼行動）

**數字換算規則**
- 碳排放：換算成「幾輛汽車行駛一年」或「幾棵樹一年吸收量」
- 投資金額：換算成「等於幾棟台北 101」或「台灣幾個月 GDP」
- 能源：換算成「幾戶家庭一年用電量」

**禁止事項**：不用數字支持預設結論、不忽略數據來源可信度、不讓數字堆積而無解讀、不用未換算的生硬數字

#### 實際輸出範例（CMP-121）

```
## W6 數據新聞風 — ESG 揭露率數據解析

### 標題
台灣上市公司 ESG 揭露率全解析：從 23% 到 78% 的三年躍進，
還有多少企業在裝睡？

### 正文

78%。

這是 2025 年台灣上市公司出具永續報告書的比例。
三年前，這個數字是 23%。

這代表什麼？等於三年內有超過 500 家企業「從零開始」學會寫 ESG 報告。
如果把這些報告疊起來，大概跟台北 101 第 20 層一樣高。
如果把撰寫這些報告投入的人力換算，約等於每天有 2,000 人全職在做 ESG 文書工作。

[圖表建議：折線圖 — 2022-2025 台灣上市公司永續報告出具率（%） — X軸:年份 / Y軸:出具率]

## 核心發現 1：金融業領跑，傳產業追趕

按產業分析，揭露率差異顯著：

| 產業 | 2023 揭露率 | 2025 揭露率 | 成長幅度 |
|------|-----------|-----------|---------|
| 金融業 | 95% | 100% | +5% |
| 半導體 | 72% | 95% | +23% |
| 電子零組件 | 45% | 82% | +37% |
| 傳統製造 | 18% | 65% | +47% |
| 零售/服務 | 12% | 58% | +46% |

[圖表建議：堆疊柱狀圖 — 各產業永續報告出具率對比 2023 vs 2025]

## 核心發現 2：揭露率 ≠ 揭露品質

數量上去了，品質呢？根據台灣永續能源研究基金會的分析，
2025 年永續報告的「品質指數」平均僅 62 分（滿分 100），
比 2023 年的 58 分只進步了 4 分。

換句話說，有報告的企業多了，但報告寫得好的企業並沒有等比增加。
（來源：台灣永續能源研究基金會，2025 ESG 報告品質評鑑）

...（後續含核心發現 3、數據侷限說明與結論，約 500 字）
```

![W6 數據新聞 ESG 揭露率](images/ch10_06_w6_data_disclosure.png)

---

### 步驟 10.7：W7 簡潔速讀風 ⚡

| 欄位 | 內容 |
|------|------|
| 代號 | W7 |
| 名稱 | 簡潔速讀風 |
| 圖示 | ⚡ |
| 月產量 | 5 篇 |
| 核心風格 | 3分鐘可讀、條列精華 |
| 字數範圍 | 400-700 字（嚴格上限！） |

**文章結構模板**

1. **TL;DR**（一句話摘要，20字以內）
2. **你需要知道的 5 件事**（每點 2-3 句話，粗體關鍵字開頭）
3. **關鍵數字**（1-3 個最重要數據，各1句說明）
4. **你現在可以做什麼**（2-3 個具體行動）

**標題公式**
- 「【懶人包】[主題]：[X]件事一次看」
- 「【速懂】[X]分鐘讀懂[主題]」
- 「[主題]更新！[X]個重點你要知道」

**禁止事項**：不寫超過 700 字（紅線，不是建議）、不用長句（超過30字的句子一律拆開）、不省略行動指引、不做分析或評論

#### 實際輸出範例（CMP-116）

```
## W7 簡潔速讀風 — 碳權懶人包

### 【懶人包】碳權交易是什麼？5 件事讓你秒懂

**TL;DR**：碳權 = 排碳的許可證，可以買賣，台灣 2026 年正式開市。

---

### 你需要知道的 5 件事

1. **碳權就是「排碳額度」**：政府設定企業可排放的碳上限，
   多出來的要買，省下來的可以賣。簡單說，就是碳的股票市場。

2. **台灣碳權交易所已成立**：2023 年成立，2026 年 Q3 正式開放交易。
   初期只交易國內自願減量額度。

3. **誰需要買碳權？**年排碳量超過 2.5 萬噸的企業。
   台灣約有 287 家企業達到這個門檻，以鋼鐵、水泥、石化業為主。

4. **碳權價格差很大**：歐盟碳權目前約每噸 60 歐元，
   台灣預估初期在每噸 NT$200-500 之間。價格會隨供需變動。

5. **不是只有大企業的事**：供應鏈效應會傳導。
   大企業被課碳費，成本會轉嫁給上下游中小企業。

---

### 關鍵數字

- **287 家**：台灣受碳費影響的企業數量
- **NT$300/噸**：目前台灣碳費第一階段費率
- **2027 年**：碳費正式開徵年度

---

### 你現在可以做什麼

1. **企業主**：盡快完成組織碳盤查（ISO 14064-1）
2. **投資人**：關注碳權 ETF 和低碳概念股
3. **一般民眾**：了解你購買的產品碳足跡標籤
```

![W7 碳權懶人包](images/ch10_07_w7_carbon_credit.png)

---

### 步驟 10.8：Writing Agent 總覽對照表

| 項目 | W1 📰 | W2 🎓 | W3 🔬 | W4 📖 | W5 💡 | W6 📈 | W7 ⚡ |
|------|--------|--------|--------|--------|--------|--------|--------|
| 風格名稱 | 新聞記者風 | 知識型教師風 | 學術分析師風 | 說故事風 | 意見領袖風 | 數據新聞風 | 簡潔速讀風 |
| 月產量 | 30 篇 | 20 篇 | 15 篇 | 15 篇 | 10 篇 | 5 篇 | 5 篇 |
| 字數 | 800-1,200 | 1,000-1,500 | 1,500-2,500 | 1,000-1,800 | 800-1,200 | 1,000-1,500 | 400-700 |
| 結構核心 | 倒三角 | 步驟教學 | 論點分析 | 場景敘事 | 觀點論辯 | 數據驅動 | 條列精華 |
| 語氣 | 中立客觀 | 親切口語 | 嚴謹正式 | 文學感 | 自信直接 | 精確清晰 | 極簡乾脆 |
| 開場方式 | 最重要事實 | 讀者痛點 | 核心論點 | 場景畫面 | 挑釁觀點 | 驚人數字 | TL;DR |
| 適合素材 | 即時新聞 | 概念解說 | 政策議題 | 人物故事 | 爭議議題 | 數據報告 | 熱門話題 |
| 第一人稱 | 禁止 | 用「你」 | 不使用 | 不使用 | 適度使用 | 不使用 | 不使用 |
| SEO 規則 | 共用 9 條 | 共用 9 條 | 共用 9 條 | 共用 9 條 | 共用 9 條 | 共用 9 條 | 共用 9 條 |

> **備註**：7 位 Writing Agent 每月合計目標 100 篇。各 Agent 的月產量比例反映內容策略：新聞（30%）為主力，教學（20%）和深度/故事（各 15%）為核心，評論（10%）提供觀點，數據和速讀（各 5%）為特殊格式。

---

## Chapter 11：品質審查層 — Q1, Q2

品質審查層共 2 位獨立審查 Agent。每篇文章必須通過 Q1（內容品質）和 Q2（ESG 專業）雙重審查才能發布。這是 ESGTimes 確保商用品質的最後防線。

---

### 步驟 11.1：Q1 內容品質審查員 ✅

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | Q1 |
| 名稱 | 內容品質審查員 |
| 圖示 | ✅ |
| 角色 | 事實正確性、結構完整性、繁中品質、數據引用品質評分 |
| 輸出格式 | 品質評分表（4 維度 x 1-10 分 + 總分 + 結論 + 改善建議） |

#### 評分維度（各 1-10 分）

| 維度 | 評分重點 |
|------|----------|
| 事實正確性 | 引用數據有來源？法規名稱正確？數字合理（無數量級錯誤）？ |
| 結構完整性 | 遵循該風格的結構模板？段落邏輯通順？有開頭、主體、結尾？ |
| 繁體中文品質 | 道地繁體中文（非簡轉繁）？台灣用語習慣？有無錯字語法錯誤？ |
| 數據引用品質 | 每個數據點標明來源？來源為可信機構？數據年份明確？ |

#### 評分表格式（Agent 必須使用此格式輸出）

```
【品質評分】
事實正確性：X/10
結構完整性：X/10
繁中品質：X/10
數據引用：X/10
總分：X/10
結論：✅ 審核通過 / ❌ 退回修改
改善建議：
1. [具體改善方向]
2. [具體改善方向]
3. [具體改善方向]
```

#### 通過/退回機制

| 條件 | 結論 | 後續動作 |
|------|------|----------|
| 總分 ≥ 7 | ✅ 審核通過 | Issue 狀態保持 `done` |
| 總分 < 7 | ❌ 退回修改 | Issue 狀態改為 `in_progress`，comment 附 ≥ 3 點改善建議 |

#### 實際輸出範例（CMP-117）

以下為 CMP-117 測試中 Q1 對一篇碳費文章的評分結果：

```
## Q1 內容品質審查 — [CMP-201] 碳費第二階段徵收制度解析

【品質評分】
事實正確性：7/10
結構完整性：8/10
繁中品質：9/10
數據引用：6/10
總分：7.5/10
結論：✅ 審核通過

改善建議：
1. 碳費費率引用建議補充環境部公告文號，目前僅引述「環境部公告」，
   缺乏具體文號讓讀者可自行查證
2. 第三段提及「約 500 家企業受影響」，建議註明此數據來源
   （環境部統計或工業局統計？）以及統計基準年
3. 文章結尾的「後續影響」段落稍短，建議補充至少一個企業界的回應引述，
   增加報導的平衡性
4. 部分數據（如「碳費收入預估 NT$200 億」）缺乏年份標示，
   建議標明「預估年度」避免讀者混淆

備註：整體品質達到發布標準。繁中品質優秀，用詞自然不生硬。
結構完整，倒三角架構清晰。主要改善空間在數據引用的嚴謹度。
```

> **備註**：此篇文章總分 7.5/10，通過 Q1 審查。雖然通過，但 Q1 仍給出 4 點改善建議供寫手參考。這些建議不影響發布，但有助於提升後續文章品質。

![Q1 品質評分結果](images/ch11_01_q1_quality_score.png)

---

### 步驟 11.2：Q2 ESG 專業顧問 🎯

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | Q2 |
| 名稱 | ESG 專業顧問 |
| 圖示 | 🎯 |
| 角色 | 術語正確性、法規引用、國際標準合規、觀點平衡性評分 |
| 輸出格式 | ESG 專業評分表（4 維度 x 1-10 分 + 總分 + 結論 + 專業建議） |

#### 評分維度（各 1-10 分）

| 維度 | 評分重點 |
|------|----------|
| ESG 術語使用正確性 | 專業術語正確使用？縮寫首次出現時展開？翻譯符合台灣業界慣用？ |
| 法規引用準確性 | 法規名稱、編號正確？適用範圍描述準確？時程/截止日期正確？ |
| 國際標準合規性 | 符合 GRI/ISSB/TCFD 最新版本？無過時標準引用？ESG 框架描述準確？ |
| 利害關係人觀點平衡性 | 非單一觀點？考慮不同利害關係人立場？爭議議題公平呈現？ |

#### 評分表格式（Agent 必須使用此格式輸出）

```
【ESG 專業評分】
術語正確性：X/10
法規引用：X/10
國際標準合規：X/10
觀點平衡性：X/10
總分：X/10
結論：✅ 審核通過 / ❌ 退回修改
專業建議：
1. [具體專業改善方向]
2. [具體專業改善方向]
```

#### 通過/退回機制

| 條件 | 結論 | 後續動作 |
|------|------|----------|
| 總分 ≥ 7 | ✅ ESG 專業審核通過 | Issue 狀態保持 `done` |
| 總分 < 7 | ❌ 退回修改（ESG 專業） | Issue 狀態改為 `in_progress`，附 ≥ 2 點專業問題 + 正確用法 |

#### 實際輸出範例（CMP-124）

以下為 CMP-124 測試中 Q2 對一篇 CBAM 報導的 ESG 專業評分：

```
## Q2 ESG 專業審查 — [CMP-203] CBAM 碳邊境機制深度分析

【ESG 專業評分】
術語正確性：8/10
法規引用：9/10
國際標準合規：8/10
觀點平衡性：7/10
總分：8.0/10
結論：✅ ESG 專業審核通過

專業建議：
1. 文中使用「碳邊境稅」一詞，建議統一使用「碳邊境調整機制」
   （Carbon Border Adjustment Mechanism, CBAM）。CBAM 在法律定義上
   不是「稅」而是「調整機制」，用「稅」會引起法律解釋上的誤導。
   台灣官方（經濟部、環境部）均使用「碳邊境調整機制」。

2. 文中提及 CBAM 涵蓋「六大產業」，正確但建議完整列出：
   鋼鐵、鋁、水泥、化肥、電力、氫。目前文中只提到前三項。

3. 關於歐盟免費配額逐步退場時程，文中引述「2026-2034 年」，
   最新修訂為「2026-2034 年線性遞減」，建議補充每年遞減比例
  （約 10%/年）以增加精確度。

4. 觀點平衡性建議：目前文章偏重「台灣企業成本增加」的角度，
   建議補充「碳邊境機制可能推動台灣加速碳定價」的正面論述，
   以及歐盟設計 CBAM 的初衷（防止碳洩漏），讓分析更完整。

備註：整體 ESG 專業度高，法規引用準確。術語使用小瑕疵不影響
整體品質，但建議後續文章統一術語規範。
```

![Q2 ESG 專業評分結果](images/ch11_02_q2_esg_score.png)

---

### 步驟 11.3：雙重審查流程

每篇文章必須通過 Q1（內容品質）和 Q2（ESG 專業）雙重審查才能進入發布流程。兩個審查 Agent 互相獨立，評分不互相影響。

**流程圖**

```
Writing Agent 完成文章
        │
        ▼
   ┌──────────┐
   │ Issue 設為 │
   │   done    │
   └──────────┘
        │
   ┌────┴────┐
   ▼         ▼
┌──────┐  ┌──────┐
│ Q1   │  │ Q2   │
│品質審查│  │專業審查│
└──────┘  └──────┘
   │         │
   ▼         ▼
 ≥ 7?      ≥ 7?
   │         │
┌──┴──┐  ┌──┴──┐
│     │  │     │
Yes   No Yes   No
│     │  │     │
└──┬──┘  └──┬──┘
   │         │
   ▼         ▼
┌──────────────────┐
│ 兩者都 ≥ 7 ？      │
│                    │
│ ✅ 是 → 審核通過    │
│   → M3 排程發布     │
│   → S1 同步 Odoo   │
│                    │
│ ❌ 否 → 退回修改    │
│   → Issue → in_progress │
│   → 原寫手重新修改   │
│   → 修改後重新送審   │
└──────────────────┘
```

> **重要規則**：
> - Q1 和 Q2 的審查是**獨立且平行**的，兩者都通過才算通過
> - 任一個低於 7 分，文章即退回修改
> - 退回時 Issue 狀態改為 `in_progress`，觸發原寫手重新修改
> - 修改後的文章需重新接受 Q1 和 Q2 的完整審查（不能只審改過的部分）
> - Q1 不評論寫作風格（那是各 Agent 的特色），Q2 不評論文學品質

![雙重審查流程圖](images/ch11_03_dual_review_flow.png)

---

## Chapter 12：同步層 — S1 Odoo 同步

同步層只有 1 位 Agent（S1 Odoo 同步助手），負責將 Paperclip 中已完成的 Issue 自動同步到 Odoo 18 專案管理系統，建立完整的內容產出紀錄。

---

### 步驟 12.1：S1 Odoo 同步助手

#### Agent 資訊

| 欄位 | 內容 |
|------|------|
| 代號 | S1 |
| 名稱 | Odoo 同步助手 |
| 圖示 | 🔄 |
| 角色 | 監控 done issues → 同步到 Odoo 專案 |
| 同步腳本 | `sync-paperclip-to-odoo.py` |

#### 同步機制

S1 的核心工作流程：

1. 查詢 Paperclip DB 中所有 `done` 狀態的 Issue
2. 檢查 Odoo 中是否已存在對應的 Task（防重複）
3. 建立新的 Odoo Task，包含結構化的 Issue 描述
4. 將 Issue 的 Comment（Agent 回覆內容）寫入 Task 的 Chatter（聊天評論區）
5. 自動偵測標題關鍵字，分配對應的 Tag

#### 同步內容對照

| Paperclip 來源 | Odoo 目標 | 說明 |
|---------------|-----------|------|
| Issue identifier + title | Task 名稱 | 格式：`[CMP-XX] 標題` |
| Issue description | Task 描述 | HTML 格式，含 Issue 概述 |
| Issue comments | Chatter messages | Agent 的實際回覆內容 |
| Issue status | Task stage | done → ✅ 已完成；todo → 📋 規劃中 |
| Title keywords | Task tags | 自動偵測分配 |

#### 防重複機制

同步腳本透過 Task 名稱進行防重複檢查：

```python
# 檢查 Odoo 中是否已存在同名 Task
task_name = f"[{identifier}] {title}"
if task_name in existing_names:
    skipped += 1
    continue  # 跳過已存在的 Task
```

> **備註**：防重複機制確保多次執行同步不會產生重複 Task。已存在的 Task 會被跳過，只建立新的 Task。

---

### 步驟 12.2：Odoo 專案結構

同步目標為 Odoo 18 中的 `Paperclip AI - ESGTimes` 專案。

#### 專案基本資訊

| 欄位 | 內容 |
|------|------|
| 專案名稱 | Paperclip AI - ESGTimes |
| 專案描述 | ESGTimes 撰寫文章自動化 -- Paperclip AI agent 產出的 ESG 媒體內容與企業管理文件記錄 |
| Odoo URL | `https://cindytech1-odoo.woowtech.io` |
| 資料庫 | cindytech |

#### Stages（任務階段）

| 順序 | 階段名稱 | 說明 |
|------|----------|------|
| 1 | 📋 規劃中 | 新建立的 Task 預設階段 |
| 2 | 🚀 執行中 | Agent 正在處理中的 Task |
| 3 | 🔍 審核中 | Q1/Q2 審查中的 Task |
| 4 | ✅ 已完成 | 通過審查、已同步的 Task |
| 5 | 🗑️ 已廢棄 | 已取消的 Task（折疊顯示） |

#### Tags（自動標籤）

同步腳本會根據 Task 標題的關鍵字自動分配標籤：

| 標籤 | 顏色 | 偵測關鍵字 |
|------|------|-----------|
| 📰 深度報導 | 1 | 報導、分析文、專題、深度、趨勢 |
| 📱 社群內容 | 2 | LinkedIn、Instagram、Reels、貼文、社群 |
| 📧 電子報 | 3 | 電子報、Newsletter |
| 🎙️ Podcast | 4 | Podcast、節目 |
| 🌐 翻譯/國際 | 5 | 翻譯、ISSB、GRI、CSRD、SEC、日本、韓國、東南亞 |
| 📊 數據分析 | 6 | 數據、排名、排行、比較、Top、評級 |
| 💼 策略規劃 | 7 | 營運、OKR、KPI、策略、計畫、預算 |
| 🤝 商業開發 | 8 | 提案、合作、培訓、課程、收購、COP |
| ⚖️ 法規政策 | 9 | 法規、碳費、碳盤查、TCFD、懶人包、政策 |
| 📝 範本/工具 | 10 | SOP、範本、合約、指南、風格、檢核 |
| 🎬 影音內容 | 11 | YouTube、影片、腳本、圖文 |

**自動標籤偵測邏輯**：

```python
def detect_tags(title, tag_map):
    """Auto-detect tags based on title keywords."""
    matched = []
    for rule in TAG_RULES:
        for kw in rule["keywords"]:
            if kw.lower() in title.lower():
                if rule["name"] in tag_map:
                    matched.append(tag_map[rule["name"]])
                break
    return matched
```

> **備註**：一個 Task 可以被分配多個標籤。例如，標題「撰寫 ISSB 準則深度報導」會同時匹配「📰 深度報導」和「🌐 翻譯/國際」兩個標籤。

---

### 步驟 12.3：Task 描述格式

每個同步到 Odoo 的 Task 描述使用以下 HTML 格式：

```html
<h3>📋 Issue 概述</h3>
<ul>
<li><b>Issue ID：</b>CMP-108</li>
<li><b>標題：</b>制定 ESGTimes 2026 年 6 月內容策略</li>
<li><b>負責 Agent：</b>🤖 總編輯長</li>
<li><b>來源狀態：</b>✅ 已完成</li>
<li><b>來源平台：</b>Paperclip AI (CMP)</li>
</ul>
<h3>📄 任務說明</h3>
<p>根據 ESGTimes 編輯方針，制定 2026 年 6 月的內容策略方向...</p>
```

#### Chatter 訊息格式

每則 Agent 回覆的 Comment 以以下格式寫入 Odoo 的 Chatter：

```html
<p><b>🤖 總編輯長</b> · 2026-05-28 14:30 · 回覆 1/3</p>
<p>## ESGTimes 2026 年 6 月內容策略</p>
<p>### 本月重點議題清單</p>
<p>...</p>
```

> **備註**：系統自動過濾長度小於 50 字元的 Comment 以及系統重試訊息（如 "automatically retried"），確保 Chatter 中只保留有意義的 Agent 回覆內容。

---

### 步驟 12.4：手動同步步驟

若需手動觸發同步，執行以下指令：

```bash
cd "/var/tmp/vibe-kanban/worktrees/0957-openclaw-v2026-4/k3s project"
python3 sync-paperclip-to-odoo.py
```

#### 預期輸出

```
============================================================
  Paperclip → Odoo Sync v2
============================================================

[1] Connecting to Odoo...
  Connected (uid=2)

[2] Creating project...
  Exists (id=15)

[3] Setting up stages...
  Stages: ['📋 規劃中', '🚀 執行中', '🔍 審核中', '✅ 已完成', '🗑️ 已廢棄']

[4] Setting up tags...
  Tags: 11

[5] Reading Paperclip issues...
  Found 47 issues

[6] Syncing...
  ... 10 tasks, 23 comments
  ... 20 tasks, 51 comments
  ... 30 tasks, 78 comments

============================================================
  ✅ SYNC COMPLETE
  Tasks created: 35
  Tasks skipped: 12
  Comments synced: 94
  Stages: 5
  Tags: 11
============================================================
```

**輸出欄位說明**

| 欄位 | 說明 |
|------|------|
| Tasks created | 本次新建立的 Odoo Task 數量 |
| Tasks skipped | 已存在而跳過的 Task 數量（防重複機制） |
| Comments synced | 成功寫入 Chatter 的 Comment 數量 |
| Stages | 專案中的階段數量 |
| Tags | 可用的標籤數量 |

#### 同步腳本執行前提

| 前提條件 | 說明 |
|----------|------|
| Kubernetes 存取 | 需能執行 `kubectl` 指令（讀取 Paperclip DB） |
| Paperclip DB Pod | `app=paperclip-db` label 在 `openclaw-tenant-1` namespace |
| Odoo 可達 | `https://cindytech1-odoo.woowtech.io` 可連線 |
| Odoo 帳號 | admin/admin，資料庫 cindytech |

> **備註**：同步腳本使用 Odoo XML-RPC API（`/xmlrpc/2/common` 和 `/xmlrpc/2/object`）進行認證與資料操作。所有 Chatter 訊息透過 `mail.message` model 的 `create` 方法寫入，確保在 Odoo 的聊天介面正確顯示。

![Odoo 專案同步結果](images/ch12_01_odoo_sync_result.png)

---

## 附錄：18 位 Agent 完整總覽

| 層級 | 代號 | 名稱 | 圖示 | 核心職責 |
|------|------|------|------|----------|
| 管理層 | M1 | 總編輯長 | 👑 | 制定內容策略、審核重大議題方向 |
| 管理層 | M2 | 內容主編 | 📋 | 日常排程、指派任務給寫手 |
| 管理層 | M3 | 營運主編 | ⚙️ | 電子報排程、社群排程、Odoo 同步觸發 |
| 研究層 | R1 | 政策法規研究員 | 📜 | 監控 ISSB/GRI/TCFD/CSRD/SEC + 台灣法規 |
| 研究層 | R2 | 企業案例研究員 | 🏢 | 監控企業 ESG 實踐案例 |
| 研究層 | R3 | 數據趨勢研究員 | 📊 | 監控 ESG 數據報告與趨勢 |
| 研究層 | R4 | 國際媒體掃描員 | 🌍 | 篩選國際 ESG 新聞 |
| 研究層 | R5 | 台灣在地線人 | 🇹🇼 | 追蹤台灣本地 ESG 動態 |
| 寫作層 | W1 | 新聞記者風 | 📰 | 倒三角結構、中立客觀（30篇/月） |
| 寫作層 | W2 | 知識型教師風 | 🎓 | 親切教學、比喻類比（20篇/月） |
| 寫作層 | W3 | 學術分析師風 | 🔬 | 深度論據、多元角度（15篇/月） |
| 寫作層 | W4 | 說故事風 | 📖 | 場景敘事、情感連結（15篇/月） |
| 寫作層 | W5 | 意見領袖風 | 💡 | 明確觀點、犀利評論（10篇/月） |
| 寫作層 | W6 | 數據新聞風 | 📈 | 數據驅動、圖表建議（5篇/月） |
| 寫作層 | W7 | 簡潔速讀風 | ⚡ | 3分鐘可讀、條列精華（5篇/月） |
| 品質層 | Q1 | 內容品質審查員 | ✅ | 事實/結構/繁中/數據引用 4 維度評分 |
| 品質層 | Q2 | ESG 專業顧問 | 🎯 | 術語/法規/標準/觀點平衡 4 維度評分 |
| 同步層 | S1 | Odoo 同步助手 | 🔄 | done issues → Odoo Task + Chatter |
# Part 4：端到端工作流程教學

## Chapter 13：端到端工作流程教學

本章節將透過完整的工作流程範例，展示 18 位 AI Agent 如何協同運作，從研究、撰寫、審查到同步發布，完成一篇高品質 ESG 新聞的全生命週期。

---

### 工作流程一：政策法規新聞（R1 → W1 → Q1 + Q2 → S1）

此工作流程為最常見的端到端流程，涵蓋政策法規研究、新聞撰寫、雙重品質審查及 Odoo 同步發布。

#### 步驟 1：建立研究任務

在 Paperclip 專案面板中建立新的 Issue，指派給 R1 政策法規研究員。

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 研究 2026 Q2 ESG 法規動態 |
| 指派 Agent | R1 政策法規研究員 |
| 描述 | 請研究 2026 年第二季度全球主要 ESG 相關法規動態，涵蓋歐盟 CBAM、亞太碳定價機制、台灣碳費政策等 |
| 優先順序 | High |

> **注意：** 研究任務的描述越具體，R1 產出的研究素材品質越高。建議明確列出關注的法規領域與地區範圍。

![建立研究任務並指派給 R1 政策法規研究員](images/ch13_01_create_research_issue.png)

#### 步驟 2：查看研究成果

R1 政策法規研究員完成任務後，會在 Issue Comment 中輸出結構化研究素材，格式如下：

| 欄位 | 說明 |
|------|------|
| 法規名稱 | 該法規的正式名稱（如：CBAM 碳邊境調整機制） |
| 來源 | 法規出處或資料來源連結 |
| 重要程度 | 高 / 中 / 低 |
| 核心摘要 | 200-300 字的法規核心內容摘要 |
| 建議寫作角度 | R1 根據法規內容建議的新聞切入角度 |

> **提示：** R1 的研究素材中「建議寫作角度」欄位特別重要，這將作為 W1 撰寫文章時的方向指引。若研究素材中包含多個法規，每個法規都會獨立呈現上述結構。

![R1 在 Issue Comment 中輸出結構化研究素材](images/ch13_02_r1_research_output.png)

#### 步驟 3：建立寫作任務

根據 R1 的研究成果，建立新的寫作 Issue，將研究素材貼入描述欄位，指派給 W1 新聞記者風寫手。

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 撰寫 CBAM 碳邊境調整機制新聞報導 |
| 指派 Agent | W1 新聞記者風寫手 |
| 描述 | 將 R1 的完整研究素材貼入此處 |
| 優先順序 | High |

> **重點：** 建立寫作任務時，請完整複製 R1 的研究素材到描述欄位，不要僅複製部分內容。W1 需要完整的研究素材才能產出高品質文章。

![建立寫作 Issue 並指派給 W1 新聞記者風寫手](images/ch13_03_create_writing_issue.png)

#### 步驟 4：查看文章成果

W1 新聞記者風寫手完成任務後，會在 Issue Comment 中產出完整的新聞報導，包含以下內容：

| 產出項目 | 規格 |
|---------|------|
| 文章本文 | 800-1200 字，新聞報導格式 |
| 標題方案 | 3 個候選標題供選擇 |
| Meta Description | SEO 用中繼描述（150 字以內） |
| 關鍵字建議 | 3-5 個 SEO 關鍵字 |

> **注意：** W1 產出的文章風格為客觀、專業的新聞報導體裁，適合作為 ESGTimes 的頭版新聞或焦點報導使用。

![W1 產出 800-1200 字新聞報導，含 3 個標題與 Meta Description](images/ch13_04_w1_article_output.png)

#### 步驟 5：品質審查（Q1）

建立品質審查 Issue，將 W1 的文章貼入描述，指派給 Q1 內容品質審查員。

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 審查 CBAM 碳邊境調整機制新聞報導品質 |
| 指派 Agent | Q1 內容品質審查員 |
| 描述 | 將 W1 的完整文章產出貼入此處 |

Q1 會在 Comment 中輸出評分表：

| 審查維度 | 評分範圍 | 說明 |
|---------|---------|------|
| 事實正確性 | 1-10 | 文中引用的事實、數據是否正確 |
| 結構完整性 | 1-10 | 文章結構是否完整（標題/導言/本文/結論） |
| 繁中品質 | 1-10 | 是否使用正確繁體中文，無簡體字或不當用語 |
| 數據引用 | 1-10 | 數據來源是否明確標註 |
| **總評** | **通過 / 退回修改** | **所有維度均 ≥ 7 分即通過** |

> **判定標準：** 四個維度的評分都必須達到 7 分（含）以上，文章才會被判定為「通過」。任一維度低於 7 分，Q1 將標記為「退回修改」並附上具體改善建議。

![建立品質審查 Issue 並指派給 Q1](images/ch13_05_create_qa_issue.png)

![Q1 輸出品質評分表與審查意見](images/ch13_06_q1_scoring_output.png)

#### 步驟 6：ESG 專業審查（Q2）

同時建立 ESG 專業審查 Issue，將相同文章指派給 Q2 ESG 專業顧問。

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | ESG 專業審查 CBAM 碳邊境調整機制報導 |
| 指派 Agent | Q2 ESG 專業顧問 |
| 描述 | 將 W1 的完整文章產出貼入此處（與 Q1 相同內容） |

Q2 會在 Comment 中輸出 ESG 專業評分表：

| 審查維度 | 評分範圍 | 說明 |
|---------|---------|------|
| ESG 框架準確度 | 1-10 | 是否正確使用 ESG 相關框架與術語 |
| 產業影響分析 | 1-10 | 對相關產業影響的分析是否到位 |
| 法規解讀正確性 | 1-10 | 法規條文解讀是否正確無誤 |
| 利害關係人觀點 | 1-10 | 是否涵蓋多方利害關係人觀點 |
| **總評** | **通過 / 退回修改** | **所有維度均 ≥ 7 分即通過** |

> **提示：** Q1 與 Q2 的審查可以同時進行（平行審查），不需等待 Q1 完成後才啟動 Q2。建議兩個審查 Issue 間隔 10-15 秒建立，避免 Gateway 過載。

![Q2 輸出 ESG 專業評分表與審查意見](images/ch13_07_q2_scoring_output.png)

#### 步驟 7：同步到 Odoo

當 Q1 和 Q2 雙重審查均通過（所有維度 ≥ 7 分）後，執行 Odoo 同步腳本將文章同步到 Odoo 專案管理系統。

```bash
python3 sync-paperclip-to-odoo.py
```

預期輸出：

```
[2026-05-29 10:30:01] Starting Paperclip → Odoo sync...
[2026-05-29 10:30:02] Fetching issues from Paperclip...
[2026-05-29 10:30:03] Found 3 new issues to sync
[2026-05-29 10:30:04] Syncing: CMP-0042 撰寫 CBAM 碳邊境調整機制新聞報導 → Done
[2026-05-29 10:30:05] Sync completed: 3 synced, 0 failed
```

同步完成後，在 Odoo 專案 Kanban 面板中檢查文章是否正確出現。

> **重要：** 只有通過 Q1 和 Q2 雙重審查的文章才應該同步到 Odoo。未通過審查的文章需要退回修改後重新審查。

![執行 sync-paperclip-to-odoo.py 同步腳本](images/ch13_08_run_sync_script.png)

![在 Odoo 專案 Kanban 中確認同步結果](images/ch13_09_odoo_kanban_result.png)

---

### 工作流程二：企業故事報導（R2 → W4 → Q1 + Q2 → S1）

此工作流程專注於企業 ESG 實踐案例的故事化報導，由 R2 企業案例研究員研究素材，交由 W4 故事型寫手撰寫。

#### 步驟 1：建立企業案例研究任務

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 研究台灣半導體業 ESG 轉型案例 |
| 指派 Agent | R2 企業案例研究員 |
| 描述 | 請研究台灣主要半導體企業在 ESG 轉型方面的最新實踐案例 |

#### 步驟 2：查看 R2 研究成果

R2 的研究素材與 R1 不同，會特別包含「故事潛力評分」：

| 欄位 | 說明 |
|------|------|
| 企業名稱 | 案例企業名稱 |
| ESG 實踐項目 | 具體的 ESG 實踐內容 |
| 故事潛力評分 | 1-10 分，評估此案例是否適合故事化報導 |
| 關鍵人物 | 可引用的關鍵人物與角色 |
| 衝突/轉折點 | 故事中的關鍵轉折或挑戰 |
| 建議敘事結構 | R2 建議的故事敘事架構 |

> **重點：** R2 的「故事潛力評分」是決定是否進入 W4 撰寫階段的關鍵指標。建議僅選擇評分 ≥ 7 的案例交給 W4 撰寫，以確保最終產出的故事品質。

#### 步驟 3：建立故事撰寫任務

將 R2 產出的高故事潛力素材交給 W4 故事型寫手：

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 撰寫台積電綠色製造轉型故事報導 |
| 指派 Agent | W4 故事型寫手 |
| 描述 | 貼入 R2 完整研究素材 |

W4 會產出 1000-1500 字的故事型報導，包含人物刻畫、場景描述與情感連結。

#### 步驟 4-6：品質審查與同步

後續流程與工作流程一相同：Q1 內容品質審查 → Q2 ESG 專業審查 → S1 Odoo 同步。

---

### 工作流程三：數據新聞（R3 → W6 → Q1 + Q2 → S1）

此工作流程專注於數據驅動的 ESG 報導，由 R3 數據報告研究員蒐集數據，交由 W6 數據新聞寫手撰寫。

#### 步驟 1：建立數據研究任務

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 研究 2025 年全球碳排放數據報告 |
| 指派 Agent | R3 數據報告研究員 |
| 描述 | 請蒐集並分析 2025 年全球主要碳排放數據報告，包含排放量趨勢、產業分布、國家排名等 |

#### 步驟 2：查看 R3 研究成果

R3 的產出著重於結構化數據與視覺化建議：

| 欄位 | 說明 |
|------|------|
| 數據來源 | 報告名稱與發布機構 |
| 關鍵數據點 | 結構化的核心數據列表 |
| 趨勢分析 | 數據趨勢與年度比較 |
| 圖表建議 | 建議使用的圖表類型與數據維度 |
| 數據可信度 | 1-10 分，評估數據來源的可信度 |

#### 步驟 3：建立數據新聞撰寫任務

| 欄位 | 填寫內容 |
|------|---------|
| 標題 | 撰寫全球碳排放趨勢數據新聞 |
| 指派 Agent | W6 數據新聞寫手 |
| 描述 | 貼入 R3 完整數據研究素材 |

W6 會產出數據新聞報導，特色包含：

| 產出項目 | 說明 |
|---------|------|
| 文章本文 | 800-1200 字數據新聞 |
| 圖表建議 | 具體的圖表設計建議（含數據軸、圖表類型） |
| 數據視覺化描述 | 文字描述建議的數據視覺化呈現方式 |
| 數據來源附註 | 完整的數據來源引用列表 |

#### 步驟 4-6：品質審查與同步

後續流程同樣為 Q1 + Q2 雙重審查，通過後執行 S1 Odoo 同步。

---

### 退回場景：文章被 QA 退回怎麼辦

當 Q1 或 Q2 審查未通過時，需要執行退回修改流程。

#### 步驟 1：確認退回原因

Q1 或 Q2 評分中任一維度 < 7 分時，Agent 會在 Comment 中標記：

```
❌ 退回修改

未通過維度：
- 繁中品質：5/10（發現 3 處簡體字用語）
- 數據引用：6/10（缺少 2 處數據來源）

改善建議：
1. 將「信息」改為「資訊」、「數據」確認用法
2. 補充 IEA 報告與 UNFCCC 數據來源
```

#### 步驟 2：Issue 狀態變更

文章被退回後，原寫作 Issue 的狀態會被設定為 `in_progress`，表示需要修改。

#### 步驟 3：修改任務描述

根據 Q1/Q2 的改善建議，在原寫作 Issue 的描述中補充修改要求：

```
【修改要求】
根據 QA 審查意見，請修正以下項目：
1. 修正繁中品質問題：將「信息」改為「資訊」...
2. 補充數據來源引用：IEA World Energy Outlook 2025...
```

#### 步驟 4：重新指派給原 Writing Agent

將修改後的 Issue 重新指派給原 Writing Agent（例如 W1），或根據需要手動修改文章內容。

> **注意：** 重新指派前，請確認該 Writing Agent 的狀態為 `idle`。若 Agent 正在處理其他任務，請等待完成後再指派。

#### 步驟 5：Writing Agent 修訂後重新提交

Writing Agent 會根據修改要求重新產出文章，新版本會在 Issue Comment 中更新。

#### 步驟 6：再次指派 Q1/Q2 審查

建立新的審查 Issue，將修訂後的文章重新指派給 Q1 和 Q2 進行第二輪審查。重複此流程直到所有維度均 ≥ 7 分通過。

> **建議：** 若文章經過兩次退回仍無法通過，建議人工介入檢視 Agent 的 System Prompt 設定是否需要調整，或由編輯人員直接修改文章。

---

### 管理層協作流程

管理層 Agent（M1、M2、M3）負責統籌整個編輯團隊的運作方向與節奏。

#### 步驟 1：M1 制定月度主題方向

M1 內容策略總監根據當月 ESG 趨勢，制定月度內容主題方向：

| 月度主題欄位 | 範例 |
|-------------|------|
| 月份 | 2026 年 6 月 |
| 核心主題 | 亞太碳定價機制全面啟動 |
| 子主題 1 | 台灣碳費開徵首季成效 |
| 子主題 2 | 日韓碳交易市場比較 |
| 子主題 3 | 東南亞碳信用市場崛起 |
| 預計產出文章數 | 12-15 篇 |

#### 步驟 2：M2 根據主題分配素材給 Writing Agents

M2 編輯台主任根據 M1 制定的主題方向，將 Research Agent 產出的素材分配給合適的 Writing Agent：

| 素材主題 | 分配給 | 原因 |
|---------|--------|------|
| 台灣碳費政策分析 | W1 新聞記者風寫手 | 政策新聞適合新聞報導體裁 |
| 台達電碳中和實踐 | W4 故事型寫手 | 企業案例適合故事化報導 |
| 亞太碳價趨勢數據 | W6 數據新聞寫手 | 數據豐富適合數據新聞 |
| CBAM 對出口業影響 | W2 深度分析寫手 | 需要深入分析的議題 |

#### 步驟 3：M3 安排發布排程

文章通過 Q1/Q2 雙重審查後，M3 發布排程管理員安排發布時間與順序：

| 發布日期 | 文章標題 | 類型 | 狀態 |
|---------|---------|------|------|
| 6/2（一） | 台灣碳費開徵首季成效報告 | 頭版新聞 | 待發布 |
| 6/3（二） | 台達電的碳中和之路 | 企業故事 | 待發布 |
| 6/4（三） | 亞太碳價走勢全解析 | 數據新聞 | 審查中 |
| 6/5（四） | CBAM 對台灣出口業的衝擊 | 深度分析 | 撰寫中 |

> **完整協作流程：** M1 定方向 → Research Agents 研究素材 → M2 分配素材給 Writing Agents → Writing Agents 撰寫文章 → Q1 + Q2 雙重審查 → M3 安排發布排程 → S1 同步到 Odoo。此流程確保從策略規劃到最終發布，每個環節都有對應的 Agent 負責把關。

---

# Part 5：系統管理與問題排除

## Chapter 14：系統管理

本章節涵蓋 Paperclip AI ESGTimes 系統的日常管理操作，包含 Pod 狀態監控、Agent 管理、日誌查看及各種維護任務。

---

### 步驟 1：查看 Pod 狀態

使用 `kubectl` 查看所有相關 Pod 的運行狀態：

```bash
kubectl get pods -n openclaw-tenant-1
```

預期輸出：

| NAME | READY | STATUS | RESTARTS | AGE |
|------|-------|--------|----------|-----|
| openclaw-gateway-6b879dcd6b-m65pm | 2/2 | Running | 0 | 7d |
| paperclip-db-99c454b69-48s4p | 1/1 | Running | 0 | 7d |
| paperclip-web-5d8f4b7c9a-x2k1p | 1/1 | Running | 0 | 7d |
| paperclip-api-7f6e3a1b2c-n4m5q | 1/1 | Running | 0 | 7d |

> **重點檢查：** OpenClaw Gateway 必須顯示 `2/2 Ready`（包含 gateway 容器與 sidecar）。若顯示 `1/2` 或 `0/2`，表示 Gateway 未完全啟動，Agent 將無法正常執行任務。

![kubectl get pods 查看 Pod 運行狀態](images/ch14_01_kubectl_get_pods.png)

---

### 步驟 2：查看 Agent 狀態（DB 查詢）

透過資料庫查詢所有 Agent 的當前狀態：

```bash
kubectl exec paperclip-db-99c454b69-48s4p -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip -t -A -c \
  "SELECT name, status, icon FROM agents WHERE company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb' AND status NOT IN ('archived') ORDER BY name;"
```

預期輸出範例：

| name | status | icon |
|------|--------|------|
| M1 內容策略總監 | idle | strategy |
| M2 編輯台主任 | idle | editor |
| M3 發布排程管理員 | idle | calendar |
| Q1 內容品質審查員 | idle | quality |
| Q2 ESG 專業顧問 | idle | expert |
| R1 政策法規研究員 | running | research |
| R2 企業案例研究員 | idle | case |
| R3 數據報告研究員 | idle | data |
| S1 Odoo同步代理 | idle | sync |
| W1 新聞記者風寫手 | idle | news |
| ... | ... | ... |

> **正常狀態：** 大部分 Agent 應顯示 `idle`。若有 Agent 顯示 `running`，表示正在執行任務。若顯示 `error`，表示上次執行發生錯誤，需要手動重設。

![查詢 Agent 狀態的資料庫輸出結果](images/ch14_02_agent_status_query.png)

---

### 步驟 3：查看 Gateway 日誌

查看 OpenClaw Gateway 的最近日誌，以排查連線問題：

```bash
kubectl logs openclaw-gateway-6b879dcd6b-m65pm -c openclaw-gateway -n openclaw-tenant-1 --tail=50
```

日誌中常見的關鍵訊息：

| 日誌訊息 | 含義 |
|---------|------|
| `Agent connected: R1` | Agent 成功連線至 Gateway |
| `Heartbeat received from: W1` | 收到 Agent 心跳回報 |
| `Task dispatched to: Q1` | 任務已派發給 Agent |
| `ECONNREFUSED` | 連線被拒絕，Gateway 可能正在重啟 |
| `adapter_failed` | Adapter 連線失敗，檢查 Agent 設定 |

> **除錯技巧：** 若需要查看更多歷史日誌，可增加 `--tail` 的數值（例如 `--tail=200`）。若需要即時監控日誌，可使用 `--follow` 參數（按 Ctrl+C 結束）。

![OpenClaw Gateway 日誌輸出範例](images/ch14_03_gateway_logs.png)

---

### 步驟 4：重設 Agent 狀態（從 error/running 恢復）

當 Agent 狀態卡在 `running` 或 `error` 時，可手動重設為 `idle`：

```bash
kubectl exec paperclip-db-99c454b69-48s4p -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip -t -A -c \
  "UPDATE agents SET status = 'idle' WHERE company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb' AND status IN ('running', 'error');"
```

> **注意：** 此操作會將所有 `running` 和 `error` 狀態的 Agent 重設為 `idle`。若只需重設特定 Agent，請在 SQL 中加入 `AND name = 'Agent名稱'` 條件。執行前請確認沒有 Agent 正在合法執行任務，以免中斷正常作業。

---

### 步驟 5：手動執行 Odoo 同步

手動觸發 Paperclip 到 Odoo 的同步作業：

```bash
cd "/var/tmp/vibe-kanban/worktrees/0957-openclaw-v2026-4/k3s project"
python3 sync-paperclip-to-odoo.py
```

預期輸出：

```
[2026-05-29 10:30:01] Starting Paperclip → Odoo sync...
[2026-05-29 10:30:02] Fetching issues from Paperclip...
[2026-05-29 10:30:03] Found 5 new issues to sync
[2026-05-29 10:30:04] Syncing: CMP-0042 撰寫 CBAM 碳邊境調整機制新聞報導 → Done
[2026-05-29 10:30:05] Syncing: CMP-0043 撰寫台達電碳中和故事報導 → Done
[2026-05-29 10:30:06] Syncing: CMP-0044 亞太碳價趨勢數據新聞 → Done
[2026-05-29 10:30:07] Syncing: CMP-0045 碳費政策深度分析 → Done
[2026-05-29 10:30:08] Syncing: CMP-0046 ESG 基金市場觀察 → Done
[2026-05-29 10:30:09] Sync completed: 5 synced, 0 failed
```

> **提示：** 同步腳本會自動偵測已同步過的 Issue，不會重複同步。若需要強制重新同步特定 Issue，請先在 Odoo 中刪除對應的任務。

![手動執行 Odoo 同步腳本的輸出結果](images/ch14_04_manual_sync_output.png)

---

### 步驟 6：查看 Heartbeat 執行歷史

查看 Agent 最近的 Heartbeat 執行紀錄，以追蹤任務執行狀態：

```bash
kubectl exec paperclip-db-99c454b69-48s4p -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip -t -A -c \
  "SELECT a.name, hr.status, hr.error_code, hr.started_at::text
   FROM heartbeat_runs hr JOIN agents a ON hr.agent_id = a.id
   WHERE a.company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb'
   ORDER BY hr.started_at DESC LIMIT 20;"
```

輸出欄位說明：

| 欄位 | 說明 |
|------|------|
| name | Agent 名稱 |
| status | 執行狀態（completed / failed / timed_out） |
| error_code | 錯誤代碼（成功時為空） |
| started_at | 執行開始時間 |

> **常見 error_code 對照：**
> - `adapter_failed`：Adapter 連線失敗，Gateway 可能無法連線
> - `timed_out`：任務執行逾時，通常是任務描述太複雜
> - `no_task`：Agent 執行心跳但沒有待處理任務（正常現象）

---

### 步驟 7：Ed25519 金鑰管理

每個 Agent 在 OpenClaw Gateway 中都擁有唯一的 Ed25519 裝置金鑰（Device Key），用於 Gateway 配對認證。此金鑰確保只有經過授權的 Agent 才能透過 Gateway 執行任務。

若需要為新 Agent 產生金鑰：

```bash
openssl genpkey -algorithm Ed25519 -out agent_NEW.pem
```

查看公鑰：

```bash
openssl pkey -in agent_NEW.pem -pubout -outform DER | base64
```

> **安全提醒：** Ed25519 私鑰（.pem 檔案）必須妥善保管，不可外洩。每個 Agent 的金鑰必須是唯一的，不可共用。若懷疑金鑰遭到洩漏，應立即產生新金鑰並在 Gateway 中更新配對。

---

### 步驟 8：Gateway Workspace 與 API Key 管理

#### 1:1 Workspace 架構

每個 OpenClaw Agent 擁有獨立的 workspace 目錄，位於 PVC 上：

```
/mnt/openclaw-agents/_workspace-{agentId}/
├── paperclip-claimed-api-key.json   # API 連線金鑰
├── .env                              # 環境變數（PAPERCLIP_API_URL）
└── AGENTS.md                         # Agent 身份標記
```

查看所有 workspace：

```bash
GW_POD=$(kubectl get pods -n openclaw-tenant-1 -l app=openclaw-gateway -o jsonpath='{.items[0].metadata.name}')
kubectl exec $GW_POD -c nerve -n openclaw-tenant-1 -- ls /mnt/openclaw-agents/ | grep workspace
```

#### API Key 檔案格式

每個 workspace 中的 `paperclip-claimed-api-key.json`：

```json
{
  "apiKey": "pk_test_editorial_team_1779960239",
  "apiUrl": "http://paperclip-svc:3100/api"
}
```

| 欄位 | 說明 |
|------|------|
| apiKey | Paperclip Board API 金鑰（所有 Agent 共用） |
| apiUrl | Paperclip API 的內部服務 URL（**必須是 `paperclip-svc:3100`，不是 `localhost:3100`**） |

#### 若 API Key 遺失（Pod 重啟後）

Gateway Pod 重啟後，workspace 目錄會保留在 PVC 上，但如果目錄遺失需要重建：

```bash
GW_POD=$(kubectl get pods -n openclaw-tenant-1 -l app=openclaw-gateway -o jsonpath='{.items[0].metadata.name}')

# 為某個 Agent 重建 workspace
AGENT_ID="r1-policy"
kubectl exec $GW_POD -c nerve -n openclaw-tenant-1 -- sh -c "
  mkdir -p /mnt/openclaw-agents/_workspace-${AGENT_ID}
  echo '{\"apiKey\": \"pk_test_editorial_team_1779960239\", \"apiUrl\": \"http://paperclip-svc:3100/api\"}' > /mnt/openclaw-agents/_workspace-${AGENT_ID}/paperclip-claimed-api-key.json
  echo 'PAPERCLIP_API_URL=http://paperclip-svc:3100' > /mnt/openclaw-agents/_workspace-${AGENT_ID}/.env
"
```

> **注意**：`apiUrl` 必須使用 K8s 內部 DNS `paperclip-svc:3100`。使用 `localhost:3100` 會導致 Agent 無法連線到 Paperclip API。

---

### 步驟 8a：在 OpenClaw Console 管理 Agent

OpenClaw Console（`https://cindytech1-openclaw.woowtech.io`）提供圖形化介面管理 Agent：

#### 查看 Agent 列表

1. 用密碼 `cindytech` 連線到 Console
2. 點擊左側 **AGENT** > **Agents**
3. 頂部下拉選單可看到所有 19 個 Agent（main + 17 ESGTimes + test-poke）

#### 直接與 Agent 對話

1. 點擊左側 **CHAT** > **Chat**
2. 頂部下拉選單選擇 Agent（例如 `r1-policy`）
3. 在底部對話框輸入訊息，按 Enter 送出
4. Agent 會以其角色性格回應

#### 查看 Agent Workspace 檔案

1. 點擊 **Agents** 頁面
2. 從下拉選單選擇 Agent
3. 點擊 **Files** 標籤
4. 可查看 AGENTS.md、SOUL.md 等 workspace 檔案

---

### 步驟 9：Issue 計數器重設（如需要）

當 Issue 編號出現不同步時（例如建立新 Issue 時出現 `duplicate key` 錯誤），需要重設 Issue 計數器：

```bash
kubectl exec paperclip-db-99c454b69-48s4p -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip -t -A -c \
  "UPDATE companies SET issue_counter = (SELECT MAX(CAST(SUBSTRING(identifier FROM 'CMP-([0-9]+)') AS INTEGER)) FROM issues WHERE company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb') WHERE id = '260d546d-46a2-463e-bcc0-23715a5bebeb';"
```

> **說明：** 此指令會將公司的 Issue 計數器重設為目前資料庫中最大的 Issue 編號。這樣下一個新建的 Issue 就會從正確的編號繼續遞增，避免 `duplicate key` 錯誤。此操作不會影響現有的 Issue。

---

## Chapter 15：常見問題排除（FAQ）

### 問題速查表

| # | 問題 | 原因 | 解決方式 |
|---|------|------|---------|
| 1 | Agent heartbeat 顯示 `adapter_failed` | Gateway 連線問題，Agent 無法連上 OpenClaw Gateway | 檢查 Gateway Pod 是否為 `Running` 狀態且 `2/2 Ready`，必要時刪除 Pod 讓 K3s 自動重建 |
| 2 | Agent heartbeat 顯示 `timed_out` | 任務描述過於複雜，或 Gateway 同時處理太多請求導致過載 | 簡化任務描述內容，減少單次任務的複雜度；等待其他 Agent 完成任務後再重試 |
| 3 | Agent 狀態卡在 `running` | 上次任務執行異常中斷，狀態未正確回寫為 `idle` | 使用 SQL 手動重設狀態：`UPDATE agents SET status = 'idle' WHERE name = '...'` |
| 4 | Agent 狀態顯示 `error` | Agent 多次執行失敗，系統自動將狀態標記為 `error` | 先重設為 `idle`，然後檢查 adapter config 設定是否正確，確認 Gateway 連線正常 |
| 5 | Issue 狀態卡在 `in_progress` | Agent 領取任務後無法完成（可能因 Gateway 斷線或逾時） | 檢查對應 Agent 是否為 `idle`，將 Issue 狀態手動改回 `todo`，重新指派觸發執行 |
| 6 | Issue 狀態變成 `blocked` | Paperclip 自動重試機制多次失敗後，將 Issue 標記為 `blocked` | 將 Issue 狀態重設為 `todo`，排除根本原因後重新觸發 Agent 執行 |
| 7 | Issue Comment 作者顯示錯誤 | Gateway 層級共用同一組 API Key，所有 Agent 的操作都以相同身份寫入 | 此為已知限制。實際執行的 Agent 可從 `heartbeat_runs` 資料表追蹤確認 |
| 8 | OpenClaw Gateway Pod 反覆 crash | 太多 Agent 同時執行任務，Gateway 資源不足 | 控制 Agent 並發數量，建立 Issue 時間隔 10-15 秒，避免同時觸發多個 Agent |
| 9 | Gateway 日誌顯示 `ECONNREFUSED` | Gateway Pod 剛完成重啟，服務尚未完全就緒 | 等待 30 秒後再重試，確認 Pod 狀態為 `2/2 Running` |
| 10 | Agent 回覆「缺少 API Key」 | `/mnt/openclaw-agents/_workspace/paperclip-claimed-api-key.json` 檔案遺失或損壞 | 重新建立 `paperclip-claimed-api-key.json` 檔案，填入正確的 `apiKey` 和 `apiUrl` |
| 11 | Odoo 同步失敗：`connection refused` | Odoo 服務無法連線，可能是 Odoo 服務未啟動或網路不通 | 檢查 Odoo 服務的 URL 是否正確，確認帳號密碼有效，驗證網路連通性 |
| 12 | Odoo 同步失敗：`duplicate key` | Paperclip 的 `issue_counter` 與 Odoo 中的任務編號不同步 | 執行 Issue 計數器重設 SQL（參見 Chapter 14 步驟 9），然後重新執行同步 |
| 13 | Agent 產出內容為簡體中文 | Agent 的 System Prompt（Instructions）中未明確強調使用繁體中文 | 檢查 Agent 的 Instructions 是否包含「請使用繁體中文撰寫」等明確規範，補充繁中書寫要求 |
| 14 | 文章字數不符合要求 | Agent 的 System Prompt 中字數規範不夠明確或位置不夠突出 | 在 Instructions 中加強字數限制說明，建議放在最前面或最後面的醒目位置 |
| 15 | Q1/Q2 評分表格式不正確 | Agent 的 System Prompt 中評分表格式未正確載入或描述不夠具體 | 檢查 Q1/Q2 的 Instructions 是否包含完整的評分表範本與格式要求 |

---

### 詳細問題排除說明

#### 問題 1：Agent heartbeat 顯示 adapter_failed

**症狀：** 在 `heartbeat_runs` 資料表中，Agent 的執行紀錄持續顯示 `adapter_failed` 錯誤。

**排查步驟：**

1. 確認 Gateway Pod 狀態：
```bash
kubectl get pods -n openclaw-tenant-1 | grep gateway
```

2. 若 Pod 不是 `Running` 或 Ready 數不是 `2/2`，刪除 Pod 讓 K3s 重建：
```bash
kubectl delete pod openclaw-gateway-6b879dcd6b-m65pm -n openclaw-tenant-1
```

3. 等待新 Pod 啟動後，重新觸發 Agent 任務。

#### 問題 3：Agent 狀態卡在 running

**症狀：** Agent 狀態長時間顯示 `running`，但實際上沒有在執行任務。

**排查步驟：**

1. 確認 Agent 是否確實沒有在執行任務（查看 heartbeat_runs）
2. 手動重設特定 Agent：
```bash
kubectl exec paperclip-db-99c454b69-48s4p -n openclaw-tenant-1 -- \
  psql -U paperclip -d paperclip -t -A -c \
  "UPDATE agents SET status = 'idle' WHERE company_id = '260d546d-46a2-463e-bcc0-23715a5bebeb' AND name = 'R1 政策法規研究員';"
```

#### 問題 8：OpenClaw Gateway Pod 反覆 crash

**症狀：** Gateway Pod 頻繁重啟，Agent 執行任務時經常失敗。

**排查步驟：**

1. 查看 Pod 重啟次數：
```bash
kubectl get pods -n openclaw-tenant-1 | grep gateway
```

2. 查看 crash 原因：
```bash
kubectl describe pod openclaw-gateway-6b879dcd6b-m65pm -n openclaw-tenant-1
```

3. 控制並發：建立 Issue 時間隔 10-15 秒，避免同時觸發超過 3 個 Agent。

#### 問題 12：Odoo 同步失敗 duplicate key

**症狀：** 執行 `sync-paperclip-to-odoo.py` 時出現 `duplicate key` 錯誤。

**排查步驟：**

1. 重設 Issue 計數器（參見 Chapter 14 步驟 9）
2. 重新執行同步：
```bash
python3 sync-paperclip-to-odoo.py
```

---

### 系統健康檢查清單

在日常維運中，建議定期執行以下健康檢查項目：

| 檢查項目 | 檢查指令 | 預期結果 |
|---------|---------|---------|
| Pod 全部 Running | `kubectl get pods -n openclaw-tenant-1` | 所有 Pod 狀態為 `Running` |
| OpenClaw Gateway Ready | `kubectl get pods -n openclaw-tenant-1 \| grep gateway` | 顯示 `2/2 Ready` |
| API Key 檔案存在 | 確認每個 workspace 的 `paperclip-claimed-api-key.json`（17 個） | 檔案存在且 `apiUrl` 為 `paperclip-svc:3100` |
| Adapter Config 正確 | 檢查 adapter_config 包含 `paperclipApiUrl` 和 `agentId` | `paperclipApiUrl` 指向正確 URL，`agentId` 對應 OpenClaw agent |
| 所有 Agent 狀態正常 | 執行 Agent 狀態查詢 SQL | 所有 Agent 為 `idle`（無異常 `error` 或長時間 `running`） |
| Issue 計數器正確 | 建立測試 Issue 確認編號連續 | 新 Issue 編號為上一個 +1 |

健康檢查清單（快速核對用）：

- [ ] `kubectl get pods` 全部 Running
- [ ] OpenClaw Gateway 2/2 Ready
- [ ] 每個 Agent 的 workspace 目錄存在（`_workspace-{agentId}`）
- [ ] 每個 workspace 的 `paperclip-claimed-api-key.json` 存在且 `apiUrl` 為 `paperclip-svc:3100`
- [ ] `adapter_config` 包含 `paperclipApiUrl` 和 `agentId`
- [ ] All agents status = idle
- [ ] Issue counter 正確
- [ ] OpenClaw Console Agents 下拉選單顯示 19 個 Agent

---

## 附錄：商用企業級測試結果

### 測試概述

2026 年 5 月 30 日執行的 17-Agent 全覆蓋測試，使用真實 ESG 企業場景驗證系統的商用部署品質。

### 測試結果（CMP-130 ~ CMP-149）

| Issue | Agent | OpenClaw ID | 內容品質 |
|-------|-------|-------------|---------|
| CMP-130 | R1 政策法規研究員 | `r1-policy` | ✅ CSRD 素材包（ESRS 準則+台灣影響） |
| CMP-131 | R2 企業案例研究員 | `r2-enterprise` | ✅ 台積電 ESG 案例（RE100/SBTi/供應鏈） |
| CMP-132 | R3 數據趨勢研究員 | `r3-data` | ✅ 全球能源轉型 Top3 報告 |
| CMP-133 | R4 國際媒體掃描員 | `r4-international` | ✅ 國際 ESG 新聞 Top5 |
| CMP-134 | R5 台灣在地線人 | `r5-taiwan` | ✅ 台灣 5-6 月動態 Top5 |
| CMP-135 | W1 新聞記者風 | `w1-news` | ✅ 碳費新聞報導（倒三角+3標題+meta） |
| CMP-136 | W2 知識型教師風 | `w2-teacher` | ✅ 碳盤查教學（5步驟+比喻） |
| CMP-137 | W3 學術分析師風 | `w3-analyst` | ✅ ISSB vs GRI 深度分析（Executive Summary） |
| CMP-138 | W4 說故事風 | `w4-storyteller` | ✅ 台南漁民故事（場景開場+對話） |
| CMP-139 | W5 意見領袖風 | `w5-opinion` | ✅ ESG 報告漂綠評論（鮮明立場） |
| CMP-140 | W6 數據新聞風 | `w6-data-news` | ✅ 碳市場價格比較（6國+圖表建議） |
| CMP-141 | W7 簡潔速讀風 | `w7-quickread` | ✅ CBAM 懶人包（TL;DR+5件事） |
| CMP-142 | M1 總編輯長 | `m1-editor-chief` | ✅ 6月內容策略（8主題+W/R分配） |
| CMP-143 | M2 內容主編 | `m2-content-editor` | ✅ 5則素材分配單（W1-W7對應） |
| CMP-144 | Q1 內容品質審查員 | `q1-quality` | ✅ 退回測試：事實6/結構2/繁中5/數據1 → **退回** |
| CMP-145 | Q2 ESG專業顧問 | `q2-esg-expert` | ✅ 退回測試：術語2/法規1/標準1 → **退回** |
| CMP-146 | M3 營運主編 | `m3-ops-editor` | ✅ 週報+發布排程 |

### 驗證通過項目

| 驗證項目 | 結果 |
|---------|------|
| 17/17 agents 產出內容 | ✅ 100% |
| Gateway 1:1 路由正確 | ✅ 每個 agent 路由到對應的 OpenClaw agent |
| QA 退回機制 | ✅ Q1 總分 3.5/10, Q2 術語 2/法規 1 → 雙雙退回 |
| 7 種寫作風格差異化 | ✅ 倒三角/教學/分析/故事/評論/數據/懶人包 |
| Odoo 同步 | ✅ 22 tasks + 76 comments synced |
| OpenClaw Console 對話 | ✅ 可選擇個別 agent 直接對話，保持角色性格 |
| Gateway 穩定性 | ✅ 0 crash（使用 15s stagger + 300s cooldown） |

### OpenClaw Console 對話驗證

在 Nerve/OpenClaw Chat 介面中，成功選擇個別 Agent 並進行獨立對話：

| Agent | 問題 | 回覆性格驗證 |
|-------|------|-------------|
| `m1-editor-chief` | 「列出3個本月ESG重點主題」 | ✅ 回覆 ISSB 準則、碳費審議會、AI×ESG（策略性思維） |
| `w4-storyteller` | 「你是誰？」 | ✅ 「溫暖而尖銳，像靠譜的同行者而不是打字機器」 |
| `r3-data` | 「你是誰？」 | ✅ 「數據趨勢研究員，挖掘 ESG 數據與趨勢情報」 |
| `q2-esg-expert` | 「你是誰？」 | ✅ 「ESG 專業顧問，專業建議與品質把關」 |

---

> **需要協助？** 請聯繫 WOOW Tech 沃科技團隊。
