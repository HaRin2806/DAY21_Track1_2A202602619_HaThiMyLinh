# Day 21 — Track 1 · Bài cá nhân: Rủi ro AI trong ngành Y tế

| | |
|---|---|
| **Học viên** | Hà Thị Mỹ Linh — 2A202602619 |
| **Ngành chọn** | **Y tế (Healthcare)** — trọng tâm: AI tư vấn sức khỏe/dinh dưỡng và AI hỗ trợ quyết định lâm sàng |
| **Liên hệ dự án** | Dự án nhóm **P-110 — AI Nutrition Agent** (tư vấn dinh dưỡng cho bệnh nhân tiểu đường, bệnh thận mạn, tim mạch, gout) |
| **Nguồn yêu cầu** | Slide *AI Ethics, AI Safety & Responsible AI* — trang 31 (Lab Assignment, mục Cá nhân); Harm Map trang 22–25 |

> **Quy ước ghi chú:** 📌 **[Sự kiện]** = thông tin đã được nguồn công bố (có link). 💭 **[Nhận định]** = phân tích/suy luận của cá nhân em, *không* phải hậu quả đã được ghi nhận.

---

## 1. Industry Risk Snapshot — Ngành Y tế

Thang chấm: 1 (thấp) → 5 (rất cao).

| Tiêu chí | Mức | Giải thích ngắn |
|---|:-:|---|
| **Tác hại chính** | 5 | Tổn hại sức khỏe thể chất/tinh thần, chẩn đoán sai hoặc bỏ sót, điều trị chậm, ngộ độc do làm theo lời khuyên sai, phân biệt đối xử trong tiếp cận chăm sóc, mất niềm tin vào hệ thống y tế. |
| **Mức độ high-stakes** | 5 | Quyết định ảnh hưởng trực tiếp đến **tính mạng**; nhiều tác hại **không đảo ngược được** (biến chứng, tử vong). Người dùng thường ở trạng thái dễ tổn thương (bệnh nặng, rối loạn tâm lý, người cao tuổi) và có xu hướng tin vào thông tin nghe "có vẻ y khoa". |
| **Dữ liệu nhạy cảm** | 5 | Hồ sơ bệnh án, chẩn đoán, thuốc đang dùng, xét nghiệm, sức khỏe tâm thần, dữ liệu sinh trắc học. Tại Việt Nam, dữ liệu sức khỏe là **dữ liệu cá nhân nhạy cảm** (Nghị định 13/2023/NĐ-CP; Luật Bảo vệ dữ liệu cá nhân 2025). |
| **Nhu cầu human review** | 5 | **Bắt buộc** với mọi đầu ra mang tính chẩn đoán, điều trị hoặc kê thực đơn cho người bệnh. AI chỉ nên đóng vai **hỗ trợ**; bác sĩ hoặc chuyên gia dinh dưỡng duyệt **trước** khi kết quả đến tay bệnh nhân. Các tình huống khẩn cấp hoặc khủng hoảng phải chuyển cho người thật ngay. |

**Failure mode điển hình của ngành:** bịa hoặc trả lời sai kiến thức y khoa (hallucination); lời khuyên đúng về mặt hóa học nhưng sai ngữ cảnh lâm sàng; mô hình dự đoán kém khi áp dụng sang bệnh viện hoặc quần thể khác (dataset shift); thiên lệch do dùng biến đại diện (proxy) sai; alert fatigue; nhà cung cấp tự thay đổi mô hình mà không thông báo.

**Kết luận:** 💭 [Nhận định] Y tế thuộc nhóm **rủi ro cao nhất**. Hệ thống AI trong ngành này cần được kiểm chứng độc lập trước khi triển khai, giám sát liên tục sau triển khai, và có người chịu trách nhiệm chuyên môn cho từng đầu ra.

---

## 2. Brief Case

Ba case dưới đây cùng thuộc ngành y tế và trải dài từ AI mà người bệnh tự dùng (case 1–2) đến AI dùng trong bệnh viện (case 3).

### Case 1 — Chatbot "Tessa" của NEDA khuyên giảm cân cho người rối loạn ăn uống (Mỹ, 2023)

