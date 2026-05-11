# Day 23 — Worksheet Product ROI Dashboard

**Học viên:** Nguyễn Thị Tuyết — **MSSV:** 2A202600215  
**Sản phẩm:** SmartAds AI — AI decision copilot tối ưu quảng cáo + ưu tiên hành động cho chủ shop thời trang nữ online  
**Ngày:** 11/05/2026

---

## 1. Part A — Adoption Context

| Trường | Trả lời |
|---|---|
| Product | **SmartAds AI** — AI decision copilot tối ưu ngân sách Meta/Google Ads + xếp hạng khách có khả năng mua cao trong 24–48h, có giải thích, human-in-the-loop. |
| Người dùng chính | Chủ shop thời trang nữ online (doanh thu ~200M–1B VND/tháng) + marketer team nhỏ 1–2 người, không có analyst / media buyer full-time. |
| Bối cảnh sử dụng | Shop chạy ads liên tục trên Facebook/Instagram + website; mỗi sáng cần biết tắt ad set nào, đẩy ngân sách vào đâu khi CPA biến động trong cửa sổ 24–48h. |
| Mục tiêu kinh doanh / học tập / vận hành | Giảm **median CPA ≥10%** sau 4 tuần ở nhóm tuân thủ; bảo vệ ROAS trong 48h đầu khi biến động; rút thời gian ra quyết định ads sáng từ 30–60 phút xuống ~15 phút. |
| Rào cản ADKAR chính | **Ability + Reinforcement** — chủ shop không phải media buyer, cần biết *khi nào tin AI, khi nào override*; phải có vòng phản hồi để duy trì hành vi áp dụng đều đặn. |

### 2-4 workflow chính

| # | Workflow | AI làm gì? | Con người kiểm tra ở đâu? | Khi AI sai thì xử lý thế nào? |
|---|---|---|---|---|
| 1 | **Kết nối dữ liệu Meta Ads + đơn hàng (data integration & readiness)** | Pull ads metrics (impressions, CTR, CPC, CPA, ROAS, frequency) + đơn hàng qua OAuth/API; chuẩn hoá, gắn cờ data gap. | Shop xác nhận quyền truy cập + kiểm tra mapping đơn hàng ↔ campaign trong onboarding <60 phút. | Hiển thị cảnh báo "data chưa đủ ổn định → khuyến nghị tạm khoá"; rơi về **Plan B**: import CSV Ads Manager + đơn hàng tuần (T+1). |
| 2 | **Xếp hạng ad set / nhóm khách theo khả năng mua (propensity scoring)** | Chấm điểm 3 mức (cao / trung / thấp) cho từng ad set & segment, kèm confidence. | Chủ shop / marketer review xếp hạng mỗi sáng trước khi quyết định ngân sách. | Nếu confidence <70% → chuyển **Watch mode**; shop có quyền **Reject** xếp hạng và ghi lý do để model học. |
| 3 | **Khuyến nghị tối ưu ngân sách 24–48h (top-3 actions có giải thích)** | Sinh ra 3 khuyến nghị/sáng: tắt ad set X, tăng ngân sách ad set Y, đổi thông điệp segment Z — mỗi khuyến nghị có lý do ngắn + mức tin cậy. | Shop bấm **Accept / Reject / Modify** trước khi áp dụng lên ads manager (không auto-push). | Ghi log lý do reject/modify; nếu burn rate >30% trong 48h sau khi Accept → cảnh báo tự động + đề xuất rollback. |
| 4 | **Watch mode + vòng Accept/Reject/Modify (human-in-the-loop)** | Phát hiện chiến dịch biến động (CPA >150% target), tự đẩy sang Watch mode; tổng hợp pattern lỗi để học. | Marketer xem nhật ký Accept/Reject/Modify hằng tuần, đối chiếu với CPA/ROAS thực tế. | Escalate cho Customer Success: review case, cập nhật rule/prompt; case nặng → đưa vào QA sample tuần kế. |

### 3 tactic tăng adoption

