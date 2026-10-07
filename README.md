# Lab 21 — Phân tích rủi ro AI qua case study thực tế

- Họ và tên: Hà Thị Mỹ Linh
- MSSV / mã học viên: 2A202602619
- Lớp: K04-L34-P2 · Track 1: AI Product Management
- Ngành đã chọn: **Y tế / symptom checker / health assistant** — AI kiểm tra triệu chứng và trợ lý sức khỏe mà người dùng tương tác trực tiếp. Đây cũng là ngành của dự án nhóm **P-110 — AI Nutrition Agent** (trợ lý dinh dưỡng cho bệnh nhân tiểu đường, bệnh thận mạn, tim mạch, gout).

> **Quy ước ghi chú:** 📌 **[Sự kiện]** = thông tin đã được nguồn công bố (có link). 💭 **[Nhận định]** = phân tích hoặc suy luận của tôi, **không** phải hậu quả đã được ghi nhận.

### 1. Industry Risk Snapshot

*Các mức Thấp / Trung bình / Cao dưới đây là đánh giá định tính phục vụ bài tập, kèm căn cứ; không phải kết luận phân loại pháp lý.*

| Nội dung | Đánh giá của tôi và lý do |
| --- | --- |
| Những tác hại chính có thể xảy ra | **Tổn hại sức khỏe** khi người dùng làm theo lời khuyên sai hoặc thiếu ngữ cảnh (ví dụ ngộ độc, bệnh nặng hơn). **Phân loại sai mức khẩn cấp (triage):** báo "không khẩn cấp" với triệu chứng nguy hiểm khiến người dùng đi khám muộn, hoặc báo động quá mức khiến người dùng đi khám không cần thiết. **Thiên lệch:** kết quả khác nhau theo giới tính hoặc nhóm người dùng. **Thay thế người thật** ở các kênh hỗ trợ cho nhóm dễ tổn thương. **Mất niềm tin** và thiệt hại uy tín cho tổ chức triển khai. Người bị ảnh hưởng: người dùng hoặc bệnh nhân, người thân, người chăm sóc, cơ sở y tế. |
| Mức độ high-stakes | **Cao.** Người dùng ra quyết định về sức khỏe, như có đi khám hay không, ăn gì, dùng gì, **ngay sau khi** đọc câu trả lời và thường **không có nhân viên y tế ở giữa**. Một số tác hại không đảo ngược được (bỏ lỡ nhồi máu cơ tim, ngộ độc). Người dùng thường đang lo lắng hoặc dễ tổn thương, và có xu hướng tin vào câu trả lời nghe "có vẻ y khoa". |
| Dữ liệu nhạy cảm có thể được sử dụng | Triệu chứng tự khai, bệnh nền, thuốc đang dùng, dị ứng, kết quả xét nghiệm, sức khỏe tâm thần, chỉ số cơ thể (cân nặng, BMI), ảnh bữa ăn hoặc ảnh tổn thương da, lịch sử hội thoại, thông tin liên hệ. Tại Việt Nam, dữ liệu sức khỏe thuộc nhóm **dữ liệu cá nhân nhạy cảm** (Nghị định 13/2023/NĐ-CP). *Bài này không dùng dữ liệu thật.* |
| Nhu cầu human review | **Cao.** **Ai kiểm tra:** bác sĩ hoặc chuyên gia dinh dưỡng có chuyên môn. **Ở bước nào:** (1) **trước khi phát hành** — chuyên gia duyệt kịch bản, ngưỡng triage và bộ kiểm thử an toàn, kèm đánh giá độc lập với người dùng thật; (2) **sau mỗi lần thay đổi** model hoặc prompt; (3) **trước khi** kết quả cá nhân hóa như thực đơn hay kế hoạch điều trị đến tay người bệnh; (4) **ngay lập tức** khi có triệu chứng khẩn cấp hoặc dấu hiệu khủng hoảng thì chuyển cho người thật hoặc cấp cứu. **Vì sao:** người dùng hành động trực tiếp theo câu trả lời, và AI không chịu trách nhiệm chuyên môn. |

---

### 2. Case study 1 — Chatbot "Tessa" của NEDA khuyên giảm cân cho người rối loạn ăn uống

#### Brief Case