| | |
|---|---|
| **Hệ thống** | Chatbot *Tessa* của National Eating Disorders Association (NEDA), do công ty Cass (X2AI) cung cấp và tùy biến |
| **Mục đích** | Hỗ trợ phòng ngừa và cung cấp thông tin cho người có nguy cơ rối loạn ăn uống; dự kiến **thay thế đường dây nóng do người trực** |
| **Vấn đề** | Tessa khuyên người dùng giảm cân, đếm calo và đo mỡ cơ thể, tức là đúng những hành vi có thể làm rối loạn ăn uống nặng hơn |

**Số liệu và sự kiện:**
- 📌 Đường dây nóng của NEDA được **gần 70.000 người** sử dụng trong năm trước đó. NEDA cho toàn bộ nhân viên đường dây nóng nghỉ việc và dự định chuyển sang Tessa từ **01/06/2023** (NPR).
- 📌 Tessa được triển khai "lặng lẽ" từ **02/2022**. Người dùng thử là chuyên gia tư vấn Sharon Maxwell; Tessa khuyên chị giảm **0,5–1 kg (1–2 pound)/tuần**, rồi Maxwell công khai các câu trả lời này.
- 📌 NEDA **gỡ Tessa ngày 30/05/2023**, tức vài ngày trước khi đóng đường dây nóng do người trực.
- 📌 Theo NEDA, sau khi điều tra họ phát hiện nhà cung cấp Cass đã **cập nhật mã nguồn của Tessa mà NEDA không biết**, khiến chatbot đưa ra những câu trả lời không còn được kiểm soát.

