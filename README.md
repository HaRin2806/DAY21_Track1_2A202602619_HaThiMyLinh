# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- Họ và tên: Hà Thị Mỹ Linh
- MSSV / mã học viên: 2A202602619
- Lớp: K04-L34-P2 · Track 1: AI Product Management
- Ngành đã chọn: **Y tế (Healthcare)** — trọng tâm là AI tư vấn sức khỏe/dinh dưỡng và AI hỗ trợ quyết định lâm sàng. Đây cũng là ngành của dự án nhóm **P-110 — AI Nutrition Agent** (tư vấn dinh dưỡng cho bệnh nhân tiểu đường, bệnh thận mạn, tim mạch, gout).

> **Quy ước ghi chú:** 📌 **[Sự kiện]** = thông tin đã được nguồn công bố (có link). 💭 **[Nhận định]** = phân tích hoặc suy luận của tôi, **không** phải hậu quả đã được ghi nhận.

### 1. Industry Risk Snapshot

| Nội dung | Đánh giá của tôi và lý do |
| --- | --- |
| Những tác hại chính có thể xảy ra | **Tổn hại sức khỏe thể chất/tinh thần** cho người bệnh, vì chẩn đoán sai, bỏ sót bệnh, điều trị chậm hoặc làm theo lời khuyên sai (ví dụ ngộ độc). **Phân biệt đối xử** trong tiếp cận chăm sóc khi mô hình bị thiên lệch. **Quá tải và mất niềm tin** của bác sĩ, điều dưỡng do cảnh báo sai (alert fatigue). **Thiệt hại uy tín và pháp lý** cho bệnh viện hoặc tổ chức triển khai. Người bị ảnh hưởng: bệnh nhân, người thân, người chăm sóc, nhân viên y tế, cơ sở y tế. |
| Mức độ high-stakes | **Cao.** Quyết định ảnh hưởng trực tiếp tới **tính mạng**, và nhiều tác hại **không đảo ngược được** (biến chứng, tử vong). Người dùng thường ở trạng thái dễ tổn thương (bệnh nặng, rối loạn tâm lý, người cao tuổi) và dễ tin vào thông tin nghe "có vẻ y khoa". |
| Dữ liệu nhạy cảm có thể được sử dụng | Hồ sơ bệnh án, chẩn đoán, bệnh nền, thuốc đang dùng, kết quả xét nghiệm, dị ứng, sức khỏe tâm thần, chỉ số cơ thể (cân nặng, BMI), ảnh bữa ăn, thông tin liên hệ của người bệnh và người chăm sóc. Tại Việt Nam, dữ liệu sức khỏe thuộc nhóm **dữ liệu cá nhân nhạy cảm** (Nghị định 13/2023/NĐ-CP; Luật Bảo vệ dữ liệu cá nhân 2025). *Bài này không dùng dữ liệu thật.* |
| Nhu cầu human review | **Cao.** **Ai kiểm tra:** bác sĩ hoặc chuyên gia dinh dưỡng có chuyên môn. **Ở bước nào:** (1) **trước** khi đầu ra mang tính chẩn đoán, điều trị hoặc thực đơn đến tay bệnh nhân; (2) **trước khi triển khai** mô hình, bằng cách kiểm chứng trên dữ liệu của chính cơ sở; (3) **sau mỗi lần thay đổi** model hoặc prompt và **định kỳ** sau triển khai; (4) **ngay lập tức** khi có dấu hiệu khẩn cấp hoặc khủng hoảng thì chuyển cho người thật. **Vì sao:** sai sót có thể gây hại không đảo ngược được, và AI không chịu trách nhiệm chuyên môn. |

---

### 2. Case study 1 — Chatbot "Tessa" của NEDA khuyên giảm cân cho người rối loạn ăn uống

#### Brief Case