| Tactic | Nhắm vào rào cản nào? | Áp dụng cho workflow nào? | Người phụ trách |
|---|---|---|---|
| **1. "Aha moment" tuần đầu** — 1–2 khuyến nghị đơn giản, lift CPA/ROAS đo được trong 48h, gửi kèm so sánh trước/sau. | **Awareness + Desire** | Workflow 3 (khuyến nghị ngân sách) | Product Lead |
| **2. Onboarding <60 phút có hướng dẫn người thật + case study cộng đồng/agency** — checklist 5 bước, có kênh chat hỗ trợ trực tiếp 7 ngày đầu. | **Knowledge + Ability** | Workflow 1 (kết nối dữ liệu) | Customer Success Lead |
| **3. Giải thích ngắn + confidence + vòng Accept/Reject/Modify** ghi nhận lý do, tóm tắt vào báo cáo tuần "AI học được gì từ bạn". | **Reinforcement (+ Trust)** | Workflow 2, 3, 4 | Product Lead + AI Lead |

---

## 2. Part B — ROI Dashboard

### B.1 Metric toàn product

| Layer | Metric | Baseline | Target | Data source | Owner | Red-team risk | Fix |
|---|---|---:|---:|---|---|---|---|
| Activation | % shop pilot kết nối thành công Meta Ads + đơn hàng và nhận khuyến nghị đầu tiên trong **tuần 1** | 0% (chưa có pilot) | **≥80%** trong tuần 1 | Onboarding log + Meta API status + DB nội bộ | Customer Success Lead | Shop bấm "kết nối" cho có nhưng data không đủ chất lượng → metric ảo | Ghép với **data-readiness check**: chỉ tính activation khi có ≥7 ngày dữ liệu liên tục cả ads + đơn hàng |
| Engagement / Retention | % shop pilot **áp dụng ≥2 khuyến nghị/tuần trong 3 tuần liên tiếp** (leading adoption metric) | 0% | **≥60%** | Action log (Accept/Modify) trong app | Product Lead | "Áp dụng" có thể chỉ là click Accept mà không thực sự đẩy ngân sách | Đối soát với Meta API: chỉ tính khi ngân sách ad set thực sự thay đổi trong 24h sau Accept |
| Trust / Quality / Value | **Median CPA giảm ≥10%** sau 4 tuần ở nhóm tuân thủ vs 2 tuần baseline (lagging outcome) | CPA baseline mỗi shop tự đo 2 tuần trước onboarding | **−10% → −20%** | Meta Ads Insights + DB đơn hàng | AI Lead + Finance BP | CPA giảm vì shop tự tắt campaign yếu, không phải nhờ AI | So sánh **cohort tuân thủ vs cohort không tuân thủ** cùng segment; theo dõi attribution của khuyến nghị được Accept |

### B.2 Metric theo workflow

#### Workflow 1 — Kết nối dữ liệu Meta Ads + đơn hàng

| Layer | Metric | Baseline | Target | Data source | Owner | Red-team risk | Fix |
|---|---|---:|---:|---|---|---|---|
| Activation | % shop hoàn tất kết nối OAuth Meta + map đơn hàng trong onboarding **≤60 phút** | 0% | **≥80%** | Onboarding session log | Customer Success Lead | Tính cả case "kết nối nhưng đơn hàng map sai" | Yêu cầu QA mapping ≥95% trước khi gắn cờ "kết nối thành công" |
| Engagement | Số ngày/tuần dữ liệu ads + đơn được sync ổn định | n/a | **≥6/7 ngày** | Sync job monitor | Eng Lead | Sync chạy nhưng dữ liệu trễ T+2 không hữu ích cho khuyến nghị sáng | Cảnh báo nếu độ trễ >24h; tự fallback sang CSV Plan B |
| Productivity | Thời gian onboarding trung bình | ~3–5 ngày (manual, chưa có sản phẩm) | **≤60 phút** có hỗ trợ | Onboarding log | Customer Success Lead | Rút thời gian onboarding nhưng shop không hiểu dữ liệu nào được dùng | Bắt buộc bước "data preview" + đồng ý dùng dữ liệu trước khi kết thúc onboarding |
| Quality | % shop pilot có **≥7 ngày dữ liệu ads + đơn hàng đầy đủ** sau onboarding | n/a | **≥90%** | Data-readiness check (job daily) | AI Lead | Data có nhưng chất lượng kém (đơn hàng thiếu giá trị / thời điểm) | Score data-readiness theo 3 trục: đầy đủ trường, đủ ngày, không null > ngưỡng |
| Trust | % shop xác nhận hiểu "AI đang dùng dữ liệu gì" sau onboarding (qua quick survey 3 câu) | n/a | **≥85%** | In-app survey cuối onboarding | Customer Success Lead | Shop tick "hiểu" cho qua | Random sample 10%/tuần CS gọi lại xác minh thực tế |
| Value | % shop pilot nhận **khuyến nghị đầu tiên trong tuần 1** | 0% | **≥80%** | Recommendation engine log | Product Lead | Khuyến nghị đầu rỗng / generic vì data chưa đủ | Chỉ trigger khi data-readiness ≥ ngưỡng; nếu không, gửi "Watch mode + checklist bổ sung data" |