**Nguồn:**
- NPR (31/05/2023) — [National Eating Disorders Association phases out human helpline, pivots to chatbot](https://www.npr.org/sections/health-shots/2023/05/31/1179244569/national-eating-disorders-association-phases-out-human-helpline-pivots-to-chatbo)
- NPR transcript — [npr.org/transcripts/1177847298](https://www.npr.org/transcripts/1177847298)
- CNN Business (01/06/2023) — [NEDA takes its AI chatbot offline after complaints of harmful advice](https://krdo.com/money/cnn-business-consumer/2023/06/01/national-eating-disorders-association-takes-its-ai-chatbot-offline-after-complaints-of-harmful-advice/)
- AI Incident Database — [Report 3141](https://incidentdatabase.ai/reports/3141)

### Case 2 — ChatGPT gợi ý thay muối ăn bằng natri bromua, người dùng nhập viện vì ngộ độc bromua (Mỹ, 2025)

| | |
|---|---|
| **Hệ thống** | ChatGPT (theo các bác sĩ báo cáo ca bệnh thì nhiều khả năng là GPT-3.5 hoặc 4.0) — chatbot đa dụng, **không** được thiết kế cho mục đích y tế |
| **Mục đích (người dùng)** | Một người đàn ông 60 tuổi muốn bỏ muối ăn (natri clorua) khỏi chế độ ăn và hỏi ChatGPT nên dùng chất gì thay thế |
| **Vấn đề** | ChatGPT nêu **bromua** là chất có thể thay cho clorua mà **không cảnh báo về sức khỏe** và không hỏi người dùng định dùng để làm gì. Người này mua natri bromua và dùng thay muối ăn |

**Số liệu và sự kiện:**
- 📌 Người bệnh dùng natri bromua thay muối ăn trong **3 tháng**, sau đó nhập viện với hoang tưởng, ảo giác, mất ngủ, mụn trứng cá ở mặt và mất điều hòa vận động; phải **nằm viện 3 tuần** (có giai đoạn bị giữ điều trị tâm thần).
- 📌 Nồng độ bromua trong máu là **1.700 mg/L**, trong khi khoảng tham chiếu là **0,9–7,3 mg/L**, tức cao hơn mức trên của khoảng tham chiếu khoảng **230 lần**.
- 📌 Các bác sĩ tự hỏi lại ChatGPT 3.5 và nhận được câu trả lời **vẫn có bromua**, chỉ ghi chung chung rằng "ngữ cảnh rất quan trọng", không cảnh báo cụ thể và không hỏi lý do như một nhân viên y tế sẽ hỏi.

**Nguồn:**
- Eichenberger A., Thielke S., Van Buskirk A. — *A Case of Bromism Influenced by Use of Artificial Intelligence*, Annals of Internal Medicine: Clinical Cases (05/08/2025), [DOI 10.7326/aimcc.2024.1260](https://doi.org/10.7326/aimcc.2024.1260)
- Live Science — [Man sought diet advice from ChatGPT and ended up with "bromism"](https://www.livescience.com/health/food-diet/man-sought-diet-advice-from-chatgpt-and-ended-up-with-bromide-intoxication)
- NBC News — [Man who asked ChatGPT about cutting out salt was hospitalized with hallucinations](https://www.nbcnews.com/tech/tech-news/man-asked-chatgpt-cutting-salt-diet-was-hospitalized-hallucinations-rcna225055)
- AI Incident Database — [Report 6092](https://incidentdatabase.ai/reports/6092)

### Case 3 — Epic Sepsis Model bỏ sót 2/3 ca nhiễm khuẩn huyết (Mỹ, 2021)

| | |
|---|---|
| **Hệ thống** | Epic Sepsis Model (ESM) — mô hình dự đoán nhiễm khuẩn huyết (sepsis) tích hợp sẵn trong hệ thống bệnh án điện tử Epic, được **hàng trăm bệnh viện** tại Mỹ sử dụng |
| **Mục đích** | Tự động cảnh báo sớm cho bác sĩ khi bệnh nhân nội trú có nguy cơ sepsis, để điều trị kháng sinh kịp thời |
| **Vấn đề** | Khi được đánh giá độc lập (external validation) tại Michigan Medicine, mô hình cho kết quả **kém hơn nhiều** so với công bố của nhà cung cấp: vừa bỏ sót nhiều ca bệnh, vừa cảnh báo sai rất nhiều |

**Số liệu và sự kiện:**
- 📌 Nghiên cứu hồi cứu trên **27.697 bệnh nhân** với **38.455 lượt nhập viện** (12/2018–10/2019).
- 📌 AUC chỉ đạt **0,63**, trong khi nhà cung cấp công bố 0,76–0,83. Độ nhạy **33%**, PPV **12%**.
- 📌 Mô hình **bỏ sót 67%** bệnh nhân sepsis nhưng vẫn **phát cảnh báo cho 18%** tổng số bệnh nhân nằm viện, gây nguy cơ **alert fatigue** (bác sĩ quen bỏ qua cảnh báo).
- 📌 Mô hình chỉ phát hiện thêm **7%** số ca sepsis mà bác sĩ chưa kịp điều trị kịp thời.

**Nguồn:**
- Wong A. et al. — *External Validation of a Widely Implemented Proprietary Sepsis Prediction Model in Hospitalized Patients*, JAMA Internal Medicine (21/06/2021), [jamanetwork.com/…/2781313](https://jamanetwork.com/journals/jamainternalmedicine/fullarticle/2781313)
- Michigan Medicine — [Popular sepsis prediction tool less accurate than claimed](https://michiganmedicine.org/health-lab/popular-sepsis-prediction-tool-less-accurate-claimed)
- MedCity News — [Popular sepsis prediction model works substantially worse than claimed](https://medcitynews.com/2021/06/popular-sepsis-prediction-model-works-substantially-worse-than-claimed-researchers-find)

---

## 3. Harm Map Worksheet

Bảng theo mẫu **Harm Map Worksheet — slide trang 25**. Mỗi case có 3 *high-risk moment*.

- **Layer:** UX · Grounding · Safety · Model.
- **Severity:** Low / Medium / High / Critical.
- **Scale, Probability, Frequency:** Low / Medium / High.
- **Nhãn trong cột "Harm xảy ra là gì?":** 📌 **[Sự kiện]** là hậu quả đã được nguồn ghi nhận; 💭 **[Nhận định]** là hậu quả có thể xảy ra theo đánh giá của em, chưa được ghi nhận.

### Harm Map — Case 1: NEDA Tessa

| High-risk moment | Stakeholder bị ảnh hưởng | Failure mode | Layer bắt đầu lỗi | Harm xảy ra là gì? | Harm lens | Severity | Scale | Probability | Frequency | Vì sao? |
|---|---|---|---|---|---|---|---|---|---|---|
| Người đang mắc hoặc có nguy cơ rối loạn ăn uống hỏi Tessa cách cải thiện sức khỏe, và bot khuyên giảm cân, đếm calo, đo mỡ cơ thể | Người dùng có rối loạn ăn uống; người thân; NEDA | Harmful advice | Safety + Model | 📌 [Sự kiện] Bot khuyên giảm 0,5–1 kg/tuần và đếm calo cho người thử (Sharon Maxwell). 💭 [Nhận định] Người bệnh làm theo có thể tái phát hoặc bệnh nặng hơn | Injury | Critical | Medium | High | Medium | Lời khuyên đi ngược hẳn mục tiêu điều trị của đúng nhóm người dùng mục tiêu. Lỗi bị phát hiện chỉ sau vài lượt thử, nên khả năng gặp phải là cao |
| Người dùng bày tỏ cảm xúc tiêu cực hoặc dấu hiệu khủng hoảng ("I hate my body") nhưng bot vẫn nói về ăn kiêng và tập luyện, không chuyển sang người thật | Người dùng đang khủng hoảng; người thân | Escalation failure | UX + Safety | 📌 [Sự kiện] Bot không phản hồi phù hợp với câu "I hate my body". 💭 [Nhận định] Người dùng bị chậm tiếp cận hỗ trợ, nhất là khi đường dây nóng do người trực sắp bị đóng | Injury | Critical | Medium | Medium | Low | Không phải phiên chat nào cũng có khủng hoảng, nhưng khi có thì hậu quả có thể đe dọa tính mạng, và lúc đó không còn kênh do con người trực để chuyển sang |
| Nhà cung cấp (Cass) cập nhật mã của Tessa mà NEDA không biết, khiến bot đưa ra câu trả lời không còn kiểm soát | NEDA; toàn bộ người dùng chatbot; nhân viên đường dây nóng bị cho nghỉ | Uncontrolled model change | Model + Safety | 📌 [Sự kiện] NEDA gỡ Tessa ngày 30/05/2023, bị truyền thông chỉ trích và mất uy tín. 💭 [Nhận định] Toàn bộ người dùng nhận câu trả lời chưa được duyệt trong thời gian bản cập nhật hoạt động | Misinformation / Trust loss | High | High | Medium | Low | Một thay đổi không được kiểm soát ảnh hưởng **cùng lúc** tới mọi người dùng (blast radius rộng). Việc cập nhật không thường xuyên, nhưng thiếu quy trình duyệt thay đổi thì sớm muộn cũng xảy ra |

**Giảm thiểu và human review:**
- Đặt guardrail chặn các chủ đề giảm cân, calo, BMI, kèm bộ kiểm thử red-team riêng cho nhóm rối loạn ăn uống.
- Hợp đồng yêu cầu nhà cung cấp **xin duyệt trước mọi thay đổi**; chạy lại bộ kiểm thử sau mỗi bản cập nhật.
- Chuyên gia lâm sàng duyệt kịch bản trước khi phát hành.
- Bot **chuyển ngay sang người tư vấn** khi phát hiện dấu hiệu khủng hoảng; không dùng chatbot để thay hoàn toàn kênh hỗ trợ do người trực.

### Harm Map — Case 2: ChatGPT và ngộ độc bromua

| High-risk moment | Stakeholder bị ảnh hưởng | Failure mode | Layer bắt đầu lỗi | Harm xảy ra là gì? | Harm lens | Severity | Scale | Probability | Frequency | Vì sao? |
|---|---|---|---|---|---|---|---|---|---|---|
| Người dùng hỏi chatbot nên dùng chất gì thay clorua (muối ăn) trong chế độ ăn, và bot gợi ý bromua mà không cảnh báo độc tính | Người dùng trực tiếp; gia đình; bệnh viện điều trị; nhà phát triển chatbot | Harmful advice (đúng về hóa học, sai ngữ cảnh ăn uống) | Grounding + Safety | 📌 [Sự kiện] Người đàn ông 60 tuổi dùng natri bromua 3 tháng, bromua máu 1.700 mg/L (bình thường 0,9–7,3), có hoang tưởng và ảo giác, nằm viện 3 tuần | Injury | Critical | Low | Low | Low | Hậu quả có thể gây tử vong. Đây là một ca được ghi nhận và cần người dùng tự thực hiện nhiều bước, nhưng khi kiểm tra lại, các bác sĩ vẫn nhận được câu trả lời có bromua, nên lỗi lặp lại được |
| Câu hỏi về sức khỏe hoặc dinh dưỡng nhưng bot không hỏi lại mục đích, không cảnh báo cụ thể, không khuyên gặp bác sĩ | Người dùng tự tìm lời khuyên sức khỏe trên chatbot đa dụng | Escalation failure | UX + Safety | 📌 [Sự kiện] Khi bác sĩ hỏi lại, câu trả lời chỉ ghi chung chung "ngữ cảnh rất quan trọng", không hỏi lý do. 💭 [Nhận định] Người dùng tự thử nghiệm trên cơ thể mình mà không có ai giám sát | Injury / Misinformation | High | High | Medium | Medium | Chatbot đa dụng có lượng người dùng rất lớn và câu hỏi sức khỏe rất phổ biến. Giao diện trò chuyện tạo cảm giác như đang được chuyên gia tư vấn, nên lời cảnh báo chung chung dễ bị bỏ qua |
| 💭 *Kịch bản giả định, liên hệ P-110:* bệnh nhân bệnh thận mạn hoặc tăng huyết áp hỏi chatbot "muối thay thế", và bot gợi ý muối kali mà không biết người dùng có bệnh nền | Bệnh nhân có bệnh nền; người chăm sóc | Harmful advice do thiếu ngữ cảnh hồ sơ bệnh | Grounding | 💭 [Nhận định — **chưa xảy ra, chưa có nguồn ghi nhận**] Nguy cơ tăng kali máu ở người suy thận, có thể gây rối loạn nhịp tim | Injury | Critical | Medium | Medium | Medium | Lời khuyên "giảm natri" đúng với người bình thường nhưng có thể nguy hiểm với người bệnh thận. Chatbot đa dụng không có hồ sơ bệnh nên không phân biệt được hai trường hợp này |

**Giảm thiểu và human review:**
- Câu hỏi về ăn uống hoặc sức khỏe thì bot **hỏi lại mục đích** trước khi trả lời.
- Đối chiếu với danh sách **chất không an toàn khi ăn uống** (chặn cứng, không phụ thuộc LLM).
- Cảnh báo **cụ thể**, không chỉ ghi "ngữ cảnh rất quan trọng".
- Chatbot đa dụng không thể có người duyệt từng câu trả lời. 💭 [Nhận định] Vì vậy, tư vấn dinh dưỡng cho người bệnh phải nằm trong sản phẩm chuyên biệt có hồ sơ bệnh và **chuyên gia dinh dưỡng duyệt trước**, như cách P-110 đang làm.

### Harm Map — Case 3: Epic Sepsis Model

| High-risk moment | Stakeholder bị ảnh hưởng | Failure mode | Layer bắt đầu lỗi | Harm xảy ra là gì? | Harm lens | Severity | Scale | Probability | Frequency | Vì sao? |
|---|---|---|---|---|---|---|---|---|---|---|
| Bệnh nhân nội trú đang tiến triển sepsis nhưng mô hình **không** phát cảnh báo (false negative) | Bệnh nhân; gia đình; bác sĩ; bệnh viện | Missed detection do dataset shift | Model + Grounding | 📌 [Sự kiện] Mô hình bỏ sót 67% ca sepsis (độ nhạy 33%) trong 38.455 lượt nhập viện. 💭 [Nhận định — **nghiên cứu không đo hậu quả lâm sàng**] Điều trị kháng sinh có thể bị chậm, làm tăng nguy cơ tử vong | Injury | Critical | High | High | High | Sepsis đe dọa tính mạng và phụ thuộc vào thời gian điều trị. Mô hình chạy liên tục trên mọi bệnh nhân nội trú ở nhiều bệnh viện, nên lỗi lặp lại hằng ngày |
| Mô hình cảnh báo cho 18% số bệnh nhân nằm viện, phần lớn là cảnh báo sai, và bác sĩ dần bỏ qua cảnh báo | Bác sĩ, điều dưỡng; bệnh nhân | False positive dẫn tới alert fatigue | UX + Model | 📌 [Sự kiện] PPV chỉ 12%, tức khoảng 88% cảnh báo là sai; cảnh báo phát cho 18% bệnh nhân. 💭 [Nhận định] Nhân viên y tế bỏ qua cả cảnh báo đúng; bệnh nhân có thể dùng kháng sinh không cần thiết | Injury / Resource loss | High | High | High | High | Cảnh báo sai liên tục làm giảm niềm tin vào mọi cảnh báo, và tác động này tích lũy theo thời gian trên toàn bộ ê-kíp |
| Bệnh viện bật mô hình dựa trên số liệu nhà cung cấp công bố, không tự kiểm chứng trên dữ liệu của mình | Bệnh viện; bệnh nhân; nhà cung cấp (Epic) | Unvalidated deployment / hiệu năng bị công bố cao hơn thực tế | Model + Safety | 📌 [Sự kiện] AUC thực tế 0,63 so với 0,76–0,83 do nhà cung cấp công bố. 💭 [Nhận định] Bệnh viện ra quyết định triển khai dựa trên thông tin hiệu năng sai lệch | Misinformation / Trust loss | High | High | Medium | Medium | Mô hình độc quyền, ít minh bạch và được tích hợp sẵn trong hệ thống bệnh án điện tử nên dễ được bật đại trà. Một sai lệch về hiệu năng lan ra nhiều bệnh viện cùng lúc |

**Giảm thiểu và human review:**
- Mỗi bệnh viện **tự kiểm chứng (external validation)** trên dữ liệu của mình trước khi bật cảnh báo.
- Hiệu chỉnh ngưỡng cảnh báo theo từng bệnh viện; theo dõi drift và tỷ lệ cảnh báo bị bỏ qua.
- Bác sĩ luôn giữ quyền quyết định cuối cùng, cảnh báo chỉ là gợi ý.
- Hội đồng AI hoặc an toàn người bệnh **duyệt trước khi triển khai** và **đánh giá theo quý** các chỉ số độ nhạy, PPV và tỷ lệ cảnh báo bị bỏ qua.

---

## 4. Tổng hợp và bài học cho dự án P-110

| | Case 1 — Tessa | Case 2 — ChatGPT/bromua | Case 3 — Epic Sepsis |
|---|---|---|---|
| Layer chính | Safety | Grounding | Model |
| Tác hại đã ghi nhận | Lời khuyên có hại; chatbot bị gỡ | Ngộ độc, nằm viện 3 tuần | Bỏ sót 67% ca sepsis |
| Bài học | Kiểm soát mọi thay đổi của nhà cung cấp và mô hình | Hỏi rõ ngữ cảnh và cảnh báo cụ thể | Kiểm chứng tại chỗ trước khi tin số liệu nhà cung cấp |

💭 **[Nhận định] Áp dụng cho P-110 — AI Nutrition Agent:**
1. **Human-in-the-loop bắt buộc** (từ Case 1 và 2): P-110 đã thiết kế để mọi thực đơn do agent sinh ra dừng ở trạng thái `PENDING_REVIEW` cho tới khi chuyên gia dinh dưỡng duyệt. Cần giữ nguyên tắc **không có đường đi nào bỏ qua bước duyệt**.
2. **Grounding và trích dẫn** (từ Case 2): cảnh báo dinh dưỡng của P-110 dựa trên guideline (RAG) và có trích nguồn. Nên bổ sung danh sách **chất hoặc thực phẩm không an toàn** để chặn cứng các gợi ý thay thế nguy hiểm, không phụ thuộc vào LLM.
3. **Kiểm soát thay đổi model và prompt** (từ Case 1): mỗi lần đổi phiên bản LLM (Gemini/OpenAI), prompt hoặc chỉ mục guideline, phải **chạy lại bộ eval an toàn** trước khi phát hành.
4. **Kiểm chứng trên dữ liệu thật trước khi mở rộng** (từ Case 3): kết quả eval trên hồ sơ demo `p_01`–`p_07` **chưa đủ** để kết luận hệ thống an toàn với bệnh nhân thật. Cần thử nghiệm có giám sát và theo dõi tỷ lệ cảnh báo sai hoặc bị bỏ qua để tránh alert fatigue cho người chăm sóc.
5. **Dữ liệu nhạy cảm**: hồ sơ bệnh nền và thuốc đang dùng là dữ liệu sức khỏe nhạy cảm, nên cần xin đồng ý rõ ràng, giới hạn quyền truy cập và lưu nhật ký truy cập.