- **Tổ chức / sản phẩm AI:** National Eating Disorders Association (NEDA), tổ chức phi lợi nhuận về rối loạn ăn uống của Mỹ. Chatbot *Tessa* do công ty Cass (trước đây là X2AI) cung cấp và tùy biến.
- **Thời gian, địa điểm / bối cảnh:** Mỹ. Tessa được triển khai lặng lẽ từ 02/2022. Tháng 5/2023, NEDA cho nhân viên đường dây nóng nghỉ việc và dự định để Tessa thay thế đường dây nóng do người trực từ 01/06/2023.
- **AI được dùng để làm gì:** chatbot hỗ trợ phòng ngừa và cung cấp thông tin cho người có nguy cơ rối loạn ăn uống.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - Chuyên gia tư vấn Sharon Maxwell thử Tessa và được khuyên **giảm cân, đếm calo, đo mỡ cơ thể**, tức là những hành vi có thể làm rối loạn ăn uống nặng hơn.
  - Bot cũng không phản hồi phù hợp với các câu như "I hate my body".
  - NEDA **gỡ Tessa ngày 30/05/2023**. Theo NEDA, nhà cung cấp Cass đã **cập nhật mã của Tessa mà NEDA không biết**.
- **Số liệu có nguồn:**
  - Đường dây nóng của NEDA được **gần 70.000 người** sử dụng trong năm trước sự kiện (NPR, 31/05/2023).
  - Tessa khuyên người thử giảm **1–2 pound/tuần (khoảng 0,5–1 kg/tuần)** (CNN, 01/06/2023).
  - Tessa bị gỡ ngày **30/05/2023**, **2 ngày** trước ngày dự kiến thay thế hoàn toàn đường dây nóng.
- **Nguồn:**
  - *National Eating Disorders Association phases out human helpline, pivots to chatbot* — NPR — 31/05/2023 — https://www.npr.org/sections/health-shots/2023/05/31/1179244569/national-eating-disorders-association-phases-out-human-helpline-pivots-to-chatbo
  - NPR transcript — https://www.npr.org/transcripts/1177847298 (mục NEDA cho biết Cass đã cập nhật mã mà NEDA không biết)
  - *NEDA takes its AI chatbot offline after complaints of harmful advice* — CNN Business — 01/06/2023 — https://krdo.com/money/cnn-business-consumer/2023/06/01/national-eating-disorders-association-takes-its-ai-chatbot-offline-after-complaints-of-harmful-advice/
  - AI Incident Database — Report 3141 — https://incidentdatabase.ai/reports/3141
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** Tessa đã đưa lời khuyên giảm cân và đếm calo; NEDA gỡ bot; theo NEDA, nhà cung cấp đã thay đổi bot mà không báo.
  - 💭 **Tôi suy luận / chưa rõ:** chưa có nguồn ghi nhận người dùng cụ thể nào **bị tái phát bệnh** vì Tessa. Số người đã nhận câu trả lời có hại chưa được công bố. Chi tiết kỹ thuật của bản cập nhật (có phải thành phần AI tạo sinh hay không) chỉ được nêu qua phát ngôn của NEDA, chưa được kiểm chứng độc lập.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Người đang mắc hoặc có nguy cơ rối loạn ăn uống hỏi Tessa cách cải thiện sức khỏe hoặc chia sẻ cảm xúc tiêu cực về cơ thể, và bot khuyên giảm cân, đếm calo thay vì hỗ trợ hoặc chuyển cho người tư vấn. |
