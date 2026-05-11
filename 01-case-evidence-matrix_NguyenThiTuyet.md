# 01 — Case Evidence Matrix

**Học viên:** Nguyễn Thị Tuyết — **MSSV:** 2A202600215
**Ngày:** 11/05/2026

---

## Case 1 — Thành công: Morgan Stanley AI Assistant (GPT-4 cho Financial Advisors)

### 1. AI được dùng trong workflow nào?

| Workflow | Mô tả | Human-in-the-loop? |
|---|---|---|
| Tra cứu tri thức nội bộ | Advisor đặt câu hỏi nghiệp vụ → AI tìm và trích dẫn từ kho >100.000 tài liệu nội bộ (research, compliance, sản phẩm). | Có — Advisor kiểm tra nguồn trích dẫn trước khi dùng |
| Chuẩn bị meeting với khách hàng | AI tóm tắt thông tin khách hàng, tạo meeting notes, action items, draft email. | Có — Advisor chỉnh sửa/gửi theo quyết định riêng |
| Debrief sau meeting | AI @ Morgan Stanley Debrief tạo biên bản, action items và lưu vào Salesforce với sự đồng ý của client. | Có — Advisor kiểm tra trước khi lưu |

### 2. Họ đo metric gì?

| Metric | Số liệu công bố | Nguồn |
|---|---|---|
| Số lượng financial advisors | >16.000 advisors | Forbes / Tom Davenport |
| Quy mô knowledge base | >100.000 tài liệu nội bộ | Forbes / Tom Davenport |
| % advisors dùng AI chatbot | 98% advisors dùng (theo CDO Magazine) | CDO Magazine (phỏng vấn Atul Dalmia) |
| Thời gian tiết kiệm | 10–15 giờ/tuần cho financial advisers (potential saving) | Reuters (CEO Ted Pick) |
| Tính năng Debrief | Meeting notes, action items, draft email, lưu Salesforce | Morgan Stanley official press release |
| Evaluation framework | Có evals về summarization, accuracy, coherence | OpenAI case study |

### 3. Metric đó chứng minh được gì?

- **AI đã được adoption thực sự:** 98% advisors dùng — không chỉ là POC hay pilot.
- **AI được gắn vào workflow thật:** Không phải tool "để đó", advisor dùng để tra cứu, chuẩn bị meeting, tạo draft.
- **Có tín hiệu productivity rõ:** 10–15 giờ/tuần tiết kiệm là con số đủ lớn để tác động đến workflow của advisor.
- **Có cơ chế evaluation:** Morgan Stanley/OpenAI có hệ thống đánh giá accuracy, coherence — không deploy blind.
- **Human-in-the-loop được thiết kế sẵn:** Advisor kiểm tra trước khi gửi cho khách hàng, không auto-push.

### 4. Metric đó chưa chứng minh được gì?

| Chưa chứng minh được | Lý do |
|---|---|
| **Revenue / AUM tăng** | 10–15 giờ/tuần là productivity proxy, không đo trực tiếp doanh thu hoặc tài sản khách hàng tăng |
| **Client retention** | Không có số liệu công khai về việc khách hàng ở lại lâu hơn nhờ AI |
| **Client outcome / satisfaction** | Chưa thấy NPS hoặc CSAT nội bộ được công bố |
| **Citation accuracy % cụ thể** | OpenAI chỉ nói có evals, không công bố % accuracy |
| **Override rate** | Không biết bao nhiêu % output AI bị advisor sửa hoặc bỏ |
| **Audit độc lập** | Các số liệu đều từ Morgan Stanley, OpenAI hoặc báo chí — chưa có audit độc lập |

### 5. Còn thiếu metric nào?

| Thiếu metric | Vai trò nếu có |
|---|---|
| **Revenue uplift / advisor** | Chứng minh AI tác động lên business outcome |
| **Advisor override rate** | Đo trust: advisor có thực sự tin output AI không |
| **Client satisfaction (CSAT) theo advisor có dùng AI vs không** | So sánh cohort để chứng minh value |
| **Compliance error rate** | Đo rủi ro pháp lý khi dùng AI trong tài chính |
| **Cost per usable output** | ROI thực: chi phí AI trên mỗi output dùng được |


## Case 2 — Cảnh báo / Thất bại: IBM Watson for Oncology tại MD Anderson

### 1. AI được dùng trong workflow nào?

| Workflow | Mô tả | Human-in-the-loop? |
|---|---|---|
| Đọc và tóm tắt hồ sơ bệnh nhân | Watson dùng NLP để tóm tắt electronic health records (EHR) và tìm trong database để đưa ra treatment recommendations. | Không rõ — thiếu human-review chuẩn hóa |
| Đề xuất phác đồ điều trị | Watson gợi ý hướng điều trị ung thư dựa trên dữ liệu bệnh nhân và database y khoa. | Không tích hợp được vào EMR thật của MD Anderson |

