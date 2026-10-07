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

Các lớp hệ thống (Layer): **UX** (giao diện, cách trình bày, luồng tương tác) · **Grounding** (dữ liệu, ngữ cảnh, nguồn tri thức) · **Safety** (guardrail, kiểm duyệt, quy trình giám sát) · **Model** (bản thân mô hình).

### Harm Map — Case 1: NEDA Tessa

| Mục | Nội dung |
|---|---|
| **Failure mode** | Đưa lời khuyên **có hại cho đúng nhóm người dùng mà hệ thống được tạo ra để bảo vệ**: khuyên giảm cân và đếm calo cho người rối loạn ăn uống; không phản hồi phù hợp khi người dùng bày tỏ cảm xúc tiêu cực về cơ thể. |
| **Layer khởi phát** | **Safety** (chính): không có guardrail chặn chủ đề giảm cân hoặc ăn kiêng; nhà cung cấp thay đổi hệ thống mà không có quy trình kiểm soát thay đổi (change management) và không kiểm thử lại trước khi phát hành. **Model** (phụ): thành phần sinh câu trả lời không còn bị giới hạn trong kịch bản đã được duyệt. |
| **Ai bị ảnh hưởng** | 📌 [Sự kiện] Người dùng thử như Sharon Maxwell đã nhận lời khuyên giảm cân. 📌 [Sự kiện] Nhân viên đường dây nóng bị cho nghỉ việc. 💭 [Nhận định] Người đang mắc hoặc có nguy cơ rối loạn ăn uống trong khoảng gần 70.000 người từng liên hệ đường dây nóng mỗi năm. |
| **Harm (thiệt hại)** | 📌 [Sự kiện] NEDA phải gỡ chatbot, bị truyền thông chỉ trích rộng rãi và mất uy tín. 💭 [Nhận định — **chưa có nguồn ghi nhận**] Người dùng dễ tổn thương có thể tái phát bệnh nếu làm theo lời khuyên; người đang khủng hoảng mất kênh hỗ trợ là con người. |
| **Mức độ / khả năng xảy ra** | Mức độ: **Cao**, vì đối tượng là nhóm dễ tổn thương và rối loạn ăn uống có thể đe dọa tính mạng. Khả năng: **Cao**, vì lỗi được phát hiện chỉ sau vài lần thử. |
| **Biện pháp giảm thiểu** | Danh sách chủ đề bị cấm (giảm cân, calo, BMI) kèm bộ kiểm thử red-team riêng cho nhóm rối loạn ăn uống; hợp đồng bắt buộc nhà cung cấp **thông báo và xin duyệt trước mọi thay đổi**; chạy lại bộ kiểm thử sau mỗi bản cập nhật; **không** dùng chatbot thay hoàn toàn kênh hỗ trợ do người trực. |
| **Human review** | Chuyên gia lâm sàng về rối loạn ăn uống **duyệt kịch bản và mọi thay đổi** trước khi phát hành; chatbot **chuyển ngay cho người tư vấn** khi phát hiện dấu hiệu khủng hoảng; đội ngũ chuyên môn định kỳ đọc lại mẫu hội thoại thật. |

### Harm Map — Case 2: ChatGPT và ngộ độc bromua