| Stakeholder bị ảnh hưởng | Người dùng có rối loạn ăn uống; người thân của họ; NEDA (tổ chức triển khai); nhân viên đường dây nóng bị thay thế; nhà cung cấp Cass. |
| Failure mode | **Harmful advice** (lời khuyên có hại cho đúng nhóm người dùng mục tiêu) kèm **Escalation failure** (không chuyển sang người thật khi người dùng có dấu hiệu khủng hoảng). |
| Layer bắt đầu lỗi | **Safety + Model.** *Safety:* không có guardrail chặn chủ đề giảm cân, calo, BMI; không có quy trình kiểm soát thay đổi để nhà cung cấp phải xin duyệt và kiểm thử lại trước khi cập nhật. *Model:* theo NEDA, sau bản cập nhật bot trả lời ngoài kịch bản đã duyệt. Chưa đủ bằng chứng công khai để xác định chính xác thay đổi kỹ thuật là gì. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** Người thử nhận lời khuyên giảm 0,5–1 kg/tuần và đếm calo; NEDA phải gỡ bot và bị chỉ trích rộng rãi. 💭 **[Nguy cơ, chưa ghi nhận]** Người bệnh làm theo có thể tái phát hoặc bệnh nặng hơn; người đang khủng hoảng mất kênh hỗ trợ là con người. |
| Harm lens | **Injury** (sức khỏe thể chất và tinh thần); phụ: **Trust loss** (niềm tin vào tổ chức hỗ trợ). |
| Severity | **Critical.** Rối loạn ăn uống có thể đe dọa tính mạng, và lời khuyên đi ngược hẳn mục tiêu điều trị. |
| Scale | **Medium.** Đường dây nóng phục vụ khoảng 70.000 người/năm, và Tessa được dự định thay thế kênh này. Một thay đổi của nhà cung cấp ảnh hưởng **cùng lúc** tới mọi người dùng, nhưng số người đã thực sự dùng Tessa chưa được công bố. |
| Probability | **High.** Lỗi bị phát hiện chỉ sau vài lượt thử của một người dùng, tức là xảy ra dễ dàng chứ không phải trường hợp hiếm. |
| Frequency | **Medium.** Không phải phiên chat nào cũng hỏi về cân nặng, nhưng với nhóm người dùng rối loạn ăn uống thì chủ đề cân nặng và ăn uống xuất hiện thường xuyên. |
| Vì sao? | Nhóm người dùng dễ tổn thương, lời khuyên sai lại đúng vào hành vi bệnh lý, và bot được định làm **kênh thay thế** cho người thật nên không còn lớp con người phía sau. **Giới hạn bằng chứng:** đánh giá Scale và Frequency là suy luận của tôi, vì NEDA chưa công bố số phiên chat hoặc số người bị ảnh hưởng. |

---

### 3. Case study 2 — ChatGPT gợi ý thay muối ăn bằng natri bromua, người dùng ngộ độc bromua

#### Brief Case

- **Tổ chức / sản phẩm AI:** ChatGPT của OpenAI, một chatbot đa dụng **không** được thiết kế cho mục đích y tế. Các tác giả báo cáo ca bệnh cho rằng người bệnh nhiều khả năng đã dùng GPT-3.5 hoặc GPT-4.0.
- **Thời gian, địa điểm / bối cảnh:** Mỹ (University of Washington, Seattle). Ca bệnh được công bố ngày 05/08/2025. Một người đàn ông 60 tuổi đọc về tác hại của muối ăn (natri clorua) nên muốn loại hẳn clorua khỏi chế độ ăn.
- **AI được dùng để làm gì:** người dùng tự hỏi ChatGPT nên dùng chất gì thay thế clorua.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - Từ câu trả lời của ChatGPT, người này biết bromua có thể thay clorua, rồi mua **natri bromua** để dùng thay muối ăn.
  - Ông nhập viện với hoang tưởng, ảo giác, mất ngủ, mụn ở mặt và mất điều hòa vận động; được chẩn đoán **ngộ độc bromua (bromism)**.
  - Khi các bác sĩ tự hỏi lại ChatGPT 3.5, câu trả lời **vẫn có bromua**, chỉ ghi chung chung "ngữ cảnh rất quan trọng", **không cảnh báo cụ thể về sức khỏe** và **không hỏi người dùng định dùng để làm gì**.
- **Số liệu có nguồn:**
  - Dùng natri bromua thay muối trong **3 tháng**.
  - Nồng độ bromua máu **1.700 mg/L**, trong khi khoảng tham chiếu là **0,9–7,3 mg/L**, tức cao hơn mức trên khoảng **230 lần**.
  - Nằm viện **3 tuần**.
  - Nguồn: báo cáo ca bệnh trên Annals of Internal Medicine: Clinical Cases, 2025.