- **Tổ chức / sản phẩm AI:** National Eating Disorders Association (NEDA), tổ chức phi lợi nhuận về rối loạn ăn uống của Mỹ. Chatbot *Tessa* ban đầu là chatbot **theo kịch bản (rule-based)** do các chuyên gia rối loạn ăn uống (Dr. Barr Taylor, Dr. Ellen Fitzsimmons-Craft) xây dựng nội dung; công ty **Cass** vận hành nền tảng.
- **Thời gian, địa điểm / bối cảnh:** Mỹ, 2022–2023.
  - Tessa ra mắt lặng lẽ từ **02/2022**.
  - Ngày **31/03/2023**, NEDA báo cho nhân viên đường dây nóng rằng họ sẽ bị cho nghỉ việc. Theo kế hoạch, Tessa thay thế đường dây nóng do người trực từ khoảng **01/06/2023**.
- **AI được dùng để làm gì:** trợ lý sức khỏe dạng chatbot, cung cấp nội dung phòng ngừa rối loạn ăn uống và hỗ trợ người có nguy cơ.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - Cuối 05/2023, chuyên gia tư vấn **Sharon Maxwell** thử Tessa và được khuyên **giảm cân, đếm calo, tạo thâm hụt calo**, tức là những hành vi có thể làm rối loạn ăn uống nặng hơn.
  - NEDA **vô hiệu hóa Tessa vô thời hạn ngày 30/05/2023**.
  - Theo NPR, trong năm trước đó **Cass đã bổ sung AI tạo sinh (generative AI)**, giúp Tessa tự tạo câu trả lời mới ngoài kịch bản. NEDA cho rằng mình không được biết; **CEO Cass thì nói thay đổi này nằm trong hợp đồng với NEDA**.
  - NEDA đã nhận ảnh chụp màn hình phản ánh vấn đề của Tessa từ **10/2022**, tức nhiều tháng trước vụ Maxwell.
- **Số liệu có nguồn:**
  - **Gần 70.000 người** dùng đường dây nóng của NEDA trong năm trước đó. Đường dây nóng do **5–6 nhân viên được trả lương, 2 giám sát** và **90–165 tình nguyện viên** vận hành (NPR, 31/05/2023).
  - Tessa khuyên Maxwell giảm **1–2 pound/tuần (khoảng 0,5–0,9 kg)** bằng cách thâm hụt **500–1.000 calo/ngày** (NPR, 08/06/2023).
  - Mốc thời gian: Tessa bị vô hiệu hóa ngày **30/05/2023**, khoảng **2 ngày** trước thời điểm dự kiến thay đường dây nóng; vấn đề đã được báo cho NEDA khoảng **7 tháng** trước đó (10/2022) (NPR).
- **Nguồn:**
  - *National Eating Disorders Association phases out human helpline, pivots to chatbot* — Kate Wells, NPR — 31/05/2023 — https://www.npr.org/sections/health-shots/2023/05/31/1179244569/national-eating-disorders-association-phases-out-human-helpline-pivots-to-chatbo (bản chữ: https://text.npr.org/1179244569)
  - *An eating disorders chatbot offered dieting advice, raising fears about AI in health* — NPR — 08/06/2023 — https://www.kunc.org/npr-news/2023-06-08/an-eating-disorders-chatbot-offered-dieting-advice-raising-fears-about-ai-in-health (mục về Cass bổ sung generative AI và ảnh chụp màn hình từ 10/2022)
  - *NEDA takes its AI chatbot offline after complaints of harmful advice* — CNN Business — 01/06/2023 — https://krdo.com/money/cnn-business-consumer/2023/06/01/national-eating-disorders-association-takes-its-ai-chatbot-offline-after-complaints-of-harmful-advice/
  - AI Incident Database — Report 3141 — https://incidentdatabase.ai/reports/3141
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** Tessa đã khuyên giảm cân và thâm hụt calo cho người thử; NEDA vô hiệu hóa bot; Cass đã bổ sung AI tạo sinh; NEDA nhận phản ánh từ 10/2022.
  - 💭 **Chưa rõ / tôi suy luận:**
    - Ai chịu trách nhiệm cho thay đổi đang **còn tranh cãi**: NEDA nói không biết, Cass nói thay đổi nằm trong hợp đồng.
    - Chưa có nguồn ghi nhận người dùng cụ thể nào **bị tái phát bệnh** vì Tessa.
    - Số người đã nhận câu trả lời có hại chưa được công bố.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Người đang mắc hoặc có nguy cơ rối loạn ăn uống hỏi Tessa cách cải thiện sức khỏe hoặc chia sẻ cảm xúc tiêu cực về cơ thể, và bot khuyên giảm cân, đếm calo thay vì hỗ trợ hoặc chuyển cho người tư vấn. |