#### Workflow 2 — Xếp hạng ad set theo khả năng mua (propensity scoring)

| Layer | Metric | Baseline | Target | Data source | Owner | Red-team risk | Fix |
|---|---|---:|---:|---|---|---:|---|
| Activation | % shop mở bảng xếp hạng ≥1 lần trong tuần đầu | 0% | **≥80%** | Frontend event log | Product Lead | Mở nhưng không đọc | Đo thêm thời gian dwell-time trên bảng + click vào chi tiết ad set |
| Engagement | Median số lần mở bảng xếp hạng / shop / tuần | n/a | **≥3 lần/tuần** | Frontend event log | Product Lead | Vanity metric nếu shop mở nhưng không hành động | Ghép với Workflow 3 (acceptance rate khuyến nghị dựa trên xếp hạng) |
| Productivity | Thời gian shop ra quyết định "tắt/đẩy ad set" sau khi xem xếp hạng | 30–60 phút (đoán + so sánh thủ công) | **≤15 phút** | Time-on-task qua event log + survey | Product Lead | Nhanh hơn nhưng quyết định sai | Đối chiếu với Quality (precision xếp hạng) và Override rate |
| Quality | **Precision xếp hạng**: % ad set hạng "cao" thực sự có ROAS ≥ trung bình shop trong 7 ngày sau | n/a (pilot xác lập) | **≥65%** ở pilot, tăng dần | Meta Ads Insights + scoring log | AI Lead | Precision cao trên tập nhỏ, không generalize | Bắt buộc đo trên ≥30 shop × ≥3 cohort theo segment thời trang trước khi công bố |
| Trust | **Override rate**: % shop từ chối/sửa xếp hạng kèm lý do | n/a | **≤30%** (override cao hơn = trust thấp) | Action log | Product Lead | Override thấp vì shop "nhắm mắt Accept" | Đối chiếu Override rate với Quality precision: nếu cả hai cùng thấp = trust giả |
| Value | **Lift CPA** giữa nhóm Accept ≥70% xếp hạng tuần vs nhóm Accept <30% (cùng segment) | n/a | **−10% trở lên** sau 4 tuần | Meta Ads Insights + cohort analysis | AI Lead + Finance BP | Cohort thiên lệch (shop giỏi tự nhiên Accept nhiều hơn) | Stratify theo doanh thu/segment; chạy A/B chính thức ở tháng 2 pilot |

#### Workflow 3 — Khuyến nghị tối ưu ngân sách 24–48h (top-3 actions / sáng)

| Layer | Metric | Baseline | Target | Data source | Owner | Red-team risk | Fix |
|---|---|---:|---:|---|---|---|---|
| Activation | % shop nhận thẻ khuyến nghị sáng trong tuần 1 | 0% | **≥80%** | Notification + in-app log | Product Lead | Nhận nhưng không mở | Đo thêm open rate ≥70% và đếm khuyến nghị được xem >10 giây |
| Engagement | **≥60% shop áp dụng ≥2 khuyến nghị/tuần trong 3 tuần liên tiếp** (leading KR) | 0% | **≥60%** | Action log + Meta API confirm | Product Lead | Click Accept ≠ thực sự đẩy ngân sách | Cross-check với Meta API: ngân sách ad set thực sự thay đổi trong 24h |
| Productivity | Thời gian từ "đọc khuyến nghị" đến "ra quyết định" mỗi sáng | 30–60 phút (manual) | **≤15 phút** | Time-on-task | Product Lead | Quyết định nhanh nhưng burn rate tăng | Ghép Productivity với Value (burn rate <30%/48h) |
| Quality | **Acceptance rate khuyến nghị có giải thích** vs khuyến nghị không giải thích | n/a (H2 test) | Accept-with-explain **cao hơn ≥10 điểm %** | A/B test log | AI Lead | Giải thích "đẹp" nhưng không đúng nguyên nhân | Random audit 10%/tuần: AI Lead chấm "giải thích đúng/sai" |
| Trust | % khuyến nghị có **giải thích đầy đủ + confidence hiển thị** | 0% | **100%** | Recommendation engine log | AI Lead | Hiển thị 100% nhưng confidence sai hiệu chỉnh | Calibration check hằng tháng: confidence 70% phải đúng thực tế ~70% |
| Value | **Median CPA giảm ≥10%** sau 4 tuần ở nhóm tuân thủ vs baseline 2 tuần | CPA baseline shop | **−10% → −20%** | Meta Ads Insights | AI Lead + Finance BP | Giảm CPA nhờ shop tự cắt, không phải nhờ AI | Stratify theo cohort "Accept ≥2 khuyến nghị/tuần" vs control |