- **Nguồn:**
  - *A Case of Bromism Influenced by Use of Artificial Intelligence* — Eichenberger A., Thielke S., Van Buskirk A. — Annals of Internal Medicine: Clinical Cases — 05/08/2025 — https://doi.org/10.7326/aimcc.2024.1260
  - *Man sought diet advice from ChatGPT and ended up with "bromism"* — Live Science — 08/2025 — https://www.livescience.com/health/food-diet/man-sought-diet-advice-from-chatgpt-and-ended-up-with-bromide-intoxication
  - *Man who asked ChatGPT about cutting out salt was hospitalized with hallucinations* — NBC News — 08/2025 — https://www.nbcnews.com/tech/tech-news/man-asked-chatgpt-cutting-salt-diet-was-hospitalized-hallucinations-rcna225055
  - AI Incident Database — Report 6092 — https://incidentdatabase.ai/reports/6092
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** người bệnh ngộ độc bromua sau khi tham khảo ChatGPT; nồng độ bromua máu 1.700 mg/L; nằm viện 3 tuần; khi hỏi lại, ChatGPT 3.5 vẫn nhắc tới bromua mà không cảnh báo cụ thể.
  - 💭 **Chưa rõ:** các bác sĩ **không xem được nguyên văn cuộc hội thoại** của người bệnh, nên không biết chính xác câu hỏi và câu trả lời ban đầu. Người dùng cũng có thể đã hiểu sai ngữ cảnh (bromua thay clorua trong mục đích khác, như tẩy rửa). Đây là **một ca được ghi nhận**, chưa đủ để ước lượng tần suất.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Người dùng hỏi chatbot đa dụng nên dùng chất gì thay clorua (muối ăn) trong chế độ ăn, và bot nêu bromua mà không hỏi lại mục đích, không cảnh báo độc tính, không khuyên gặp bác sĩ. |
| Stakeholder bị ảnh hưởng | Người dùng trực tiếp; gia đình; bệnh viện và đội điều trị (chi phí, nguồn lực); nhà phát triển chatbot (uy tín, pháp lý); về sau là người bệnh có bệnh nền tự tìm lời khuyên dinh dưỡng trên chatbot. |
| Failure mode | **Harmful advice:** câu trả lời **đúng về mặt hóa học nhưng sai ngữ cảnh** ăn uống. Kèm **Escalation failure:** không hỏi lại, không chuyển người dùng tới chuyên gia y tế. |
| Layer bắt đầu lỗi | **Grounding + Safety**, phụ là **UX.** *Grounding:* không xác định được ngữ cảnh "chế độ ăn của con người" và không đối chiếu với tri thức về an toàn thực phẩm. *Safety:* thiếu guardrail cho chủ đề "chất thay thế để ăn uống". *UX:* giao diện trò chuyện tạo cảm giác như đang được chuyên gia tư vấn, còn lời nhắc "ngữ cảnh rất quan trọng" quá chung chung nên dễ bị bỏ qua. Vì không có nguyên văn hội thoại, chưa đủ bằng chứng để loại trừ việc người dùng hiểu sai. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** Người đàn ông 60 tuổi bị ngộ độc bromua (1.700 mg/L), có biểu hiện loạn thần và nằm viện 3 tuần. 💭 **[Nguy cơ, chưa ghi nhận]** Người có bệnh nền như suy thận hỏi về "muối thay thế" có thể được gợi ý muối kali, gây nguy cơ tăng kali máu, vì chatbot đa dụng không biết hồ sơ bệnh của họ. |
| Harm lens | **Injury**; phụ: **Misinformation** (thông tin sức khỏe sai ngữ cảnh). |
| Severity | **Critical.** Ngộ độc bromua gây loạn thần và phải nằm viện dài ngày, có thể nguy hiểm tính mạng. |
| Scale | **Low** với sự kiện đã ghi nhận (1 ca); **Medium–High** với nguy cơ, vì chatbot đa dụng có lượng người dùng rất lớn và câu hỏi về sức khỏe, dinh dưỡng rất phổ biến (nhận định của tôi). |
| Probability | **Low–Medium.** Để gây hại cần chuỗi nhiều bước: hỏi đúng kiểu câu, hiểu sai, tự mua hóa chất và dùng lâu dài. Tuy nhiên câu trả lời có vấn đề **lặp lại được** khi các bác sĩ thử lại. |
| Frequency | **Low** với hậu quả nặng như ca này; câu trả lời thiếu ngữ cảnh về sức khỏe thì có thể gặp **thường xuyên hơn** (nhận định, chưa có số liệu). |
| Vì sao? | Hậu quả rất nặng nhưng cần nhiều bước mới xảy ra, nên Severity cao trong khi Probability và Frequency thấp hơn. Căn cứ chính là báo cáo ca bệnh đã qua bình duyệt. **Giới hạn bằng chứng:** chỉ có 1 ca, không có nguyên văn hội thoại, và mô hình ChatGPT đã được cập nhật nhiều lần từ đó. |