| Stakeholder bị ảnh hưởng | Người dùng có rối loạn ăn uống; người thân của họ; NEDA (tổ chức triển khai); nhân viên đường dây nóng bị thay thế; nhà cung cấp Cass. |
| Failure mode | **Harmful advice** (lời khuyên có hại cho đúng nhóm người dùng mục tiêu) kèm **Escalation failure** (không chuyển sang người thật khi người dùng có dấu hiệu khủng hoảng). |
| Layer bắt đầu lỗi | **Model + Safety.** *Model:* chatbot vốn chạy theo kịch bản đã được chuyên gia duyệt, nhưng Cass bổ sung AI tạo sinh nên bot tự sinh câu trả lời ngoài kịch bản (NPR). *Safety:* không có guardrail chặn chủ đề giảm cân, calo, BMI; không có quy trình kiểm thử lại sau khi đổi mô hình; NEDA đã nhận phản ánh từ 10/2022 nhưng không xử lý dứt điểm. Chưa đủ bằng chứng công khai về chi tiết kỹ thuật của bản cập nhật. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** Người thử nhận lời khuyên giảm 1–2 pound/tuần với mức thâm hụt 500–1.000 calo/ngày; NEDA phải vô hiệu hóa bot và bị chỉ trích rộng rãi. 💭 **[Nguy cơ, chưa ghi nhận]** Người bệnh làm theo có thể tái phát hoặc bệnh nặng hơn; người đang khủng hoảng mất kênh hỗ trợ là con người. |
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

### 4. Case study 3 — Symptom checker của Babylon Health được quảng bá "ngang bác sĩ" nhưng thiếu bằng chứng

#### Brief Case

- **Tổ chức / sản phẩm AI:** Babylon Health (Anh). Sản phẩm là symptom checker — AI chẩn đoán và phân loại mức khẩn cấp (*Babylon Diagnostic and Triage System*), tích hợp trong ứng dụng, trong đó có dịch vụ GP at Hand của NHS.
- **Thời gian, địa điểm / bối cảnh:** Vương quốc Anh và một số quốc gia khác, 2018–2019.
  - **06/2018:** Babylon công bố kết quả đánh giá tại Royal College of Physicians, London, khẳng định AI chẩn đoán "ngang bác sĩ".
  - **11/2018:** các nhà nghiên cứu độc lập phản biện kết quả này trên The Lancet.
- **AI được dùng để làm gì:** người dùng nhập triệu chứng; AI gợi ý bệnh có thể mắc và khuyên mức xử lý, như tự chăm sóc, gặp bác sĩ đa khoa hay đi cấp cứu.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - **Tuyên bố hiệu năng thiếu bằng chứng:** Babylon tuyên bố AI đạt điểm cao hơn mức đỗ trung bình của bác sĩ trong bài thi MRCGP và chính xác ngang bác sĩ. Fraser, Coiera và Wong viết trên The Lancet rằng nghiên cứu này "không đưa ra bằng chứng thuyết phục" rằng hệ thống tốt hơn bác sĩ trong tình huống thực tế, và "có khả năng nó kém hơn đáng kể". Lý do: dữ liệu được **bác sĩ nhập** chứ không phải người bệnh thật.
  - **Thiên lệch giới tính:** Undark (2019) ghi lại trường hợp hai hồ sơ giống hệt nhau, cùng triệu chứng đau ngực. Với hồ sơ **nữ**, app gợi ý trầm cảm hoặc cơn hoảng loạn; với hồ sơ **nam**, app gợi ý vấn đề tim mạch.
  - **Quảng cáo gây hiểu lầm:** Babylon từng bị cơ quan quản lý quảng cáo của Anh khiển trách. Royal College of General Practitioners và British Medical Association cũng lên tiếng nghi ngờ các tuyên bố của công ty.