### 2. Họ đo / công bố metric gì?

| Metric | Số liệu công bố | Nguồn |
|---|---|---|
| Thời gian & chi phí dự án | ~62 triệu USD, 5 năm (2012–2017) | JNCI / Oxford Academic |
| Trạng thái dự án | Bị hủy/dừng trước khi dùng trên bệnh nhân thật tại MD Anderson | JNCI / IEEE Spectrum |
| Kết quả training | Watson được train trên hypothetical cases, không phải hồ sơ thật | STAT News |
| Chất lượng khuyến nghị | Internal IBM documents cho thấy Watson từng đưa ra khuyến nghị "unsafe and incorrect" | STAT News (2018) |

### 3. Metric đó chứng minh được gì?

- **Đã có hoạt động và chi tiêu:** Dự án có đầu tư lớn (~62 triệu USD), có đội ngũ, có training.
- **Hệ thống có chạy về mặt kỹ thuật:** Watson có thể xử lý hồ sơ, tóm tắt, tìm database.
- **Có rủi ro rõ ràng về chất lượng khuyến nghị:** Internal documents chỉ ra unsafe recommendations.
- **Khoảng cách giữa kỳ vọng và thực tế là lớn:** Dù IBM quảng bá mạnh, deployment thật không xảy ra.

### 4. Metric đó chưa chứng minh được gì?

| Chưa chứng minh được | Lý do |
|---|---|
| **Giá trị lâm sàng (survival rate, response rate)** | Không có dữ liệu outcome trên bệnh nhân thật tại MD Anderson |
| **Adoption bởi bác sĩ** | Không có số liệu bác sĩ chấp nhận hay dùng Watson trong workflow thật |
| **Safety / incident rate** | Ngược lại, còn có bằng chứng về unsafe recommendations |
| **ROI tài chính** | Chỉ có chi phí (~62M), không có value |
| **Data readiness** | Train trên hypothetical cases — không chứng minh generalize được ra bệnh nhân thật |

### 5. Còn thiếu metric nào?

| Thiếu metric | Vai trò nếu có |
|---|---|
| **Doctor acceptance rate** | Đo trust: bác sĩ có accept khuyến nghị không |
| **Override / disagreement rate** | Đo quality: bác sĩ có sửa bao nhiêu % |
| **Clinical outcome metric** | Survival, response rate, adverse event rate |
| **Data readiness score** | % hồ sơ bệnh nhân đủ sạch, đủ cấu trúc để AI xử lý |
| **EMR integration success rate** | Hệ thống có chạy được trong infrastructure thật không |
| **Cost per usable recommendation** | Chi phí trên mỗi khuyến nghị dùng được |


## Nguồn tham khảo

**Morgan Stanley AI Assistant**
1. OpenAI (2023). *Morgan Stanley wealth management deploys GPT-4*. https://openai.com/index/morgan-stanley/
2. Morgan Stanley (2023). *AI @ Morgan Stanley Debrief Launch*. https://www.morganstanley.com/press-releases/ai-at-morgan-stanley-debrief-launch
3. Forbes / Tom Davenport. *How Morgan Stanley Is Training GPT*. https://www.forbes.com/sites/tomdavenport/2023/03/20/how-morgan-stanley-is-training-gpt-to-help-financial-advisors/
4. Reuters. *Morgan Stanley CEO says AI could save 10–15 hours/week*. https://www.reuters.com/technology/morgan-stanley-ceo-says-ai-could-save-financial-advisers-10-15-hours-week-2024-06-10/
5. CDO Magazine. *98% of Morgan Stanley Wealth Management Advisors Use Its AI Chatbot*. https://www.cdomagazine.tech/aiml/98-of-morgan-stanley-wealth-management-advisors-use-its-ai-chatbot-with-improved-productivity-cao-explains-how

**IBM Watson for Oncology / MD Anderson**
6. JNCI / Oxford Academic. *MD Anderson and IBM Watson*. https://academic.oup.com/jnci/article/109/5/djx113/3847623
7. IEEE Spectrum. *How IBM Watson Overpromised and Underdelivered*. https://spectrum.ieee.org/how-ibm-watson-overpromised-and-underdelivered-on-ai-health-care
8. STAT News. *IBM Watson recommended unsafe and incorrect treatments*. https://www.statnews.com/2018/07/25/ibm-watson-recommended-unsafe-incorrect-treatments/
9. Forbes. *MD Anderson Benches IBM Watson*. https://www.forbes.com/sites/matthewherper/2017/02/19/md-anderson-benches-ibm-watson-in-setback-for-artificial-intelligence-in-medicine/