#### Workflow 4 — Watch mode + vòng Accept/Reject/Modify (human-in-the-loop)

| Layer | Metric | Baseline | Target | Data source | Owner | Red-team risk | Fix |
|---|---|---:|---:|---|---|---|---|
| Activation | % shop từng kích hoạt Watch mode (do confidence <70% hoặc CPA >150% target) trong 30 ngày đầu | n/a | **40–70%** (đủ để chứng minh guardrail hoạt động) | System log | Eng Lead | Watch mode bật quá hiếm = guardrail yếu | Stress test thủ công: tạo case CPA xấu nhân tạo, kiểm tra trigger |
| Engagement | Số lượt Accept/Reject/Modify được ghi nhận/shop/tuần | n/a | **≥10/tuần** | Action log | Product Lead | Modify nhưng không ghi lý do | Bắt buộc dropdown lý do tối thiểu khi Reject/Modify |
| Productivity | Thời gian phản ứng khi CPA xấu (từ lúc CPA vượt ngưỡng → có hành động đầu tiên) | 24–72h (theo pitch) | **≤24h** | Meta Insights + action log | Customer Success Lead | Phản ứng nhanh nhưng hành động sai | Đo kết hợp Quality (Watch-mode-correct rate) |
| Quality | **Watch-mode-correct rate**: % case Watch mode kích hoạt dẫn đến hành động bảo vệ thật (cắt/giảm ngân sách kịp) | n/a | **≥70%** | Action log + outcome 48h sau | AI Lead | Đo trên tập nhỏ, nhiễu | Audit hằng tuần ≥30 case ngẫu nhiên, có CS Lead chấm |
| Trust | % Reject/Modify **có lý do ghi nhận** + tỉ lệ lý do được sử dụng để cập nhật rule/prompt | 0% | **≥80% có lý do**; **≥1 rule update/tháng** | Action log + changelog model | AI Lead | Ghi lý do cho có, không thực sự đưa vào loop học | Báo cáo tuần "AI đã học được gì từ shop" gửi từng shop pilot |
| Value | **Tỉ lệ burn rate >30% trong 48h KHÔNG có cảnh báo trước** (incident rate) | Hiện tại ~15–25% các đợt biến động không kịp xử lý | **= 0** mỗi quý | Meta Insights + Watch mode log | Risk Owner (AI Lead) | Đặt mục tiêu = 0 quá lý tưởng, dễ giấu sự cố | Có post-mortem cho mọi incident; công bố nội bộ |

---

## 3. Part C — Dashboard Mock

Vẽ 6 ô: ô đầu là sức khoẻ toàn product, các ô sau theo workflow + quyết định.