- **Số liệu có nguồn:**
  - **Theo công bố của Babylon (06/2018):**
    - Trên **100 tình huống bệnh mô phỏng** (vignette), AI đạt độ chính xác **80%**, trong khi **7 bác sĩ** đối chứng đạt **64–94%**.
    - Với câu hỏi mẫu của kỳ thi MRCGP, AI đạt **81%**, so với mức đỗ trung bình **72%** của bác sĩ trong 5 năm trước đó.
  - **Theo Undark (12/2019):** symptom checker đã được dùng khoảng **1,7 triệu lượt** ở nhiều quốc gia, và nền tảng đã có khoảng **700.000 lượt tư vấn số** giữa bệnh nhân và bác sĩ.
  - Theo Undark (12/2019), khi đó **chưa có nghiên cứu ngẫu nhiên có đối chứng, đã bình duyệt** nào kiểm chứng hiệu năng của hệ thống trên bệnh nhân thật.
- **Nguồn:**
  - *Safety of patient-facing digital symptom checkers* — Fraser H., Coiera E., Wong D. — The Lancet 392(10161):2263–2264 — 24/11/2018 — https://eprints.whiterose.ac.uk/id/eprint/138305/
  - *Babylon AI achieves equivalent accuracy with human doctors* — thông cáo của Babylon, đăng lại trên BioSpectrum — 06/2018 — https://biospectrumindia.com/news/57/11202/babylon-ai-achieves-equivalent-accuracy-with-human-doctors-.html
  - *Review says Babylon's AI claims lack 'convincing evidence'* — Digital Health — 11/2018 — https://www.digitalhealth.net/2018/11/lancet-review-babylons-ai/
  - *Medical Advice From a Bot: The Unproven Promise of Babylon Health* — Undark — 09/12/2019 — https://undark.org/2019/12/09/babylon-health-artificial-intelligence-medical-advice/ (mục về thiên lệch giới tính, khiển trách quảng cáo, số lượt sử dụng)
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:**
    - Các con số 80%, 81% là **do Babylon tự công bố**.
    - Phản biện về phương pháp đã được đăng trên The Lancet.
    - Ví dụ về thiên lệch giới tính và việc bị khiển trách vì quảng cáo được Undark ghi lại.
  - 💭 **Chưa rõ / tôi suy luận:**
    - **Chưa có nguồn ghi nhận người dùng cụ thể nào bị tổn hại** vì triage sai của Babylon. Tác hại tôi phân tích dưới đây là **nguy cơ**.
    - Ví dụ thiên lệch giới tính là một phép thử đơn lẻ, chưa phải nghiên cứu có hệ thống về tỷ lệ sai lệch.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Một phụ nữ đau ngực nhập triệu chứng vào symptom checker, và app gợi ý trầm cảm hoặc cơn hoảng loạn thay vì cảnh báo nguy cơ tim mạch, nên người dùng không đi cấp cứu. Bối cảnh rộng hơn: người dùng và NHS tin vào tuyên bố "ngang bác sĩ" của nhà cung cấp. |
| Stakeholder bị ảnh hưởng | Người dùng app, đặc biệt là phụ nữ có triệu chứng tim mạch; người thân; NHS và bác sĩ đa khoa tiếp nhận ca chuyển tuyến; Babylon (uy tín, pháp lý). |
| Failure mode | **Bias / fairness** (triage khác nhau theo giới tính với cùng triệu chứng) kèm **Unsafe triage** (đánh giá thấp mức khẩn cấp); gốc rễ là **tuyên bố hiệu năng vượt quá bằng chứng** (overclaiming), chưa được kiểm chứng độc lập với người dùng thật. |
| Layer bắt đầu lỗi | **Model + Safety**, phụ là **UX.** *Model:* mô hình chẩn đoán có thể học thiên lệch từ dữ liệu, chẳng hạn bệnh tim ở nữ giới vốn hay bị chẩn đoán sót. *Safety:* thiếu đánh giá độc lập và nghiên cứu trên người dùng thật trước khi triển khai rộng (Lancet). *UX:* app trình bày kết quả như lời khuyên của bác sĩ, khiến người dùng tin và không đi khám. Chưa đủ bằng chứng công khai để xác định nguồn gốc kỹ thuật của thiên lệch. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** Có kết quả thiên lệch theo giới tính trong phép thử được Undark ghi lại; tuyên bố hiệu năng bị The Lancet phản biện; Babylon bị khiển trách vì quảng cáo gây hiểu lầm. 💭 **[Nguy cơ, chưa ghi nhận]** Phụ nữ bị nhồi máu cơ tim có thể đi cấp cứu muộn; người dùng và hệ thống y tế ra quyết định dựa trên hiệu năng bị thổi phồng. |
| Harm lens | **Injury**; phụ: **Dignity loss / fairness** (đối xử khác nhau theo giới tính) và **Misinformation** (thông tin hiệu năng sai lệch). |
| Severity | **Critical.** Bỏ lỡ nhồi máu cơ tim có thể gây tử vong, và cấp cứu tim mạch rất phụ thuộc vào thời gian. |
| Scale | **High.** Khoảng 1,7 triệu lượt dùng symptom checker ở nhiều quốc gia (Undark), có tích hợp trong dịch vụ của NHS. Một thiên lệch trong mô hình lặp lại với **mọi** người dùng cùng nhóm. |
| Probability | **Medium.** Sai lệch đã xuất hiện trong phép thử nhưng chưa có số liệu về tỷ lệ trên người dùng thật. Lancet cho rằng hệ thống *có thể* kém hơn bác sĩ đáng kể khi người dùng thật tự nhập. |
| Frequency | **Medium.** Đau ngực là triệu chứng phổ biến khi tra cứu, nhưng tình huống nguy hiểm thật (nhồi máu cơ tim) chỉ chiếm một phần. Đây là nhận định của tôi, chưa có số liệu. |
| Vì sao? | Symptom checker đứng **trước** bác sĩ: một lời khuyên "không khẩn cấp" sai có thể khiến người dùng không bao giờ gặp bác sĩ. Thiên lệch ở cấp mô hình lặp lại theo quy mô người dùng. **Giới hạn bằng chứng:** số liệu hiệu năng là Babylon tự công bố; ví dụ thiên lệch là phép thử đơn lẻ; không có số liệu tổn hại thực tế. |

