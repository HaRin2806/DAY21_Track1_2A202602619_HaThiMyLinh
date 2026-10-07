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

- **Tổ chức / sản phẩm AI:** National Eating Disorders Association (NEDA), tổ chức phi lợi nhuận về rối loạn ăn uống của Mỹ. Chatbot *Tessa* ban đầu là chatbot **theo kịch bản (rule-based)**, nội dung do nhóm nghiên cứu của Dr. Ellen Fitzsimmons-Craft xây dựng; công ty **Cass** vận hành nền tảng.
- **Thời gian, địa điểm / bối cảnh:** Mỹ, 2022–2023.
  - Tessa ra mắt lặng lẽ từ **02/2022** (CNN).
  - Ngày **31/03/2023**, NEDA báo cho nhân viên đường dây nóng rằng vị trí của họ sẽ bị cắt vào tháng 6; Tessa được định thay thế đường dây nóng do người trực (NPR/WHYY).
- **AI được dùng để làm gì:** trợ lý sức khỏe dạng chatbot, cung cấp nội dung phòng ngừa rối loạn ăn uống và hỗ trợ người có nguy cơ.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - Cuối 05/2023, chuyên gia tư vấn **Sharon Maxwell** thử Tessa và được khuyên **giảm cân và thâm hụt calo**, tức là những hành vi có thể làm rối loạn ăn uống nặng hơn (Daily Dot).
  - NEDA **gỡ Tessa** "cho tới khi có thông báo mới" (CNN, 01/06/2023). Theo Vice, bot bị vô hiệu hóa **2 ngày** trước khi được triển khai đầy đủ (AI Incident Database, Incident 545).
  - Theo The Conversation (dẫn NPR), Tessa ban đầu không thể trả lời ngoài kịch bản. Sau đó công ty vận hành đã đổi Tessa sang phiên bản có tính năng hỏi đáp dùng **AI tạo sinh (generative AI)**.
- **Số liệu có nguồn:**
  - **Gần 70.000 người** dùng đường dây nóng của NEDA trong năm trước khi đóng. Đường dây nóng chỉ có **5–6 nhân viên được trả lương, 2 giám sát** và **90–165 tình nguyện viên** luân phiên (NPR, 31/05/2023, bản đăng lại trên WHYY).
  - Tessa khuyên người dùng giảm **1–2 pound/tuần (khoảng 0,5–0,9 kg)** bằng cách thâm hụt **500–1.000 calo/ngày** (Daily Dot, 2023, lưu trên AI Incident Database).
- **Nguồn:**
  - *National Eating Disorders Association phases out human helpline, pivots to chatbot* — Kate Wells, NPR (bản đăng lại trên WHYY) — 31/05/2023 — https://whyy.org/npr-story/national-eating-disorders-association-chatbot/
  - *NEDA takes its AI chatbot offline after complaints of harmful advice* — CNN Business — 01/06/2023 — https://krdo.com/money/cnn-business-consumer/2023/06/01/national-eating-disorders-association-takes-its-ai-chatbot-offline-after-complaints-of-harmful-advice/
  - *'This robot causes harm': National Eating Disorders Association's new chatbot advises people with disordering eating to lose weight* — Daily Dot — 2023 — AI Incident Database, Report 3130: https://incidentdatabase.ai/reports/3130
  - *Replacing frontline workers with AI can be a bad idea — here's why* — Mark Tsagas, The Conversation — 30/10/2023 — https://theconversation.com/replacing-frontline-workers-with-ai-can-be-a-bad-idea-heres-why-215120 (đoạn về việc Tessa được đổi sang AI tạo sinh)
  - AI Incident Database — Incident 545 (tổng hợp các bài của Vice, Guardian, WSJ, NYT) — https://incidentdatabase.ai/cite/545/
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** Tessa đã khuyên giảm cân và thâm hụt calo; NEDA gỡ bot; Tessa đã được đổi từ chatbot theo kịch bản sang phiên bản dùng AI tạo sinh.
  - 💭 **Chưa rõ / tôi suy luận:**
    - Ai quyết định và ai chịu trách nhiệm cho việc đổi sang AI tạo sinh **chưa được làm rõ** trong các nguồn tôi kiểm tra được.
    - Chưa có nguồn ghi nhận người dùng cụ thể nào **bị tái phát bệnh** vì Tessa.
    - Số người đã nhận câu trả lời có hại chưa được công bố.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Người đang mắc hoặc có nguy cơ rối loạn ăn uống hỏi Tessa về cân nặng hay cách cải thiện sức khỏe — thời điểm chatbot thay thế đường dây nóng do người trực và câu trả lời có thể định hướng hành vi ăn uống của họ. |