---

### 4. Case study 3 — Epic Sepsis Model bỏ sót 2/3 ca nhiễm khuẩn huyết

#### Brief Case

- **Tổ chức / sản phẩm AI:** Epic Systems, với Epic Sepsis Model (ESM) là mô hình dự đoán độc quyền tích hợp sẵn trong hệ thống bệnh án điện tử Epic, được **hàng trăm bệnh viện** tại Mỹ sử dụng.
- **Thời gian, địa điểm / bối cảnh:** Michigan Medicine (University of Michigan), Mỹ. Dữ liệu từ 12/2018 đến 10/2019; nghiên cứu công bố ngày 21/06/2021.
- **AI được dùng để làm gì:** tự động cảnh báo sớm cho bác sĩ khi bệnh nhân nội trú có nguy cơ nhiễm khuẩn huyết (sepsis), để điều trị kháng sinh kịp thời.
- **Vấn đề hoặc sự kiện đáng chú ý:** khi được kiểm chứng độc lập (external validation), mô hình cho kết quả **kém hơn nhiều so với công bố của nhà cung cấp**. Nó vừa bỏ sót phần lớn ca sepsis, vừa phát rất nhiều cảnh báo sai, gây nguy cơ **alert fatigue**.
- **Số liệu có nguồn** (JAMA Internal Medicine, 2021):
  - Mẫu nghiên cứu: **27.697 bệnh nhân**, **38.455 lượt nhập viện**.
  - **AUC 0,63**, trong khi nhà cung cấp công bố 0,76–0,83.
  - Độ nhạy **33%**, PPV **12%**.
  - **Bỏ sót 67%** bệnh nhân sepsis nhưng **phát cảnh báo cho 18%** tổng số bệnh nhân nằm viện.
  - Chỉ phát hiện thêm **7%** số ca sepsis mà bác sĩ chưa kịp điều trị kịp thời.
- **Nguồn:**
  - *External Validation of a Widely Implemented Proprietary Sepsis Prediction Model in Hospitalized Patients* — Wong A. et al. — JAMA Internal Medicine — 21/06/2021 — https://jamanetwork.com/journals/jamainternalmedicine/fullarticle/2781313 (mục Results)
  - *Popular sepsis prediction tool less accurate than claimed* — Michigan Medicine — 06/2021 — https://michiganmedicine.org/health-lab/popular-sepsis-prediction-tool-less-accurate-claimed
  - *Popular sepsis prediction model works substantially worse than claimed* — MedCity News — 06/2021 — https://medcitynews.com/2021/06/popular-sepsis-prediction-model-works-substantially-worse-than-claimed-researchers-find
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** các chỉ số AUC, độ nhạy, PPV, tỷ lệ bỏ sót và tỷ lệ cảnh báo **tại một hệ thống bệnh viện** (Michigan Medicine).
  - 💭 **Tôi suy luận / chưa rõ:** nghiên cứu **không đo hậu quả lâm sàng** như tử vong hay điều trị chậm do mô hình. Kết quả có thể khác ở bệnh viện khác. Epic đã phản biện về cách chọn ngưỡng cảnh báo trong nghiên cứu.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Bệnh nhân nội trú đang tiến triển sepsis nhưng mô hình **không** phát cảnh báo; đồng thời bác sĩ phải nhận rất nhiều cảnh báo sai nên dần bỏ qua cả cảnh báo đúng. |