---

### 5. Tổng hợp và bài học cho dự án P-110

| | Case 1 — Tessa | Case 2 — ChatGPT/bromua | Case 3 — Babylon symptom checker |
| --- | --- | --- | --- |
| Layer chính | Model + Safety | Grounding + Safety | Model + Safety |
| Tác hại đã ghi nhận | Lời khuyên giảm cân cho người rối loạn ăn uống; bot bị vô hiệu hóa | Ngộ độc bromua, nằm viện 3 tuần | Triage thiên lệch giới tính trong phép thử; tuyên bố hiệu năng bị phản biện (chưa ghi nhận tổn hại thực tế) |
| Bài học | Kiểm soát mọi thay đổi model của nhà cung cấp; xử lý ngay phản ánh của người dùng | Hỏi rõ ngữ cảnh và cảnh báo cụ thể | Kiểm chứng độc lập với người dùng thật; kiểm thử thiên lệch; không quảng bá vượt bằng chứng |

💭 **[Nhận định] Áp dụng cho P-110 — AI Nutrition Agent:**
1. **Human-in-the-loop bắt buộc** (từ Case 1 và 2): mọi thực đơn do agent sinh ra dừng ở trạng thái `PENDING_REVIEW` cho tới khi chuyên gia dinh dưỡng duyệt. Cần giữ nguyên tắc **không có đường đi nào bỏ qua bước duyệt**.
2. **Grounding và trích dẫn** (từ Case 2): cảnh báo dựa trên guideline (RAG) và có trích nguồn. Nên bổ sung danh sách **chất hoặc thực phẩm không an toàn** để chặn cứng các gợi ý thay thế nguy hiểm, không phụ thuộc vào LLM. Hồ sơ bệnh nền phải được đưa vào khi kiểm tra, để tránh trường hợp gợi ý muối kali cho người suy thận.
3. **Kiểm soát thay đổi model và prompt** (từ Case 1): mỗi lần đổi phiên bản LLM, prompt hoặc chỉ mục guideline, phải **chạy lại bộ eval an toàn** trước khi phát hành.
4. **Kiểm chứng độc lập và kiểm thử thiên lệch** (từ Case 3): eval trên hồ sơ demo **chưa đủ** để kết luận hệ thống an toàn với bệnh nhân thật. Cần thử nghiệm có giám sát, so sánh kết quả giữa các nhóm (giới tính, tuổi, bệnh nền), và **không quảng bá** P-110 là "ngang chuyên gia" khi chưa có bằng chứng.
5. **Dữ liệu nhạy cảm:** hồ sơ bệnh nền và thuốc đang dùng là dữ liệu sức khỏe nhạy cảm, nên cần xin đồng ý rõ ràng, giới hạn quyền truy cập và lưu nhật ký truy cập.