| Stakeholder bị ảnh hưởng | **Người dùng có rối loạn ăn uống** (trực tiếp); **người thân** của họ; **NEDA** (tổ chức triển khai); **nhân viên và tình nguyện viên đường dây nóng** (bị thay thế); **Cass** (nhà cung cấp). |
| Failure mode | **Harmful advice** — bot khuyên giảm cân và thâm hụt calo, là hành vi có hại cho đúng nhóm người dùng mục tiêu. Phụ: **Escalation failure** — bot tiếp tục tự xử lý chủ đề cân nặng thay vì chuyển cho chuyên gia. *Giả thuyết:* lỗi chuyển tiếp này là suy luận của tôi từ việc đường dây nóng do người trực sắp bị đóng; nguồn không mô tả cơ chế chuyển tiếp của Tessa. |
| Layer bắt đầu lỗi | **Model + Safety.** *Model (có nguồn):* theo The Conversation (dẫn NPR), Tessa được đổi từ chatbot theo kịch bản sang phiên bản dùng AI tạo sinh, nên có thể tự sinh câu trả lời ngoài kịch bản do chuyên gia soạn. Mô hình mới không phù hợp với tác vụ cần nội dung được kiểm soát chặt. *Safety (giả thuyết):* việc thay đổi mô hình mà không kiểm thử lại, và không có guardrail chặn chủ đề giảm cân, là suy luận của tôi từ việc bot đưa lời khuyên giảm cân sau khi được nâng cấp; **chưa đủ bằng chứng** về quy trình kiểm thử và cấu hình guardrail thực tế. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** **Người thử (Sharon Maxwell)** bị khuyên giảm 1–2 pound/tuần, thâm hụt 500–1.000 calo/ngày **khi** hỏi Tessa. **NEDA** bị chỉ trích và phải vô hiệu hóa bot **khi** các câu trả lời được công khai. 💭 **[Nguy cơ, chưa ghi nhận]** **Người bệnh rối loạn ăn uống** có thể tái phát hoặc bệnh nặng hơn **khi** làm theo lời khuyên. **Người đang khủng hoảng** có thể không được hỗ trợ kịp **khi** kênh do người trực bị thay bằng bot. |
| Harm lens | **Injury** (tổn hại sức khỏe thể chất do hành vi ăn kiêng ở người rối loạn ăn uống); phụ: **Misinformation** (lời khuyên sức khỏe sai với đối tượng). |
| Severity | **Critical** |
| Scale | **Medium** — đánh giá của tôi. Căn cứ: đường dây nóng phục vụ gần 70.000 người/năm (NPR) và Tessa được định thay thế kênh này. **Giới hạn:** số người thực sự đã dùng Tessa và nhận câu trả lời có hại **chưa được công bố**. |
| Probability | **High** — đánh giá của tôi. Căn cứ: người thử nhận được lời khuyên có hại ngay khi hỏi về cân nặng, và nhiều người dùng khác cũng phản ánh nội dung tương tự (các bài tổng hợp trong AI Incident Database, Incident 545). Không có tỷ lệ đo được. |
| Frequency | **Chưa đủ dữ liệu để đánh giá bằng số.** Nhận định: **Medium** — chủ đề cân nặng thường gặp với người rối loạn ăn uống, nhưng không phải phiên chat nào cũng hỏi. |
| Vì sao? | **Severity Critical:** rối loạn ăn uống có thể đe dọa tính mạng, và lời khuyên đi ngược hẳn mục tiêu điều trị. **Scale Medium:** quy mô tiềm năng lớn (70.000 người/năm) nhưng bot bị tắt chỉ vài ngày sau khi định thay thế đường dây nóng, nên tôi không chọn High. **Probability High:** lỗi tái hiện dễ và được nhiều người dùng phản ánh. **Frequency:** không có số phiên chat nên chỉ nhận định. **Nguồn:** NPR/WHYY 31/05/2023; CNN 01/06/2023; Daily Dot; The Conversation; AI Incident Database. |

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
  - *A Case of Bromism Influenced by Use of Artificial Intelligence* — Eichenberger A., Thielke S., Van Buskirk A. — Annals of Internal Medicine: Clinical Cases — 08/2025 — nội dung báo cáo ca bệnh được lưu trên AI Incident Database, Report 6092: https://incidentdatabase.ai/reports/6092 (mục nồng độ bromua, thời gian dùng, thời gian nằm viện, phép thử ChatGPT 3.5)
  - *Man sought diet advice from ChatGPT and ended up with "bromism"* — Live Science — 08/2025 — https://www.livescience.com/health/food-diet/man-sought-diet-advice-from-chatgpt-and-ended-up-with-bromide-intoxication
  - *Man who asked ChatGPT about cutting out salt was hospitalized with hallucinations* — NBC News — 08/2025 — https://www.nbcnews.com/tech/tech-news/man-asked-chatgpt-cutting-salt-diet-was-hospitalized-hallucinations-rcna225055
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:** người bệnh ngộ độc bromua sau khi tham khảo ChatGPT; nồng độ bromua máu 1.700 mg/L; nằm viện 3 tuần; khi hỏi lại, ChatGPT 3.5 vẫn nhắc tới bromua mà không cảnh báo cụ thể.
  - 💭 **Chưa rõ:** các bác sĩ **không xem được nguyên văn cuộc hội thoại** của người bệnh, nên không biết chính xác câu hỏi và câu trả lời ban đầu. Người dùng cũng có thể đã hiểu sai ngữ cảnh (bromua thay clorua trong mục đích khác, như tẩy rửa). Đây là **một ca được ghi nhận**, chưa đủ để ước lượng tần suất.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Người dùng hỏi chatbot đa dụng nên dùng chất gì thay clorua (muối ăn) **trong chế độ ăn** — thời điểm câu trả lời của AI trở thành căn cứ để người dùng tự đưa một hóa chất vào cơ thể, không có nhân viên y tế ở giữa. |