```text
┌──────────────────────────────────────┐ ┌──────────────────────────────────────┐
│ PRODUCT HEALTH                       │ │ WORKFLOW 1 — DATA READINESS          │
│ Metric: % shop pilot Accept ≥2       │ │ Metric: % shop có ≥7 ngày data        │
│   khuyến nghị/tuần × 3 tuần          │ │   ads + đơn hàng đầy đủ              │
│ Current: __% Target: ≥60%            │ │ Current: __% Target: ≥90%            │
│ Status: Green / Amber / Red          │ │ Status: Green / Amber / Red          │
│ Action if red: Onboarding deep-dive  │ │ Action if red: Fallback CSV Plan B + │
│   + CS call + thu hẹp scope khuyến   │ │   chặn khuyến nghị tới khi đủ data   │
│   nghị tới khi engagement phục hồi   │ │                                      │
└──────────────────────────────────────┘ └──────────────────────────────────────┘

┌──────────────────────────────────────┐ ┌──────────────────────────────────────┐
│ WORKFLOW 2 — SCORING QUALITY         │ │ WORKFLOW 3 — RECOMMENDATION VALUE    │
│ Metric: Precision xếp hạng "cao"     │ │ Metric: Median CPA giảm 4 tuần       │
│   (ROAS ≥ tb shop trong 7 ngày)      │ │   ở nhóm tuân thủ vs baseline 2 tuần │
│ Current: __% Target: ≥65%            │ │ Current: __% Target: −10% → −20%     │
│ Status: Green / Amber / Red          │ │ Status: Green / Amber / Red          │
│ Action if red: Hạ ngưỡng hiển thị    │ │ Action if red: Pause khuyến nghị     │
│   "cao", tăng tỉ trọng rule-based,   │ │   ngân sách lớn, chuyển sang Watch    │
│   re-train với cohort mới            │ │   mode + audit 10 case gần nhất       │
└──────────────────────────────────────┘ └──────────────────────────────────────┘

┌──────────────────────────────────────┐ ┌──────────────────────────────────────┐
│ WORKFLOW 4 — TRUST & SAFETY          │ │ DECISION                             │
│ Metric: Incident rate — burn >30%/   │ │ Continue / Pivot / Kill: CONTINUE    │
│   48h KHÔNG có cảnh báo trước        │ │   WITH GUARDRAILS                    │
│ Current: __ ca/quý Target: = 0       │ │ Metric mạnh nhất: Adoption × Value   │
│ Status: Green / Amber / Red          │ │   = ≥60% Accept ≥2/tuần × CPA −10%   │
│ Action if red: Post-mortem 48h, mở   │ │ Before scale: data-readiness ≥90%,   │
│   rộng Watch-mode trigger, freeze     │ │   precision ≥65%, LTV/CAC >3x cohort │
│   khuyến nghị mức "high confidence"  │ │   thật, incident rate = 0 ≥1 quý     │
└──────────────────────────────────────┘ └──────────────────────────────────────┘
```

---

## 4. Part D — Decision Memo

```markdown
# Decision Memo — SmartAds AI (Pilot Phase, 30 shop / 12 tháng)

1. Khuyến nghị: **continue with guardrails**.
   Tiếp tục pilot 30 shop trong 60–90 ngày với scope NOW (workflow 1–4),
   chưa scale ra agency-led growth cho tới khi pass đủ điều kiện ở mục 4.

2. Metric mạnh nhất để bảo vệ quyết định là **tổ hợp adoption × outcome × trust**:
   - **≥60% shop pilot Accept ≥2 khuyến nghị/tuần trong 3 tuần liên tiếp** (leading)
   - **Median CPA giảm ≥10% sau 4 tuần** ở nhóm tuân thủ vs baseline (lagging)
   - **Override rate ≤30% và Incident rate (burn >30%/48h không cảnh báo) = 0** (trust)
   Lý do: một mình adoption là vanity (giống Watson đếm case); một mình CPA có thể
   do shop tự cắt; chỉ tổ hợp cả 3 mới chứng minh AI thực sự tạo giá trị.

3. Metric hoặc giả định đã sửa sau red-team là:
   V1: "≥60% shop áp dụng ≥2 khuyến nghị/tuần" — đếm click Accept.
   V2: "≥60% shop Accept ≥2 khuyến nghị/tuần × có thay đổi ngân sách thực tế
       trong 24h (Meta API cross-check) × CPA cohort tuân thủ giảm ≥10%".
   Vì sao sửa này tốt hơn: loại được vanity "click cho có"; ép phải có outcome
   tài chính đi kèm; tách cohort để bảo vệ trước câu hỏi "có thật là nhờ AI?".

   V1 (Workflow 1): "% shop kết nối thành công" — chỉ đếm OAuth.
   V2: "% shop có ≥7 ngày dữ liệu ads + đơn hàng đầy đủ qua data-readiness check".
   Vì sao tốt hơn: bài học từ Watson — không được dừng ở input metric;
   readiness của dữ liệu phải là điều kiện bật đèn xanh trước khi tính ROI.

4. Trước khi scale, bạn phải:
   1. Đạt **data-readiness ≥90%** + **precision xếp hạng ≥65%** trên ≥30 shop pilot
      qua ít nhất 3 cohort segment (thời trang nữ, doanh thu khác nhau).
   2. Xác nhận **LTV/CAC > 3x** và **CAC payback ≤ 4 tháng** trên cohort pilot thật
      (không chỉ giả định trong file Excel), tách theo kênh community vs agency.
   3. **Incident rate = 0** (burn >30%/48h không cảnh báo) trong ≥1 quý liên tiếp,
      có post-mortem công khai cho mọi case Watch-mode-correct < 70%.
```

---