| Stakeholder bị ảnh hưởng | Bệnh nhân nội trú và gia đình; bác sĩ, điều dưỡng (quá tải cảnh báo); bệnh viện (trách nhiệm, chi phí); nhà cung cấp Epic (uy tín). |
| Failure mode | **Missed detection** (false negative) do **dataset shift** hoặc khả năng tổng quát hóa kém; kèm **false positive gây alert fatigue**; gốc rễ là **triển khai khi chưa kiểm chứng** và hiệu năng được công bố cao hơn thực tế. |
| Layer bắt đầu lỗi | **Model + Grounding**, phụ là **UX.** *Model:* mô hình độc quyền, ít minh bạch, hiệu năng thực tế thấp hơn công bố. *Grounding:* dữ liệu và cách ghi chép ở từng bệnh viện khác với dữ liệu lúc huấn luyện. *UX:* cảnh báo cho 18% bệnh nhân làm loãng sự chú ý của ê-kíp. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra, ở mức đo lường]** Tại Michigan Medicine, mô hình bỏ sót 67% ca sepsis, khoảng 88% cảnh báo là sai (PPV 12%), và cảnh báo phát cho 18% bệnh nhân. 💭 **[Nguy cơ, nghiên cứu không đo]** Điều trị kháng sinh có thể bị chậm, làm tăng nguy cơ tử vong; dùng kháng sinh không cần thiết; nhân viên y tế mất niềm tin vào hệ thống cảnh báo. |
| Harm lens | **Injury**; phụ: **Misinformation** (thông tin hiệu năng sai lệch khi ra quyết định triển khai). |
| Severity | **Critical.** Sepsis đe dọa tính mạng và rất phụ thuộc vào thời gian điều trị. |
| Scale | **High.** Mô hình chạy liên tục trên **mọi bệnh nhân nội trú** và được tích hợp ở **hàng trăm bệnh viện**; nghiên cứu riêng một hệ thống đã có 38.455 lượt nhập viện. |
| Probability | **High.** Độ nhạy chỉ 33% nghĩa là bỏ sót là kết quả thường gặp, không phải ngoại lệ. |
| Frequency | **High.** Mô hình đánh giá bệnh nhân liên tục mỗi ngày, nên lỗi bỏ sót và cảnh báo sai lặp lại hằng ngày. |
| Vì sao? | Các đánh giá dựa trên số liệu đo trực tiếp trong nghiên cứu JAMA đã qua bình duyệt, với cỡ mẫu lớn. **Giới hạn bằng chứng:** chỉ một hệ thống bệnh viện; không có số liệu về hậu quả lâm sàng; nhà cung cấp phản biện cách đặt ngưỡng cảnh báo. |

---

### 5. Tổng hợp và bài học cho dự án P-110

| | Case 1 — Tessa | Case 2 — ChatGPT/bromua | Case 3 — Epic Sepsis |
| --- | --- | --- | --- |
| Layer chính | Safety | Grounding | Model |
| Tác hại đã ghi nhận | Lời khuyên có hại; chatbot bị gỡ | Ngộ độc, nằm viện 3 tuần | Bỏ sót 67% ca sepsis |
| Bài học | Kiểm soát mọi thay đổi của nhà cung cấp và mô hình | Hỏi rõ ngữ cảnh và cảnh báo cụ thể | Kiểm chứng tại chỗ trước khi tin số liệu nhà cung cấp |

💭 **[Nhận định] Áp dụng cho P-110 — AI Nutrition Agent:**
1. **Human-in-the-loop bắt buộc** (từ Case 1 và 2): mọi thực đơn do agent sinh ra dừng ở trạng thái `PENDING_REVIEW` cho tới khi chuyên gia dinh dưỡng duyệt. Cần giữ nguyên tắc **không có đường đi nào bỏ qua bước duyệt**.
2. **Grounding và trích dẫn** (từ Case 2): cảnh báo dựa trên guideline (RAG) và có trích nguồn. Nên bổ sung danh sách **chất hoặc thực phẩm không an toàn** để chặn cứng các gợi ý thay thế nguy hiểm, không phụ thuộc vào LLM. Hồ sơ bệnh nền phải được đưa vào khi kiểm tra, để tránh trường hợp gợi ý muối kali cho người suy thận.
3. **Kiểm soát thay đổi model và prompt** (từ Case 1): mỗi lần đổi phiên bản LLM, prompt hoặc chỉ mục guideline, phải **chạy lại bộ eval an toàn** trước khi phát hành.
4. **Kiểm chứng trên dữ liệu thật trước khi mở rộng** (từ Case 3): eval trên hồ sơ demo **chưa đủ** để kết luận hệ thống an toàn với bệnh nhân thật. Cần thử nghiệm có giám sát và theo dõi tỷ lệ cảnh báo sai hoặc bị bỏ qua để tránh alert fatigue cho người chăm sóc.
5. **Dữ liệu nhạy cảm:** hồ sơ bệnh nền và thuốc đang dùng là dữ liệu sức khỏe nhạy cảm, nên cần xin đồng ý rõ ràng, giới hạn quyền truy cập và lưu nhật ký truy cập.