| Stakeholder bị ảnh hưởng | **Người dùng** (trực tiếp); **gia đình**; **bệnh viện và đội điều trị** (chi phí, nguồn lực); **OpenAI** (uy tín, pháp lý); **người bệnh có bệnh nền** tự tìm lời khuyên dinh dưỡng trên chatbot (nguy cơ). |
| Failure mode | **Harmful advice** — nêu bromua làm chất thay clorua mà không cảnh báo độc tính khi dùng để ăn. Phụ: **Escalation failure** — không hỏi lại mục đích, không khuyên gặp bác sĩ (các bác sĩ báo cáo ca bệnh đã xác nhận khi hỏi lại). Phụ: **Over-reliance** — người dùng tin câu trả lời và tự dùng hóa chất suốt 3 tháng. |
| Layer bắt đầu lỗi | **Grounding + Safety**, phụ là **UX** — **giả thuyết của tôi**, vì OpenAI không công bố chi tiết hệ thống và các bác sĩ không có nguyên văn hội thoại. *Grounding:* câu trả lời không được giới hạn trong ngữ cảnh "thực phẩm cho người". *Safety:* có nguồn xác nhận rằng khi hỏi lại, bot **không cảnh báo cụ thể và không hỏi lý do**, tức lớp bảo vệ không kích hoạt với chủ đề nguy hiểm. *UX:* giao diện trò chuyện tạo cảm giác như được tư vấn, còn lời nhắc "ngữ cảnh rất quan trọng" quá chung chung. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra]** **Người đàn ông 60 tuổi** bị ngộ độc bromua (1.700 mg/L), loạn thần và phải nằm viện 3 tuần **khi** dùng natri bromua thay muối ăn suốt 3 tháng sau khi tham khảo ChatGPT. 💭 **[Nguy cơ, chưa ghi nhận]** **Người suy thận** có thể bị tăng kali máu **khi** chatbot không biết bệnh nền của họ mà gợi ý muối kali làm "muối thay thế". |
| Harm lens | **Injury** (ngộ độc, phải nằm viện); phụ: **Misinformation** (thông tin đúng về hóa học nhưng sai ngữ cảnh ăn uống). |
| Severity | **Critical** |
| Scale | **Low** với hậu quả đã ghi nhận: **1 ca** trong báo cáo ca bệnh. Với nguy cơ: **chưa đủ dữ liệu** để định lượng. Nhận định của tôi: phạm vi tiềm năng lớn vì chatbot đa dụng có rất nhiều người dùng hỏi về sức khỏe. |
| Probability | **Low** — đánh giá của tôi. Căn cứ: để gây hại cần chuỗi nhiều bước (hỏi đúng kiểu câu, hiểu sai, tự mua hóa chất, dùng lâu dài). Tuy nhiên câu trả lời có vấn đề **lặp lại được** khi các bác sĩ hỏi lại. |
| Frequency | **Chưa đủ dữ liệu để đánh giá.** Đây là ca duy nhất có trong nguồn; nhận định: **Low** với hậu quả nặng như ca này. |
| Vì sao? | **Severity Critical:** ngộ độc gây loạn thần và phải nằm viện 3 tuần — tổn hại thể chất nghiêm trọng đã xảy ra. **Scale Low:** chỉ có 1 ca được ghi nhận; không suy ra số người bị ảnh hưởng. **Probability và Frequency Low:** cần nhiều bước mới gây hại, cho thấy Severity cao **không** kéo theo xác suất cao. **Giới hạn:** không có nguyên văn hội thoại; mô hình đã được cập nhật nhiều lần từ đó. **Nguồn:** Annals of Internal Medicine: Clinical Cases, 2025. |