| Mục | Nội dung |
|---|---|
| **Failure mode** | Câu trả lời **đúng về mặt hóa học nhưng sai ngữ cảnh**: bromua có thể thay clorua trong một số ứng dụng như tẩy rửa, nhưng **không** dùng được trong thực phẩm. Mô hình không hỏi người dùng định dùng để làm gì và không cảnh báo độc tính. |
| **Layer khởi phát** | **Grounding** (chính): mô hình không xác định được ngữ cảnh "chế độ ăn của con người" và không đối chiếu với tri thức về an toàn thực phẩm. **Safety**: không có guardrail cho chủ đề "chất thay thế để ăn uống". **UX**: giao diện trò chuyện tạo cảm giác như đang được chuyên gia tư vấn, và lời cảnh báo chung chung rất dễ bị bỏ qua. |
| **Ai bị ảnh hưởng** | 📌 [Sự kiện] Người đàn ông 60 tuổi trong báo cáo ca bệnh. 💭 [Nhận định] Bất kỳ người dùng nào tự tìm lời khuyên dinh dưỡng trên chatbot đa dụng, đặc biệt là người có bệnh nền phải kiêng natri (tăng huyết áp, bệnh thận mạn). |
| **Harm (thiệt hại)** | 📌 [Sự kiện] Ngộ độc bromua (bromua máu 1.700 mg/L), rối loạn tâm thần, **nằm viện 3 tuần**. 💭 [Nhận định] Chi phí điều trị và nguy cơ để lại di chứng thần kinh; người dùng mất niềm tin vào thông tin sức khỏe do AI cung cấp. |
| **Mức độ / khả năng xảy ra** | Mức độ: **Rất cao**, có thể gây tử vong. Khả năng: **Trung bình**, vì cần người dùng tự làm theo nhiều bước, nhưng số người dùng chatbot rất lớn nên tổng rủi ro không nhỏ. |
| **Biện pháp giảm thiểu** | Khi câu hỏi liên quan đến ăn uống hoặc sức khỏe, mô hình **hỏi lại mục đích** trước khi trả lời; đối chiếu với danh sách chất không an toàn cho người dùng ăn uống; cảnh báo **cụ thể**, không chỉ ghi "ngữ cảnh rất quan trọng"; khuyến nghị người dùng hỏi bác sĩ hoặc chuyên gia dinh dưỡng khi muốn thay đổi chế độ ăn. |
| **Human review** | Chatbot đa dụng không thể có người duyệt từng câu trả lời. 💭 [Nhận định] Vì vậy, chức năng tư vấn dinh dưỡng cho người bệnh **phải** nằm trong sản phẩm chuyên biệt có chuyên gia dinh dưỡng duyệt trước, như cách P-110 đang làm. Với chatbot đa dụng, cần đội ngũ y khoa tham gia red-team định kỳ các chủ đề sức khỏe. |

### Harm Map — Case 3: Epic Sepsis Model

| Mục | Nội dung |
|---|---|
| **Failure mode** | **Hiệu năng sụt giảm khi áp dụng ở bệnh viện khác (dataset shift)** và không được kiểm chứng độc lập trước khi triển khai: vừa bỏ sót ca bệnh (false negative), vừa cảnh báo sai quá nhiều (false positive) dẫn tới **alert fatigue**. |
| **Layer khởi phát** | **Model** (chính): mô hình độc quyền, hiệu năng thực tế thấp hơn công bố và cách xây dựng không minh bạch. **Grounding**: dữ liệu và quy trình ghi chép ở từng bệnh viện khác với dữ liệu lúc huấn luyện. **UX**: cảnh báo cho 18% số bệnh nhân nằm viện làm loãng sự chú ý của bác sĩ. |
| **Ai bị ảnh hưởng** | 📌 [Sự kiện] Bệnh nhân nội trú tại Michigan Medicine: mô hình bỏ sót 67% ca sepsis trong mẫu nghiên cứu. 💭 [Nhận định] Bác sĩ và điều dưỡng chịu quá tải cảnh báo; bệnh nhân tại các bệnh viện khác dùng ESM mà chưa tự kiểm chứng. |
| **Harm (thiệt hại)** | 📌 [Sự kiện] Mô hình không phát hiện được 2/3 số ca sepsis; tạo ra rất nhiều cảnh báo sai (PPV 12%). 💭 [Nhận định — **nghiên cứu không đo trực tiếp hậu quả lâm sàng**] Có thể làm chậm điều trị kháng sinh, tăng nguy cơ tử vong, hoặc dẫn tới dùng kháng sinh không cần thiết. |
| **Mức độ / khả năng xảy ra** | Mức độ: **Rất cao**, vì sepsis là tình trạng đe dọa tính mạng và phụ thuộc vào thời gian điều trị. Khả năng: **Cao**, do mô hình được triển khai rộng ở nhiều bệnh viện. |
| **Biện pháp giảm thiểu** | **Kiểm chứng độc lập (external validation) tại từng bệnh viện** trước khi bật cảnh báo; nhà cung cấp phải minh bạch về hiệu năng; hiệu chỉnh ngưỡng cảnh báo theo từng bệnh viện; theo dõi độ trôi hiệu năng (drift) sau triển khai; đo tỷ lệ cảnh báo bị bỏ qua. |
| **Human review** | Bác sĩ luôn giữ quyền quyết định cuối cùng, cảnh báo chỉ là gợi ý. Hội đồng AI hoặc an toàn người bệnh của bệnh viện **duyệt trước khi triển khai** và **đánh giá định kỳ** (theo quý) độ nhạy, PPV và tỷ lệ bỏ qua cảnh báo. |

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
