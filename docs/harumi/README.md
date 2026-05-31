# 春美ハルミHarumi — Odoo 18 日式餐廳管理平台使用手冊

> **版本**：Odoo 18 Community
> **適用對象**：POS 收銀人員、廚房人員、店長、Portal 管理者
> **最後更新**：2026-05-31
> **維護團隊**：WOOW Tech 沃科技

---

## 目錄

| 編號 | 章節名稱 | 說明 |
|------|---------|------|
| [1](#1-平台總覽與帳號說明) | 平台總覽與帳號說明 | 系統網址、帳號、店面資訊 |
| [1.5](#15-新手快速開始指南) | 新手快速開始指南 | 依角色學習路徑、5 分鐘速覽 |
| [2](#2-登入與帳號管理) | 登入與帳號管理 | 登入畫面、帳號密碼 |
| [3](#3-pos-收銀台--開店與基本操作) | POS 收銀台 — 開店與基本操作 | 解鎖收銀機、樓面圖、選桌、加產品 |
| [4](#4-pos-收銀台--結帳與付款) | POS 收銀台 — 結帳與付款 | 現金付款、綠界線上付款、直接付款出廚房單 |
| [5](#5-pos-收銀台--售完管理) | POS 收銀台 — 售完管理 | 動作選單、售完管理 Popup |
| [6](#6-pos-收銀台--廚房備註與訂單管理) | POS 收銀台 — 廚房備註與訂單管理 | 備註欄、訂單按鈕 |
| [7](#7-客人自助點餐--內用流程) | 客人自助點餐 — 內用流程 | 桌上 QR Code → 產品 → 付款（現場+線上） |
| [8](#8-客人自助點餐--外帶流程) | 客人自助點餐 — 外帶流程 | 外帶 QR Code → 產品 → 付款（僅線上） |
| [9](#9-kds-廚房顯示螢幕) | KDS 廚房顯示螢幕 | 訂單顯示、品項打勾、Done、語言切換 |
| [10](#10-escpos-廚房出單機) | ESC/POS 廚房出單機 | 印表機設定、Print Copies、Cloud Relay |
| [11](#11-綠界金流設定) | 綠界金流設定 | ECPay MerchantID、HashKey/IV |
| [13](#13-portal-管理者入口) | Portal 管理者入口 | /my/pos 店面卡片 |
| [16](#16-前台網站管理) | 前台網站管理 | 首頁、預約訂位、菜單、聯絡我們 |
| [18](#18-qr-code-連結一覽) | QR Code 連結一覽 | 所有桌位 + 外帶連結 |
| [19](#19-常見問題faq) | 常見問題（FAQ） | 常見問題與解答 |

---

## 1. 平台總覽與帳號說明

本章說明春美 HARUMI Odoo 18 日式餐廳管理平台的整體架構。

### 步驟 1：系統網址一覽

| 系統 | 網址 | 說明 |
|------|------|------|
| 前台網站 | https://harumi-odoo.woowtech.io/ | 春美官方網站 |
| 後台管理介面 | https://harumi-odoo.woowtech.io/odoo | Odoo 18 後台入口 |
| POS 收銀台 | https://harumi-odoo.woowtech.io/pos/ui?config_id=1 | POS 收銀台直連 |
| 客人自助點餐（外帶） | https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66 | 外帶自助點餐 |
| KDS 廚房螢幕 | https://harumi-odoo.woowtech.io/pos-kds/1?token=0b298baf780e40e1 | 廚房顯示螢幕 |
| Portal 管理者入口 | https://harumi-odoo.woowtech.io/my/pos | 管理者 POS 入口 |

### 步驟 2：預設帳號與角色

| 帳號 | 密碼 | 姓名 | 角色 | 說明 |
|------|------|------|------|------|
| `admin` | `admin` | Administrator | 全系統管理 | 後台管理、POS 設定、產品管理 |
| `fuhao1219@gmail.com` | `harumi4711` | 李富豪 | 店長（最高權限） | POS 操作、訂單管理、預約通知 |

### 步驟 3：店面資訊

| 項目 | 內容 |
|------|------|
| 公司名稱 | 春美ハルミHarumi |
| 地址 | 台北市文山區興隆路四段67號1樓 |
| 電話 | 0958-274-711 |
| 營業時間 | 週一至週六 17:00 — 22:00 |
| 幣別 | 新台幣（TWD） |
| POS 名稱 | 餐廳 |

### 步驟 4：平台功能模組

| 功能模組 | 說明 |
|---------|------|
| POS 銷售點 | 餐廳 POS 收銀系統，支援現金與綠界線上付款 |
| Self-Order 自助點餐 | 客人手機掃 QR Code 點餐、線上付款 |
| KDS 廚房顯示螢幕 | 即時廚房訂單顯示，支援品項打勾、出餐完成 |
| ESC/POS 出單機 | 廚房單自動列印，支援 Cloud Relay 雲端出單 |
| 綠界金流 | ECPay 信用卡線上付款 |
| 電子發票 | ECPay 台灣電子統一發票 |
| 售完管理 | UberEats 風格的產品售完即時切換 |
| 前台網站 | 官方網站（首頁、預約、菜單、聯絡） |

---

## 1.5 新手快速開始指南

### 依角色選擇學習路徑

| 角色 | 建議學習章節 | 說明 |
|------|------------|------|
| POS 收銀員 | Ch3 ~ Ch6 | 開店、結帳付款、售完管理、廚房備註 |
| 廚房人員 | Ch9 ~ Ch10 | KDS 操作、出單機設定 |
| 店長 | Ch1 ~ Ch18 | 全部功能 |
| 客人 | Ch7 ~ Ch8 | 自助點餐（內用/外帶） |

### 您的第一筆 POS 交易（5 分鐘速覽）

1. 以管理員帳號登入後台（`admin` / `admin`）。
2. 進入 POS 收銀台，點擊「解鎖收銀機」。
3. 選擇一張餐桌（例如 Table 3）。
4. 點擊商品加入購物車（如「極上金羽丼」）。
5. 點擊「付款」→ 選擇「現金」→ 點擊「核實」。
6. 完成！廚房自動收到訂單並出單。

---

## 2. 登入與帳號管理

### 步驟 1：開啟登入頁面

在瀏覽器輸入 `https://harumi-odoo.woowtech.io/web/login`。

![登入頁面](images/ch02_01_login_page.png)

### 步驟 2：輸入帳號密碼

- 管理員：`admin` / `admin`
- 店長：`fuhao1219@gmail.com` / `harumi4711`

---

## 3. POS 收銀台 — 開店與基本操作

### 步驟 1：解鎖收銀機

進入 POS 後，看到鎖定畫面，點擊「解鎖收銀機」。

![鎖定畫面](images/ch03_01_pos_lock_screen.png)

### 步驟 2：樓面圖

解鎖後看到樓面圖，顯示所有餐桌。有訂單的桌子會標示不同顏色。

![樓面圖](images/ch03_02_pos_floor_plan.png)

### 步驟 3：選擇餐桌

點擊一張桌子（如 Table 3）進入產品畫面。

![產品畫面（空訂單）](images/ch03_03_pos_product_screen_empty.png)

### 步驟 4：瀏覽產品分類

上方有多個分類標籤，點擊可切換：

| 分類 | 截圖 |
|------|------|
| 海鮮丼飯 | ![海鮮丼飯](images/ch03_04_pos_category_donburi.png) |
| 酒餚烤物 | ![酒餚烤物](images/ch03_05_pos_category_yakimono.png) |
| 刺身 | ![刺身](images/ch03_06_pos_category_sashimi.png) |
| 野菜 | ![野菜](images/ch03_07_pos_category_yasai.png) |
| 酒類 | ![酒類](images/ch03_08_pos_category_sake.png) |

### 步驟 5：加入產品到訂單

點擊產品即可加入左側購物車。

![加入一個產品](images/ch03_09_pos_product_added_one.png)

繼續加入更多產品：

![多個產品](images/ch03_11_pos_multiple_products.png)

### 步驟 6：數量與價格調整

左下角的數字鍵盤可以修改數量、價格、折扣。

![數字鍵盤](images/ch03_12_pos_order_numpad.png)

### 步驟 7：廚房備註

點擊「廚房備註」按鈕可以為產品加上備註（例如「不要辣」）。

![廚房備註](images/ch03_13_pos_kitchen_note_popup.png)

### 步驟 8：動作選單

點擊「動作」按鈕可以看到更多功能選項。

![動作選單](images/ch03_14_pos_actions_popup.png)

### 步驟 9：訂單按鈕

左下角的「訂單」按鈕顯示待出單的品項數量。點擊可以確認送出訂單到廚房。

![訂單按鈕](images/ch03_17_pos_order_button_badge.png)

### 步驟 10：帶有訂單的樓面圖

回到樓面圖，可以看到有訂單的桌子顯示不同狀態。

![帶有訂單的樓面圖](images/ch03_18_pos_floorplan_occupied.png)

### 步驟 11：餐桌列表視圖

點擊「餐桌」切換到列表視圖，可以看到所有桌子的狀態。

![餐桌列表](images/ch03_19_pos_table_list_view.png)

### 步驟 12：QR Code 頁面

點擊「取得二維碼」可以下載或顯示桌上 QR Code。

![QR Code 頁面](images/ch03_21_pos_qrcode_page.png)

---

## 4. POS 收銀台 — 結帳與付款

### 步驟 1：進入付款畫面

確認購物車內容後，點擊「付款」按鈕。

![付款畫面](images/ch04_01_pos_payment_screen.png)

### 步驟 2：選擇現金付款

點擊「現金」方式，金額自動填入。

![現金付款](images/ch04_02_pos_cash_selected.png)

### 步驟 3：確認付款

點擊「核實」按鈕完成交易。

![核實按鈕](images/ch04_03_pos_validate_button.png)

### 步驟 4：收據畫面

付款完成後顯示收據，可以列印或直接開始下一筆訂單。

![收據畫面](images/ch04_04_pos_receipt_screen.png)

### 步驟 5：綠界線上付款

付款畫面也支援「綠界」線上信用卡付款。

![綠界付款選項](images/ch04_05_pos_payment_ecpay.png)

點擊「綠界」顯示為選中狀態：

![綠界已選](images/ch04_06_pos_ecpay_selected.png)

### 重要提醒：直接付款 = 廚房出單

**不需要先按「訂單」再付款。** 直接按「付款」完成結帳後，廚房單會自動印出。

---

## 5. POS 收銀台 — 售完管理

### 步驟 1：開啟售完管理

在產品頁面點擊「動作」→「產品售完管理」。

![售完管理入口](images/ch03_14_pos_actions_popup.png)

### 步驟 2：售完管理 Popup

顯示所有產品列表，每個產品有開關：
- 開關**開啟**（綠色）= 有庫存
- 開關**關閉**（灰色）= 售完

![售完管理 Popup](images/ch05_01_pos_soldout_popup.png)

### 步驟 3：查看已售完品項

點擊「已售完」分頁，可以快速看到哪些品項已標記為售完。

![已售完列表](images/ch05_02_pos_soldout_list.png)

### 步驟 4：售完產品覆蓋效果

關閉 Popup 後，售完的產品會顯示灰色覆蓋和「售完」文字。客人在自助點餐頁面也會看到同樣的效果。

![售完覆蓋效果](images/ch03_15_pos_product_soldout_overlay.png)

---

## 6. POS 收銀台 — 廚房備註與訂單管理

### 廚房備註

選取一個訂單品項後，點擊「廚房備註」按鈕可以輸入特殊要求。備註會印在廚房單上，KDS 也會顯示。

![廚房備註](images/ch03_13_pos_kitchen_note_popup.png)

### 外帶訂單溝通

對於外帶訂單，收銀員可以在備註欄打上預計取餐時間（例如「約 19:30 可取」），客人在「我的訂單」頁面可以看到。

---

## 7. 客人自助點餐 — 內用流程

### 步驟 1：掃桌上 QR Code

每張桌子有專屬的 QR Code。客人掃描後進入首頁。

> **重要**：桌上 QR Code 不會出現「堂食/外賣」選擇頁，直接進入點餐。

![內用首頁](images/ch07_01_dinein_landing.png)

### 步驟 2：瀏覽菜單

點擊「菜餚點餐」直接進入產品列表。

![內用產品列表](images/ch07_02_dinein_products.png)

向下滾動可以看到更多產品分類：

![更多產品](images/ch07_03_dinein_products_scroll.png)

### 步驟 3：選擇產品

點擊產品查看詳情，可以選擇數量和加量選項。

![產品詳情](images/ch07_04_dinein_product_detail.png)

### 步驟 4：購物車

確認品項後，點擊「訂單」查看購物車。

![購物車](images/ch07_05_dinein_cart.png)

### 步驟 5：付款

點擊「付款」進入付款頁面。**內用可以選擇「現場結帳」或「線上支付」。**

![內用付款（兩個選項）](images/ch07_06_dinein_payment_both.png)

---

## 8. 客人自助點餐 — 外帶流程

### 步驟 1：掃外帶 QR Code

外帶 QR Code 沒有 `table_identifier` 參數，客人掃描後進入首頁。

> **重要**：外帶連結不會出現桌號選擇，也不會出現「堂食/外賣」選擇頁。

![外帶首頁](images/ch08_01_takeaway_landing.png)

### 步驟 2：瀏覽菜單

直接進入產品列表，與內用相同的菜單。

![外帶產品列表](images/ch08_02_takeaway_products.png)

### 步驟 3：選擇產品

![產品詳情](images/ch08_03_takeaway_product_detail.png)

### 步驟 4：購物車

![購物車](images/ch08_04_takeaway_cart.png)

### 步驟 5：付款

點擊「付款」進入付款頁面。**外帶只有「線上支付」，沒有「現場結帳」。**

![外帶付款（只有線上）](images/ch08_05_takeaway_payment_online_only.png)

---

## 9. KDS 廚房顯示螢幕

### 步驟 1：開啟 KDS

在任何瀏覽器或平板開啟 KDS 網址（不需要登入）：

```
https://harumi-odoo.woowtech.io/pos-kds/1?token=0b298baf780e40e1
```

![KDS 主畫面](images/ch09_01_kds_main.png)

### 步驟 2：語言切換

點擊「EN/中」按鈕切換為中文介面。

![KDS 中文](images/ch09_02_kds_chinese.png)

### 步驟 3：三種視圖

| 視圖 | 功能 |
|------|------|
| 訂單 | 顯示進行中的訂單卡片，每張卡片有品項列表和計時器 |
| 品項 | 品項總覽，可以看到所有待做的品項 |
| 歷史 | 已完成的訂單記錄 |

### 步驟 4：操作方式

| 操作 | 說明 |
|------|------|
| 點品項打勾 | 該品項完成（劃線） |
| 按 Done | 整張訂單完成出餐 |
| 按 Recall | 召回已完成的訂單 |

### 內用 vs 外帶辨識

- **有桌號**（如 Table 3）= 內用 → 送到桌上
- **無桌號**（只有訂單編號）= 外帶 → 放取餐區

---

## 10. ESC/POS 廚房出單機

### 印表機設定

| 項目 | 設定值 |
|------|--------|
| 印表機名稱 | 廚房機 |
| 類型 | Network ESC/POS |
| Cloud Relay URL | https://harumi-ha.woowtech.io/ |
| 紙張寬度 | 80mm |
| 列印張數 | 可設定 1-10 張 |

### Print Copies（列印張數）

在 POS 設定 → 印表機 → Print Copies 欄位，可以設定每次出單印幾張。例如設定 2 張，每次出廚房單會印 2 份。

---

## 11. 綠界金流設定

### 設定摘要

| 項目 | 值 |
|------|-----|
| MerchantID | 3492679 |
| HashKey | wGe2pkWAhqrQ3KDh |
| HashIV | zqlelAT3mmXsC0RI |
| Domain | https://harumi-odoo.woowtech.io/ |
| 信用卡 | 啟用 |
| 電子發票 Demo mode | 關閉（正式模式） |

> 綠界後台的「信用卡交易設定」需要填入：
> - 成交頁面：`https://harumi-odoo.woowtech.io/payment/ecpay/return`
> - 失敗頁面：`https://harumi-odoo.woowtech.io/payment/ecpay/return`
> - 幕後回傳：`https://harumi-odoo.woowtech.io/payment/ecpay/notify`

---

## 13. Portal 管理者入口

### 步驟 1：進入 Portal POS 入口

登入後到 `https://harumi-odoo.woowtech.io/my/pos`，可以看到 POS 店面卡片。

![Portal POS 入口](images/ch13_01_portal_mypos.png)

### 功能

- 查看 POS 營業狀態（營業中/已關閉）
- 一鍵開啟 POS 收銀台
- 一鍵開啟 KDS 廚房螢幕

---

## 16. 前台網站管理

春美的前台網站包含以下頁面：

| 頁面 | 網址 | 截圖 |
|------|------|------|
| 首頁 | `/` | ![首頁](images/ch16_01_website_home.png) |
| 預約訂位 | `/reservation` | ![預約](images/ch16_02_website_reservation.png) |
| 菜餚介紹 | `/menu` | ![菜單](images/ch16_03_website_menu.png) |
| 聯絡我們 | `/contactus` | ![聯絡](images/ch16_04_website_contact.png) |
| 交通資訊 | `/access` | ![交通](images/ch16_05_website_access.png) |
| 最新消息 | `/news` | ![消息](images/ch16_06_website_news.png) |

---

## 18. QR Code 連結一覽

### 外帶 QR Code

| 用途 | 連結 |
|------|------|
| **外帶點餐** | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66` |

### 內用桌位 QR Code（春美食堂樓層）

| 桌號 | table_identifier | 完整連結 |
|------|-----------------|---------|
| Table 1 | `bf6b6ae6` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=bf6b6ae6` |
| Table 2 | `32f6f316` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=32f6f316` |
| Table 3 | `aeca7ec0` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=aeca7ec0` |
| Table 4 | `bb81dc17` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=bb81dc17` |
| Table 5 | `461a55ca` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=461a55ca` |
| Table 101 | `ddb4aae4` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=ddb4aae4` |
| Table 102 | `7cc7e2c0` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=7cc7e2c0` |
| Table 103 | `4ec9dcca` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=4ec9dcca` |
| Table 105 | `ebc093ce` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=ebc093ce` |
| Table 106 | `79a98317` | `https://harumi-odoo.woowtech.io/pos-self/1?access_token=2190d45b1c7a4a66&table_identifier=79a98317` |

### KDS 連結

```
https://harumi-odoo.woowtech.io/pos-kds/1?token=0b298baf780e40e1
```

---

## 19. 常見問題（FAQ）

### Q1：直接付款不出廚房單？

請確認 POS 瀏覽器已清除快取（Ctrl+Shift+R）。新版模組已修正此問題 — 直接付款即等於確認訂單。

### Q2：外帶客人看到「現場結帳」選項？

這不應該出現。外帶連結（無 `table_identifier`）的付款頁面只會顯示「線上支付」。如果出現「現場結帳」，請確認使用的是正確的外帶連結。

### Q3：KDS 打不開？

確認使用正確的 token：`token=0b298baf780e40e1`。錯誤或缺少 token 會顯示 403 錯誤。

### Q4：廚房單印 1 張但設定 2 張？

需要更新 HA Add-on（ESC/POS Print Proxy）到 v0.6.0 以上版本。舊版不支援 copies 參數。

### Q5：售完的產品關店後還會維持售完嗎？

不會。關閉 POS Session 後，所有售完狀態自動重置。下次開店時所有產品恢復為有庫存。

### Q6：如何區分內用和外帶訂單？

- 有桌號的訂單 = 內用（KDS 顯示 Table X）
- 沒有桌號的訂單 = 外帶（KDS 只顯示訂單編號）

### Q7：綠界交易失敗顯示 CheckMacValue Error？

請確認 HashKey 和 HashIV 前後沒有空格或 Tab 字元。綠界後台的「系統介接設定」複製時容易帶入隱藏字元。

### Q8：POS 設定頁面出現「session stop_at」錯誤？

這是因為 POS Session 被異常關閉（例如伺服器重啟）。請聯繫技術人員修復。

---

> 本手冊由 WOOW Tech 沃科技維護。如有問題請聯繫技術支援。