---

### 4. Case study 3 — Symptom checker của Babylon Health được quảng bá "ngang bác sĩ" nhưng thiếu bằng chứng

#### Brief Case

- **Tổ chức / sản phẩm AI:** Babylon Health (Anh). Sản phẩm là symptom checker — AI chẩn đoán và phân loại mức khẩn cấp (*Babylon Diagnostic and Triage System*), tích hợp trong ứng dụng, trong đó có dịch vụ GP at Hand của NHS.
- **Thời gian, địa điểm / bối cảnh:** Vương quốc Anh và một số quốc gia khác, 2018–2019.
  - **06/2018:** Babylon công bố kết quả đánh giá tại Royal College of Physicians, London, khẳng định AI chẩn đoán "ngang bác sĩ".
  - **11/2018:** các nhà nghiên cứu độc lập phản biện kết quả này trên The Lancet.
- **AI được dùng để làm gì:** người dùng nhập triệu chứng; AI gợi ý bệnh có thể mắc và khuyên mức xử lý, như tự chăm sóc, gặp bác sĩ đa khoa hay đi cấp cứu.
- **Vấn đề hoặc sự kiện đáng chú ý:**
  - **Tuyên bố hiệu năng thiếu bằng chứng:** Babylon tuyên bố AI đạt điểm cao hơn mức đỗ trung bình của bác sĩ trong bài thi MRCGP và chính xác ngang bác sĩ. Theo Undark, các nhà nghiên cứu (nhóm Fraser, Coiera, Wong trên The Lancet) kết luận nghiên cứu của Babylon **"không đưa ra bằng chứng thuyết phục"** rằng symptom checker hoạt động tốt hơn bác sĩ. Undark cũng ghi nhận khi đó **chưa có nghiên cứu ngẫu nhiên có đối chứng, đã bình duyệt** nào kiểm chứng hệ thống trên bệnh nhân thật.
  - **Thiên lệch giới tính:** Undark (2019) ghi lại trường hợp hai hồ sơ giống hệt nhau, cùng triệu chứng đau ngực. Với hồ sơ **nữ**, app gợi ý trầm cảm hoặc cơn hoảng loạn; với hồ sơ **nam**, app gợi ý vấn đề tim mạch.
  - **Quảng cáo gây hiểu lầm:** Babylon từng bị cơ quan quản lý quảng cáo của Anh khiển trách. Royal College of General Practitioners và British Medical Association cũng lên tiếng nghi ngờ các tuyên bố của công ty.
- **Số liệu có nguồn:**
  - **Theo công bố của Babylon (06/2018):**
    - Trên **100 tình huống bệnh mô phỏng** (vignette), AI đạt độ chính xác **80%**, trong khi **7 bác sĩ** đối chứng đạt **64–94%**.
    - Với câu hỏi mẫu của kỳ thi MRCGP, AI đạt **81%**, so với mức đỗ trung bình **72%** của bác sĩ trong 5 năm trước đó.
  - **Theo Undark (12/2019):** symptom checker đã được dùng khoảng **1,7 triệu lượt** ở nhiều quốc gia, và nền tảng đã có khoảng **700.000 lượt tư vấn số** giữa bệnh nhân và bác sĩ.
  - Theo Undark (12/2019), khi đó **chưa có nghiên cứu ngẫu nhiên có đối chứng, đã bình duyệt** nào kiểm chứng hiệu năng của hệ thống trên bệnh nhân thật.
- **Nguồn:**
  - *Safety of patient-facing digital symptom checkers* — Fraser H., Coiera E., Wong D. — The Lancet 392(10161):2263–2264 — 24/11/2018 — thông tin xuất bản: https://eprints.whiterose.ac.uk/id/eprint/138305/ (trang này chỉ có thông tin trích dẫn; nội dung phản biện được trích lại trong bài Undark bên dưới)
  - *Babylon AI achieves equivalent accuracy with human doctors* — thông cáo của Babylon, đăng lại trên BioSpectrum — 06/2018 — https://biospectrumindia.com/news/57/11202/babylon-ai-achieves-equivalent-accuracy-with-human-doctors-.html
  - *Medical Advice From a Bot: The Unproven Promise of Babylon Health* — Undark — 09/12/2019 — https://undark.org/2019/12/09/babylon-health-artificial-intelligence-medical-advice/ (mục về phản biện của nhóm Lancet, thiên lệch giới tính, khiển trách quảng cáo, số lượt sử dụng)
- **Phân biệt bằng chứng và nhận định:**
  - 📌 **Nguồn xác nhận:**
    - Các con số 80%, 81% là **do Babylon tự công bố**.
    - Phản biện của nhóm Lancet được Undark trích lại.
    - Ví dụ về thiên lệch giới tính và việc bị khiển trách vì quảng cáo được Undark ghi lại.
  - 💭 **Chưa rõ / tôi suy luận:**
    - **Chưa có nguồn ghi nhận người dùng cụ thể nào bị tổn hại** vì triage sai của Babylon. Tác hại tôi phân tích dưới đây là **nguy cơ**.
    - Ví dụ thiên lệch giới tính là một phép thử đơn lẻ, chưa phải nghiên cứu có hệ thống về tỷ lệ sai lệch.

#### Harm Map Worksheet

| Trường | Phân tích của tôi |
| --- | --- |
| High-risk moment | Một phụ nữ đau ngực nhập triệu chứng vào symptom checker — thời điểm AI đưa ra gợi ý chẩn đoán và mức khẩn cấp, quyết định người dùng có đi cấp cứu hay không. |
| Stakeholder bị ảnh hưởng | **Người dùng nữ** có triệu chứng tim mạch (trực tiếp); **người thân**; **NHS và bác sĩ đa khoa** tiếp nhận ca chuyển tuyến; **Babylon** (uy tín, pháp lý). |
| Failure mode | **Bias / fairness** — cùng triệu chứng đau ngực nhưng hồ sơ nữ được gợi ý trầm cảm hoặc hoảng loạn, hồ sơ nam được gợi ý bệnh tim (Undark). Phụ: **Over-reliance** — app được quảng bá là chính xác "ngang bác sĩ", có thể khiến người dùng tin kết quả mà không đi khám; đây là nhận định của tôi. |
| Layer bắt đầu lỗi | **Model + Safety**, phụ là **UX.** *Model:* kết quả khác nhau theo giới tính với cùng triệu chứng cho thấy hành vi mô hình không phù hợp với tác vụ triage. **Chưa đủ bằng chứng** về nguyên nhân kỹ thuật; giả thuyết của tôi là thiên lệch từ dữ liệu hoặc từ quy tắc xác suất theo giới tính. *Safety (có nguồn):* theo Undark, chưa có nghiên cứu ngẫu nhiên có đối chứng, đã bình duyệt nào kiểm chứng hệ thống trên bệnh nhân thật, dù app đã được triển khai rộng. *UX (giả thuyết):* kết quả được trình bày như lời khuyên y khoa nên khó để người dùng tự kiểm tra lại. |
| Harm xảy ra là gì? | 📌 **[Đã xảy ra, ở mức phép thử]** **Hồ sơ người dùng nữ** nhận gợi ý trầm cảm hoặc hoảng loạn thay vì bệnh tim **khi** nhập cùng triệu chứng với hồ sơ nam (Undark). **Babylon** bị khiển trách **khi** quảng cáo gây hiểu lầm. 💭 **[Nguy cơ, chưa ghi nhận]** **Phụ nữ bị nhồi máu cơ tim** có thể đi cấp cứu muộn **khi** tin vào gợi ý "trầm cảm hoặc hoảng loạn". |
| Harm lens | **Injury** (nguy cơ bỏ lỡ cấp cứu tim mạch); phụ: **Dignity loss** (triệu chứng của nữ giới bị quy cho vấn đề tâm lý, đối xử khác biệt theo giới tính). |
| Severity | **Critical** |
| Scale | **High** — đánh giá của tôi. Căn cứ: symptom checker đã được dùng khoảng **1,7 triệu lượt** ở nhiều quốc gia (Undark, 12/2019). Nếu thiên lệch nằm trong mô hình thì nó lặp lại với mọi người dùng cùng nhóm. **Giới hạn:** không có số liệu về số người dùng nữ bị đau ngực. |
| Probability | **Chưa đủ dữ liệu để đánh giá bằng tỷ lệ.** Nhận định: **Medium** — sai lệch xuất hiện trong phép thử, và theo Undark, hiệu năng của hệ thống trên bệnh nhân thật chưa được kiểm chứng. |
| Frequency | **Chưa đủ dữ liệu để đánh giá.** Nhận định: **Medium** — đau ngực là triệu chứng hay được tra cứu, nhưng ca nguy hiểm thật chỉ là một phần. |
| Vì sao? | **Severity Critical:** bỏ lỡ nhồi máu cơ tim có thể gây tử vong. **Scale High:** có số lượt sử dụng lớn từ nguồn, và thiên lệch ở mức mô hình mang tính hệ thống. **Probability và Frequency** chỉ nhận định, vì không có nghiên cứu đo tỷ lệ sai trên người dùng thật. **Giới hạn:** ví dụ thiên lệch là phép thử đơn lẻ; số liệu hiệu năng 80%/81% là Babylon tự công bố. **Nguồn:** Undark (12/2019), trích phản biện của nhóm Lancet (11/2018); thông cáo của Babylon (06/2018). |

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
