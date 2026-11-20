# **BUSINESS REQUIREMENTS SPECIFICATION (BRS)**

## **Intelligent Reading Assistant**

**Trạng thái tài liệu:** Bản dự thảo cơ sở  
**Phiên bản:** 0.1  
**Loại tài liệu:** Business Requirements Specification (BRS)  
**Sản phẩm:** Intelligent Reading Assistant  
**Phạm vi:** Sản phẩm trợ lý đọc thông minh, bao gồm Chrome Browser Extension và website quản lý  
**Chuẩn tham chiếu:** ISO/IEC/IEEE 29148:2018

---

# **1\. Introduction**

## **1.1 Purpose**

Tài liệu Business Requirements Specification (BRS) xác định nhu cầu nghiệp vụ, mục tiêu, phạm vi, giá trị kỳ vọng, các năng lực cốt lõi và các ràng buộc ở cấp độ nghiệp vụ của sản phẩm Intelligent Reading Assistant.

Tài liệu được xây dựng nhằm:

* Thống nhất cách hiểu về vấn đề mà sản phẩm cần giải quyết và giá trị mà sản phẩm hướng đến.  
* Xác định các nhóm người dùng, nhu cầu đọc và các năng lực nghiệp vụ cần có.  
* Xác định phạm vi demo, những nguyên tắc vận hành quan trọng và các vấn đề cần được làm rõ trong các giai đoạn tiếp theo.  
* Làm cơ sở cho việc xây dựng Operational Concept Description (OpsCon), Stakeholder Requirements Specification (StRS), System Requirements Specification (SyRS) và Software Requirements Specification (SRS).  
* Tạo cơ sở đánh giá sơ bộ sản phẩm thông qua thử nghiệm với người dùng thực tế.

BRS tập trung vào **điều người dùng cần và kết quả nghiệp vụ sản phẩm cần đạt được**, không quyết định trước cách triển khai kỹ thuật của từng chức năng.

## **1.2 Scope**

### **1.2.1 Product scope**

Intelligent Reading Assistant là sản phẩm trợ lý đọc thông minh, hỗ trợ người dùng hiểu, xử lý và tiếp thu nội dung đang đọc trên các website bên ngoài.

Sản phẩm hướng đến việc tích hợp hoạt động hỗ trợ vào quá trình đọc hiện tại, giảm nhu cầu chuyển đổi qua lại giữa nhiều công cụ và cho phép người dùng cá nhân hóa cách nhận hỗ trợ.

Sản phẩm bao gồm hai thành phần trải nghiệm chính:

* **Browser Extension:** Thành phần hỗ trợ người dùng trực tiếp trong quá trình đọc trên website.  
* **Website quản lý:** Thành phần phục vụ thiết lập ban đầu, quản lý tài khoản, cấu hình hỗ trợ, quản lý dữ liệu cá nhân và xem lại lịch sử đọc theo phạm vi chức năng được xác định.

Demo tập trung vào Chrome Browser Extension. Sản phẩm được định hướng mở rộng khả năng tương thích với các trình duyệt khác trong tương lai.

### **1.2.2 In-scope**

Các nhóm năng lực thuộc phạm vi nghiệp vụ của demo gồm:

1. Hỗ trợ giải thích và hiểu nội dung đang đọc.  
2. Tóm tắt nội dung và trích xuất ý chính.  
3. Hỗ trợ dịch thuật và hiểu ngôn ngữ theo ngữ cảnh.  
4. Hỗ trợ củng cố kiến thức và kiểm tra mức độ hiểu.  
5. Cá nhân hóa cách thức và mức độ hỗ trợ.  
6. Đề xuất hỗ trợ chủ động trong quá trình đọc.  
7. Hỗ trợ tiếp cận thông qua tùy chỉnh cách trình bày nội dung.  
8. Hỗ trợ tương tác với một đoạn văn bản được người dùng lựa chọn.  
9. Ghi nhớ ngữ cảnh trao đổi và hỗ trợ tiếp tục quá trình đọc.  
10. Quản lý tài khoản, lịch sử đọc và dữ liệu cá nhân.  
11. Cấu hình LLM bằng API Key do người dùng cung cấp.  
12. Quản lý quyền riêng tư và việc sử dụng dữ liệu cho cá nhân hóa.  
13. Xử lý tình huống không thể đọc nội dung, dịch vụ không khả dụng hoặc AI không thể trả lời.  
14. Thu thập phản hồi đơn giản về kết quả AI.  
15. Hỗ trợ giao diện tiếng Việt và tiếng Anh.

Các nhóm năng lực trên đều thuộc phạm vi demo. Mức độ hoàn thiện, tiêu chí nghiệm thu và phương thức triển khai cụ thể sẽ được xác định trong các tài liệu tiếp theo.

### **1.2.3 Out-of-scope hoặc chưa cam kết**

Các nội dung sau chưa được xác định là cam kết nghiệp vụ cho demo:

* Hỗ trợ đầy đủ mọi trình duyệt ngoài Chrome.  
* Bảo đảm tương thích với mọi website, mọi cấu trúc trang và mọi loại nội dung nhúng.  
* Bảo đảm hiệu quả đọc hoặc mức cải thiện ghi nhớ bằng một ngưỡng định lượng cụ thể.  
* Cam kết một mức độ chính xác tuyệt đối cho mọi kết quả AI.  
* Cam kết một nhà cung cấp LLM cụ thể hoặc danh sách nhà cung cấp hoàn chỉnh.  
* Một mô hình thương mại, cơ chế thu phí hoặc gói dịch vụ miễn phí/trả phí.  
* Một cơ chế xử lý hoàn toàn tại thiết bị hoặc không truyền dữ liệu ra ngoài trong mọi trường hợp.  
* Các tiêu chuẩn kỹ thuật, kiến trúc, giao thức tích hợp, thuật toán và mô hình đánh giá chi tiết.

Việc chưa cam kết các nội dung này không đồng nghĩa với việc chúng bị loại trừ vĩnh viễn khỏi định hướng phát triển sản phẩm.

## **1.3 Product/Business Overview**

### **1.3.1 Business problem**

Trong quá trình đọc website, người dùng thường gặp khó khăn trong việc hiểu thuật ngữ, giải thích khái niệm, nắm bắt ý chính, xử lý văn bản dài, hiểu nội dung bằng ngoại ngữ và kết nối thông tin giữa các phần.

Khi gặp khó khăn, người dùng thường phải thực hiện các thao tác thủ công như:

* Chuyển sang công cụ dịch thuật.  
* Tìm kiếm thông tin trên công cụ tìm kiếm hoặc website khác.  
* Sử dụng chatbot AI để hỏi về nội dung đang đọc.  
* Mở thêm tài liệu liên quan để bổ sung kiến thức nền.  
* Quay lại nội dung ban đầu, đọc lại và tự tổng hợp câu trả lời.  
* Tự tìm lại vị trí đọc và ngữ cảnh khi tiếp tục ở một thời điểm khác.

Những thao tác này làm tăng thời gian và công sức đọc, gây gián đoạn tập trung và tạo thêm gánh nặng cho người dùng trong việc tổng hợp, ghi nhớ và đánh giá mức độ hiểu.

Bên cạnh đó, nhu cầu hỗ trợ khác nhau đáng kể giữa các cá nhân. Một người có thể cần giải thích chuyên sâu, trong khi người khác chỉ cần bản tóm tắt ngắn gọn hoặc cách trình bày dễ đọc hơn. Một cơ chế hỗ trợ cố định khó đáp ứng đầy đủ các khác biệt này.

### **1.3.2 Product vision**

Intelligent Reading Assistant hướng tới trở thành một trợ lý đọc thông minh, có khả năng hỗ trợ người dùng ngay trong môi trường đọc hiện tại, hiểu ngữ cảnh nội dung và điều chỉnh cách hỗ trợ theo nhu cầu của từng cá nhân.

Sản phẩm hướng đến trải nghiệm đọc thuận tiện, liền mạch, dễ tiếp cận và có khả năng cá nhân hóa, đồng thời duy trì quyền chủ động của người dùng đối với cách AI hỗ trợ và dữ liệu cá nhân của họ.

Sản phẩm không hướng đến một nhóm người dùng chuyên biệt duy nhất mà cung cấp các năng lực hỗ trợ đọc tổng quát, phục vụ nhiều nhu cầu và hoàn cảnh sử dụng khác nhau.

### **1.3.3 Target user groups**

Các nhóm người dùng và nhu cầu điển hình gồm:

| Nhóm người dùng | Nhu cầu và khó khăn tiêu biểu |
| ----- | ----- |
| Học sinh, sinh viên và người học | Hiểu thuật ngữ, nắm ý chính, kết nối kiến thức, ghi nhớ và tự đánh giá mức độ hiểu |
| Người nghiên cứu và người làm chuyên môn | Xử lý khối lượng tài liệu lớn, hiểu phương pháp và kết quả, tìm thông tin liên quan và giảm thời gian tra cứu |
| Người đọc nội dung chuyên ngành nhưng không có kiến thức nền tương ứng | Hiểu khái niệm, ngữ cảnh, lập luận và mối quan hệ giữa các nội dung |
| Người có nhu cầu hỗ trợ tiếp cận khi đọc | Điều chỉnh cách trình bày, giảm khó khăn khi theo dõi văn bản và cá nhân hóa trải nghiệm đọc |
| Người đọc ngoại ngữ | Hiểu từ vựng, cấu trúc câu, nghĩa theo ngữ cảnh và nội dung bằng ngôn ngữ khác |
| Người dùng phổ thông | Nắm nhanh nội dung dài, đặt câu hỏi trong khi đọc, tìm lại nội dung và tiếp tục phiên đọc |

Các nhóm trên không phải những phân khúc sản phẩm tách biệt. Sản phẩm cần hỗ trợ các nhu cầu khác nhau thông qua một trải nghiệm thống nhất, có khả năng tùy chỉnh.

### **1.3.4 Product direction**

* Nền tảng demo: Chrome Browser Extension và website quản lý.  
* Ngôn ngữ giao diện: tiếng Việt và tiếng Anh.  
* Ngôn ngữ nội dung ưu tiên: tiếng Việt và tiếng Anh.  
* Ngôn ngữ trả lời AI mặc định: tiếng Việt.  
* Cách sử dụng AI: người dùng tự cung cấp API Key để kết nối với LLM.  
* Mô hình AI: mỗi người dùng cấu hình một mô hình dùng chung cho các tác vụ hỗ trợ đọc tại một thời điểm.  
* Cá nhân hóa: ưu tiên cấu hình theo tài khoản và duy trì trải nghiệm thống nhất trên nhiều thiết bị.  
* Mở rộng tương lai: hỗ trợ thêm trình duyệt, ngôn ngữ và các khả năng đọc khác khi phù hợp.

## **1.4 Definitions, Acronyms, Abbreviations**

| Thuật ngữ | Định nghĩa |
| ----- | ----- |
| BRS | Business Requirements Specification – Tài liệu đặc tả yêu cầu nghiệp vụ |
| OpsCon | Operational Concept Description – Mô tả khái niệm vận hành |
| StRS | Stakeholder Requirements Specification – Đặc tả yêu cầu bên liên quan |
| SyRS | System Requirements Specification – Đặc tả yêu cầu hệ thống |
| SRS | Software Requirements Specification – Đặc tả yêu cầu phần mềm |
| AI | Artificial Intelligence – Trí tuệ nhân tạo |
| LLM | Large Language Model – Mô hình ngôn ngữ lớn |
| API Key | Khóa xác thực được sử dụng để truy cập dịch vụ AI hoặc API của nhà cung cấp |
| Browser Extension | Tiện ích mở rộng trình duyệt, cung cấp chức năng bổ sung trong môi trường trình duyệt |
| Context | Ngữ cảnh nội dung đang đọc và thông tin liên quan trong quá trình tương tác |
| Personalization | Cá nhân hóa trải nghiệm dựa trên lựa chọn, nhu cầu hoặc dữ liệu được người dùng cho phép sử dụng |
| Privacy Mode | Chế độ riêng tư nhằm tăng cường bảo vệ dữ liệu khi người dùng xử lý nội dung nhạy cảm |
| Reading History | Lịch sử đọc, phản ánh các nội dung hoặc phiên đọc đã được ghi nhận |
| Demo | Phiên bản trình diễn và thử nghiệm ban đầu của sản phẩm |
| As-Is | Quy trình hiện tại trước khi áp dụng sản phẩm |
| To-Be | Quy trình mục tiêu sau khi áp dụng sản phẩm |

---

# **2\. Referenced Documents**

| ID | Tài liệu | Vai trò |
| ----- | ----- | ----- |
| REF-01 | ISO/IEC/IEEE 29148:2018 – Systems and software engineering — Life cycle processes — Requirements engineering | Chuẩn tham chiếu cho hoạt động kỹ nghệ yêu cầu |
| REF-02 | Đề bài và yêu cầu của dự án HCI | Cơ sở xác định bối cảnh học thuật, phạm vi demo và yêu cầu trình diễn; cần đối chiếu với bản đề bài chính thức |
| REF-03 | Kết quả phỏng vấn nghiệp vụ người dùng/đại diện dự án | Nguồn thông tin chính để xác định vấn đề, mục tiêu, phạm vi và yêu cầu nghiệp vụ trong tài liệu này |

**Lưu ý:** REF-02 cần được đối chiếu với đề bài chính thức nếu tài liệu được dùng làm bản nộp hoặc làm cơ sở phê duyệt dự án. BRS này không giả định các điều kiện chưa được cung cấp.

---

# **3\. Business Context and Operational Concept**

## **3.1 Business Goals and Objectives/KPIs**

### **3.1.1 Business goals**

**BG-01 — Giảm công sức hỗ trợ đọc thủ công**

Giảm nhu cầu chuyển đổi giữa website đang đọc và các công cụ bên ngoài để dịch thuật, tra cứu, hỏi đáp và tổng hợp thông tin.

**BG-02 — Cải thiện khả năng hiểu nội dung**

Hỗ trợ người dùng hiểu từ vựng, thuật ngữ, khái niệm, ngữ cảnh, ý chính và mối quan hệ giữa các thông tin trong nội dung đang đọc.

**BG-03 — Hỗ trợ tiếp thu và ghi nhớ kiến thức**

Giúp người dùng củng cố kiến thức, tự đánh giá mức độ hiểu và kết nối nội dung mới với kiến thức đã có.

**BG-04 — Tăng tính liên tục của trải nghiệm đọc**

Giảm gián đoạn trong quá trình đọc và hỗ trợ người dùng tiếp tục từ ngữ cảnh hoặc vị trí đọc trước đó khi có thể.

**BG-05 — Cá nhân hóa trải nghiệm**

Cho phép người dùng điều chỉnh cách thức hỗ trợ, mức độ chi tiết, ngôn ngữ và các hành vi liên quan theo nhu cầu cá nhân.

**BG-06 — Tăng khả năng tiếp cận**

Hỗ trợ người dùng có nhu cầu đọc khác nhau thông qua khả năng tùy chỉnh cách trình bày và tiếp nhận nội dung.

**BG-07 — Duy trì quyền kiểm soát và sự tin cậy**

Đảm bảo người dùng hiểu được phạm vi hỗ trợ của AI, nhận biết khi hệ thống không đủ thông tin và kiểm soát việc sử dụng dữ liệu cá nhân.

**BG-08 — Xây dựng cơ sở đánh giá sản phẩm**

Tạo ra một bản demo có thể được người dùng trải nghiệm thực tế, từ đó thu thập phản hồi để xác định điểm mạnh, hạn chế và hướng phát triển tiếp theo.

### **3.1.2 Business objectives**

Các mục tiêu trên được cụ thể hóa ở cấp nghiệp vụ như sau:

* Tích hợp các năng lực hỗ trợ đọc chính vào môi trường đọc hiện tại.  
* Hỗ trợ cả tương tác theo yêu cầu và gợi ý chủ động.  
* Duy trì trải nghiệm hỗ trợ có thể tùy chỉnh, không áp đặt một cách thức đọc cho mọi người.  
* Ưu tiên nội dung của trang đang đọc và phân biệt rõ thông tin bổ sung từ nguồn bên ngoài.  
* Bảo vệ dữ liệu cá nhân và không tự động sử dụng nội dung đọc để huấn luyện mô hình.  
* Cho phép người dùng tự cấu hình LLM và quản lý API Key.  
* Thử nghiệm sản phẩm với người dùng nói chung và thu thập phản hồi qua khảo sát sau thử nghiệm.

### **3.1.3 KPIs and evaluation approach**

Tại thời điểm lập BRS, dự án chưa xác định ngưỡng KPI định lượng. Việc đặt ra các con số cụ thể khi chưa có dữ liệu thực nghiệm có thể tạo ra tiêu chí đánh giá thiếu cơ sở.

Do đó, demo sẽ được đánh giá sơ bộ thông qua thử nghiệm và khảo sát người dùng.

| Nhóm đánh giá | Nội dung cần xem xét |
| ----- | ----- |
| Hiệu quả hỗ trợ đọc | Người dùng có cảm thấy dễ hiểu nội dung hơn hay không |
| Công sức và thời gian | Sản phẩm có giảm nhu cầu tra cứu thủ công và chuyển đổi công cụ hay không |
| Tính hữu ích | Các chức năng có đáp ứng được nhu cầu đọc thực tế hay không |
| Khả năng cá nhân hóa | Người dùng có điều chỉnh được cách AI hỗ trợ theo nhu cầu hay không |
| Tính liên tục | Người dùng có thể tiếp tục trao đổi hoặc phiên đọc thuận tiện hơn hay không |
| Tính tiếp cận | Các tùy chỉnh có giúp người dùng tiếp nhận nội dung phù hợp hơn hay không |
| Độ tin cậy cảm nhận | Người dùng có nhận biết được giới hạn, sự không chắc chắn và nguồn của thông tin hay không |
| Quyền riêng tư | Người dùng có hiểu và kiểm soát được các lựa chọn liên quan đến dữ liệu hay không |
| Trải nghiệm tổng thể | Mức độ thuận tiện và hài lòng khi sử dụng sản phẩm |

Việc đọc nhanh hơn không được xem là thành công nếu người dùng hiểu nội dung kém đi. Các chỉ số đo lường, phương pháp thu thập dữ liệu và ngưỡng chấp nhận sẽ được xác định sau khi có cơ sở thử nghiệm phù hợp.

## **3.2 Business Model & Value Proposition**

### **3.2.1 Business model**

Sản phẩm hiện được định hướng là một công cụ phục vụ nhu cầu đọc của cá nhân.

Tại thời điểm lập BRS:

* Chưa xác định mô hình thương mại.  
* Chưa quyết định cơ chế thu phí hoặc phân chia gói miễn phí/trả phí.  
* Người dùng tự cung cấp API Key để sử dụng LLM.  
* Chưa xác định liệu sản phẩm có phát sinh các khoản chi phí khác ngoài dịch vụ AI do người dùng sử dụng hay không.

Do đó, BRS chưa đưa ra cam kết doanh thu, giá bán hoặc mô hình kinh doanh cụ thể.

### **3.2.2 Value proposition**

**Đối với người dùng:** Intelligent Reading Assistant cung cấp hỗ trợ đọc ngay trong môi trường đọc hiện tại, giúp giảm thao tác thủ công, cải thiện khả năng hiểu nội dung và cho phép cá nhân hóa cách tiếp nhận thông tin.

Các giá trị chính gồm:

1. **Hỗ trợ đúng ngữ cảnh:** Người dùng có thể yêu cầu hỗ trợ đối với nội dung đang đọc hoặc một đoạn văn bản cụ thể.  
2. **Giảm gián đoạn:** Giảm nhu cầu chuyển đổi qua lại giữa nhiều công cụ để xử lý khó khăn khi đọc.  
3. **Hỗ trợ đa dạng nhu cầu:** Cung cấp giải thích, tóm tắt, dịch thuật, kiểm tra mức độ hiểu và tùy chỉnh cách trình bày.  
4. **Cá nhân hóa:** Cho phép điều chỉnh hành vi hỗ trợ và cấu hình LLM theo lựa chọn cá nhân.  
5. **Duy trì tính liên tục:** Hỗ trợ ghi nhớ ngữ cảnh và tiếp tục quá trình đọc.  
6. **Minh bạch và kiểm soát:** Thông báo khi AI không thể đáp ứng yêu cầu, phân biệt nội dung nguồn với thông tin bổ sung và cho phép người dùng quản lý dữ liệu cá nhân.

### **3.2.3 Business stakeholders**

Các bên liên quan được xác định hiện tại:

* **Người dùng cá nhân:** Người trực tiếp sử dụng tiện ích và website quản lý.  
* **Nhóm phát triển dự án:** Chịu trách nhiệm xây dựng, thử nghiệm, thu thập phản hồi và cải tiến sản phẩm.

Nhà cung cấp LLM là bên phụ thuộc bên ngoài có liên quan đến việc cung cấp dịch vụ AI và xử lý dữ liệu. Vai trò, điều kiện tích hợp và yêu cầu đối với nhà cung cấp sẽ được xác định chi tiết hơn trong các tài liệu phù hợp.

## **3.3 As-Is vs To-Be Process Flow**

### **3.3.1 As-Is process**

Quy trình đọc phổ biến hiện tại:

1. Người dùng mở website hoặc tài liệu cần đọc.  
2. Người dùng tự đọc và cố gắng hiểu nội dung.  
3. Khi gặp khó khăn, người dùng chuyển sang công cụ khác để dịch, tra cứu, hỏi AI hoặc tìm tài liệu liên quan.  
4. Người dùng tự tổng hợp thông tin và quay lại nội dung ban đầu.  
5. Người dùng tiếp tục đọc; khi cần quay lại sau, họ có thể phải tìm lại vị trí đọc và khôi phục ngữ cảnh.

Các vấn đề chính của quy trình này là thao tác thủ công, gián đoạn, khó duy trì ngữ cảnh và thiếu khả năng điều chỉnh hỗ trợ theo nhu cầu cá nhân.

### **3.3.2 To-Be process**

Quy trình mục tiêu khi sử dụng Intelligent Reading Assistant:

1. Người dùng truy cập website có nội dung họ được phép đọc.  
2. Người dùng đọc nội dung bình thường trong trình duyệt.  
3. Khi cần, người dùng chọn một đoạn văn bản hoặc yêu cầu hỗ trợ theo ngữ cảnh.  
4. AI cung cấp giải thích, tóm tắt, dịch thuật, trả lời câu hỏi hoặc hỗ trợ phù hợp khác.  
5. Hệ thống ưu tiên nội dung của trang hiện tại; nếu cần bổ sung thông tin, hệ thống phân biệt nội dung nguồn với thông tin bên ngoài.  
6. Hệ thống có thể chủ động gợi ý hỗ trợ theo hành vi và cấu hình của người dùng, đồng thời hạn chế làm gián đoạn việc đọc.  
7. Người dùng điều chỉnh mức độ chi tiết, cách diễn đạt và hành vi hỗ trợ theo nhu cầu.  
8. Khi tiếp tục ở phiên sau, người dùng có thể khôi phục ngữ cảnh hoặc vị trí đọc trong phạm vi hệ thống hỗ trợ.  
9. Người dùng quản lý tài khoản, lịch sử đọc, dữ liệu cá nhân và cấu hình AI theo quyền được cung cấp.

Nếu hệ thống hoặc dịch vụ AI không khả dụng, người dùng vẫn có thể tiếp tục đọc website gốc khi website còn truy cập được. Hệ thống phải thông báo rõ những chức năng đang tạm thời không thể sử dụng.

---

# **4\. Business Requirements**

## **4.1 Core Business Capabilities**

Các yêu cầu dưới đây mô tả năng lực và kết quả nghiệp vụ mong muốn. Tiêu chí nghiệm thu chi tiết sẽ được xác định ở các tầng yêu cầu tiếp theo.

### **4.1.1 Hỗ trợ hiểu nội dung**

**BR-01 — Giải thích nội dung:** Sản phẩm phải hỗ trợ người dùng hiểu từ vựng, thuật ngữ, khái niệm và nội dung khó hiểu trong quá trình đọc.

**BR-02 — Hỏi đáp theo ngữ cảnh:** Sản phẩm phải cho phép người dùng đặt câu hỏi về nội dung đang đọc và nhận câu trả lời phù hợp với ngữ cảnh.

**BR-03 — Xử lý đoạn văn bản được chọn:** Người dùng phải có thể chọn một đoạn văn bản cụ thể để yêu cầu AI giải thích, dịch, tóm tắt hoặc trả lời câu hỏi liên quan.

**BR-04 — Tóm tắt và trích xuất ý chính:** Sản phẩm phải hỗ trợ tóm tắt nội dung và làm nổi bật các ý quan trọng để giảm công sức xử lý văn bản dài hoặc phức tạp.

### **4.1.2 Hỗ trợ ngôn ngữ và học tập**

**BR-05 — Dịch thuật theo ngữ cảnh:** Sản phẩm phải hỗ trợ dịch nội dung và giải thích nghĩa của từ hoặc cụm từ theo ngữ cảnh.

**BR-06 — Củng cố kiến thức:** Sản phẩm phải hỗ trợ các hình thức kiểm tra mức độ hiểu và củng cố kiến thức trong quá trình đọc.

**BR-07 — Điều chỉnh hoạt động học tập:** Sản phẩm cần cho phép cấu hình hoạt động kiểm tra mức độ hiểu theo nhu cầu. Ví dụ về tần suất là sau một lượng nội dung nhất định; giá trị cụ thể chưa được chốt.

### **4.1.3 Cá nhân hóa và hỗ trợ chủ động**

**BR-08 — Tùy chỉnh kết quả hỗ trợ:** Người dùng phải có khả năng yêu cầu giải thích lại, điều chỉnh độ dài, mức độ chi tiết và cách diễn đạt phù hợp với trình độ hoặc nhu cầu.

**BR-09 — Tùy chỉnh phạm vi thông tin:** Người dùng phải có thể yêu cầu AI chỉ dựa vào nội dung của trang hiện tại khi cần.

**BR-10 — Hỗ trợ chủ động:** Sản phẩm phải có khả năng chủ động đề xuất hỗ trợ trong quá trình đọc. Tính năng này được bật mặc định và người dùng có thể tinh chỉnh hoặc tắt theo nhu cầu.

**BR-11 — Điều chỉnh cách trình bày:** Sản phẩm phải hỗ trợ các lựa chọn trình bày nội dung nhằm đáp ứng nhu cầu tiếp cận khác nhau, trong khi ưu tiên giữ nguyên giao diện và nội dung gốc của website nếu người dùng không yêu cầu thay đổi.

### **4.1.4 Độ tin cậy và nguồn thông tin**

**BR-12 — Ưu tiên nguồn hiện tại:** Khi hỗ trợ người dùng hiểu nội dung, sản phẩm phải ưu tiên nội dung của trang đang đọc.

**BR-13 — Phân biệt thông tin bổ sung:** Khi nội dung nguồn không đủ và hệ thống sử dụng kiến thức AI hoặc nguồn bên ngoài, sản phẩm phải phân biệt thông tin bổ sung với nội dung từ trang gốc.

**BR-14 — Thể hiện giới hạn:** Khi không đủ thông tin hoặc không chắc chắn, hệ thống phải thể hiện sự không chắc chắn hoặc thông báo rằng chưa có đủ cơ sở để trả lời, thay vì trình bày suy đoán như sự thật đã được xác minh.

**BR-15 — Phản hồi kết quả:** Người dùng nên có thể đánh dấu kết quả hữu ích/không hữu ích hoặc báo cáo kết quả chưa chính xác. Cơ chế phản hồi trong demo không bắt buộc người dùng nhập lý do.

### **4.1.5 Tài khoản, ngữ cảnh và lịch sử đọc**

**BR-16 — Tài khoản cá nhân:** Sản phẩm phải hỗ trợ tài khoản để duy trì cấu hình và dữ liệu người dùng theo phạm vi được cho phép.

**BR-17 — Trải nghiệm đa thiết bị:** Người dùng phải có thể đăng nhập tài khoản trên nhiều thiết bị và duy trì cấu hình cá nhân hóa thống nhất.

**BR-18 — Lịch sử đọc:** Sản phẩm phải hỗ trợ ghi nhận và xem lại lịch sử đọc theo tài khoản, phù hợp với lựa chọn quyền riêng tư của người dùng.

**BR-19 — Xóa lịch sử đọc:** Người dùng phải có khả năng xóa toàn bộ lịch sử đọc của tài khoản. Việc xóa lịch sử đọc cần được phân biệt với việc xóa các dữ liệu cá nhân hóa khác.

**BR-20 — Ghi nhớ ngữ cảnh:** Sản phẩm nên hỗ trợ ghi nhớ ngữ cảnh trao đổi và hỗ trợ tiếp tục quá trình đọc ở phiên sau, theo cấu hình và chính sách dữ liệu của người dùng.

**BR-21 — Khôi phục vị trí đọc:** Sản phẩm nên hỗ trợ ghi nhớ vị trí đọc gần nhất khi phù hợp, tránh xung đột với chức năng có sẵn của website.

### **4.1.6 Cấu hình AI**

**BR-22 — Tự cấu hình LLM:** Người dùng phải tự cung cấp API Key để sử dụng các tính năng AI.

**BR-23 — Một mô hình dùng chung:** Mỗi người dùng cấu hình một mô hình LLM dùng chung cho các tác vụ tại một thời điểm, không yêu cầu cấu hình riêng theo từng tác vụ.

**BR-24 — Quản lý API Key:** Người dùng phải có khả năng lưu cấu hình để dùng lại, thay đổi hoặc xóa API Key và kiểm tra trạng thái kết nối.

**BR-25 — Thông báo lỗi AI:** Hệ thống phải thông báo rõ khi API Key không hợp lệ, dịch vụ không khả dụng hoặc yêu cầu không thể xử lý.

**BR-26 — Theo dõi mức sử dụng:** Sản phẩm nên hỗ trợ người dùng theo dõi mức sử dụng AI và nhận biết các vấn đề về hạn mức hoặc chi phí trong phạm vi thông tin mà nhà cung cấp cho phép truy xuất.

### **4.1.7 Quyền riêng tư và dữ liệu cá nhân**

**BR-27 — Thông báo và xin phép:** Trước khi sử dụng lịch sử đọc hoặc tương tác AI để cá nhân hóa trải nghiệm, hệ thống phải thông báo rõ mục đích và xin phép người dùng.

**BR-28 — Rút lại sự đồng ý:** Khi người dùng rút lại sự đồng ý cho mục đích cá nhân hóa, hệ thống phải ngừng sử dụng dữ liệu cho mục đích đó. Cách xử lý dữ liệu cá nhân hóa đã thu thập trước đó cần được làm rõ trong yêu cầu chi tiết.

**BR-29 — Quản lý dữ liệu cá nhân:** Người dùng phải được cung cấp khả năng xem và xóa dữ liệu cá nhân theo phạm vi chức năng được xác định.

**BR-30 — Chế độ riêng tư:** Sản phẩm phải hỗ trợ chế độ riêng tư dành cho nội dung nhạy cảm, ưu tiên hạn chế lưu trữ và sử dụng lại dữ liệu, đồng thời vẫn cho phép AI hỗ trợ khi điều kiện bảo vệ dữ liệu được đáp ứng.

**BR-31 — Điều kiện đối với nhà cung cấp AI:** Khi chế độ riêng tư được sử dụng, sản phẩm phải ưu tiên các nhà cung cấp LLM có chính sách xử lý dữ liệu phù hợp. Nếu chưa đáp ứng điều kiện đã xác định, hệ thống phải cảnh báo hoặc ngăn gửi nội dung nhạy cảm theo chính sách được quy định.

**BR-32 — Giới hạn mục đích sử dụng:** Nội dung đọc chỉ được xử lý để cung cấp chức năng người dùng yêu cầu và không tự động được sử dụng để huấn luyện mô hình.

### **4.1.8 Tương thích và xử lý tình huống**

**BR-33 — Hỗ trợ website rộng rãi:** Sản phẩm phải hướng tới khả năng hoạt động trên nhiều website có nội dung văn bản, không giới hạn vào một nhóm website chuyên biệt.

**BR-34 — Giới hạn quyền truy cập:** Sản phẩm chỉ được xử lý nội dung mà người dùng có quyền truy cập; không tìm cách vượt qua cơ chế bảo vệ của website.

**BR-35 — Thông báo không thể đọc nội dung:** Khi không thể trích xuất hoặc xử lý nội dung của trang, hệ thống phải thông báo rõ và không được thể hiện như thể nội dung đã được đọc thành công.

**BR-36 — Bảo toàn trải nghiệm gốc:** Sản phẩm phải ưu tiên giữ nguyên giao diện và nội dung gốc của website. Những thay đổi có ảnh hưởng đến cách trình bày cần tuân theo lựa chọn hoặc cấu hình của người dùng.

**BR-37 — Tiếp tục đọc khi AI không khả dụng:** Khi AI hoặc kết nối mạng không khả dụng, sản phẩm phải giữ trải nghiệm đọc thông thường và cho phép tiếp tục sử dụng các chức năng không phụ thuộc AI trong phạm vi có thể.

**BR-38 — Ngôn ngữ demo:** Sản phẩm phải hỗ trợ giao diện tiếng Việt và tiếng Anh trong demo; nội dung hỗ trợ ưu tiên tiếng Việt và tiếng Anh; ngôn ngữ trả lời AI mặc định là tiếng Việt.

## **4.2 Financial & Commercial Requirements**

**BR-39 — Chi phí sử dụng LLM:** Người dùng tự cung cấp API Key và chịu trách nhiệm đối với chi phí phát sinh từ việc sử dụng dịch vụ AI theo điều kiện của nhà cung cấp.

**BR-40 — Minh bạch chi phí:** Sản phẩm nên hỗ trợ người dùng nhận biết mức sử dụng AI và các vấn đề liên quan đến hạn mức hoặc chi phí, nếu nhà cung cấp cung cấp dữ liệu cần thiết.

**BR-41 — Chưa xác định mô hình thương mại:** Sản phẩm hiện chưa có yêu cầu nghiệp vụ được phê duyệt về thu phí, gói dịch vụ, doanh thu hoặc thương mại hóa.

Không đặt ra yêu cầu ngân sách, giá bán, giới hạn chi tiêu hay cơ chế thanh toán cụ thể trong BRS này do chưa có quyết định tương ứng.

## **4.3 Regulatory & Compliance Requirements**

**BR-42 — Quyền riêng tư và bảo vệ dữ liệu:** Sản phẩm phải duy trì nguyên tắc minh bạch về dữ liệu được xử lý, mục đích sử dụng và quyền kiểm soát của người dùng.

**BR-43 — Sự đồng ý của người dùng:** Việc sử dụng dữ liệu cá nhân cho mục đích cá nhân hóa phải dựa trên thông báo và sự đồng ý của người dùng. Việc rút lại sự đồng ý phải được tôn trọng.

**BR-44 — Hạn chế sử dụng dữ liệu:** Nội dung đọc không được tự động sử dụng cho mục đích huấn luyện mô hình và không được sử dụng cho mục đích khác ngoài phạm vi đã được cho phép.

**BR-45 — Bảo vệ dữ liệu khi sử dụng dịch vụ bên ngoài:** Sản phẩm phải xem xét chính sách xử lý dữ liệu của nhà cung cấp LLM và thông báo hoặc ngăn xử lý nội dung nhạy cảm khi điều kiện bảo vệ dữ liệu chưa đáp ứng.

**BR-46 — Quyền truy cập nội dung:** Sản phẩm phải tôn trọng quyền truy cập và cơ chế bảo vệ nội dung của website nguồn.

Các yêu cầu trên là nguyên tắc nghiệp vụ. Các nghĩa vụ pháp lý cụ thể, phạm vi áp dụng, thời hạn lưu trữ, điều kiện chuyển dữ liệu cho bên thứ ba và tiêu chí đánh giá nhà cung cấp chưa được xác định. Chúng cần được rà soát trong các giai đoạn tiếp theo trước khi đưa ra cam kết tuân thủ pháp lý cụ thể.

---

# **5\. Business Constraints and Assumptions**

## **5.1 Business constraints**

**BC-01 — Phạm vi demo:** Demo tập trung vào Chrome Browser Extension và website quản lý. Khả năng hỗ trợ các trình duyệt khác thuộc định hướng phát triển tương lai.

**BC-02 — Ngôn ngữ:** Demo ưu tiên tiếng Việt và tiếng Anh, chưa yêu cầu triển khai các ngôn ngữ khác.

**BC-03 — Cấu hình AI:** Người dùng phải tự cung cấp API Key. Mỗi người dùng chỉ cấu hình một mô hình LLM dùng chung tại một thời điểm.

**BC-04 — Phụ thuộc nhà cung cấp:** Khả năng sử dụng AI, theo dõi mức tiêu thụ và xử lý lỗi phụ thuộc vào dịch vụ và thông tin do nhà cung cấp LLM cung cấp.

**BC-05 — Tương thích website:** Mức độ hỗ trợ phụ thuộc vào khả năng truy cập và trích xuất nội dung từ website. Sản phẩm không được cam kết hỗ trợ mọi website hoặc mọi dạng nội dung nhúng.

**BC-06 — Quyền riêng tư:** Việc lưu trữ, sử dụng và truyền dữ liệu phải tuân theo nguyên tắc quyền riêng tư đã thống nhất, bao gồm sự đồng ý cho cá nhân hóa và giới hạn mục đích sử dụng dữ liệu.

**BC-07 — Giữ nguyên trang nguồn:** Sản phẩm ưu tiên không làm thay đổi giao diện và nội dung gốc của website ngoài những thay đổi người dùng yêu cầu hoặc cho phép.

**BC-08 — Chưa có ngưỡng KPI:** Chưa xác định các ngưỡng định lượng về thời gian đọc, mức độ hiểu, mức độ ghi nhớ hoặc sự hài lòng.

**BC-09 — Chưa xác định ngân sách và tiến độ:** Các giới hạn cụ thể về thời gian, nguồn lực và ngân sách chưa được xác nhận trong giai đoạn phỏng vấn nghiệp vụ.

## **5.2 Business assumptions**

**BA-01:** Người dùng có nhu cầu hỗ trợ hiểu nội dung khi đọc website và mong muốn giảm thao tác thủ công.

**BA-02:** Việc cung cấp hỗ trợ ngay trong môi trường đọc có thể giảm gián đoạn so với quy trình chuyển đổi qua nhiều công cụ.

**BA-03:** Người dùng có thể có các nhu cầu đọc khác nhau; khả năng tùy chỉnh sẽ hữu ích hơn việc áp dụng một cách hỗ trợ cố định.

**BA-04:** Người dùng có thể tự cung cấp và quản lý API Key để sử dụng dịch vụ LLM.

**BA-05:** Các nhà cung cấp LLM có thể khác nhau về chính sách xử lý dữ liệu, khả năng cung cấp thông tin sử dụng và tính sẵn sàng của dịch vụ.

**BA-06:** Người dùng có thể tiếp tục truy cập website gốc khi một số chức năng của tiện ích không khả dụng, miễn là website đó vẫn hoạt động.

**BA-07:** Phản hồi và khảo sát sau thử nghiệm có thể cung cấp dữ liệu ban đầu để đánh giá mức độ hữu ích của sản phẩm và xác định hướng cải tiến.

Các giả định trên cần được kiểm chứng thông qua thiết kế, triển khai và thử nghiệm. Chúng không được xem là kết quả đã được chứng minh.

---

# **6\. Business Risks and Opportunities**

## **6.1 Business risks**

| ID | Rủi ro | Ảnh hưởng nghiệp vụ | Hướng xử lý ở cấp nghiệp vụ |
| ----- | ----- | ----- | ----- |
| RSK-01 | AI trả lời sai hoặc thiếu chính xác | Người dùng hiểu sai nội dung và giảm niềm tin vào sản phẩm | Ưu tiên nguồn hiện tại, thể hiện giới hạn và phân biệt thông tin bổ sung |
| RSK-02 | Kết quả không bám sát nội dung đang đọc | Hỗ trợ không giải quyết đúng vấn đề người dùng gặp phải | Ưu tiên ngữ cảnh trang hiện tại và cho phép giới hạn phạm vi thông tin |
| RSK-03 | Rò rỉ hoặc sử dụng dữ liệu sai mục đích | Ảnh hưởng nghiêm trọng đến quyền riêng tư và mức độ tin cậy | Giới hạn mục đích sử dụng, yêu cầu sự đồng ý và xác định điều kiện với nhà cung cấp |
| RSK-04 | Nhà cung cấp LLM lưu hoặc sử dụng dữ liệu không phù hợp | Nội dung nhạy cảm có thể không được bảo vệ như người dùng kỳ vọng | Đánh giá chính sách nhà cung cấp; cảnh báo hoặc ngăn gửi dữ liệu khi chưa đáp ứng điều kiện |
| RSK-05 | API Key không hợp lệ, hết hạn hoặc hết hạn mức | Các chức năng AI bị gián đoạn | Thông báo rõ trạng thái và hướng dẫn người dùng kiểm tra cấu hình |
| RSK-06 | Website không tương thích hoặc không thể trích xuất nội dung | Một số yêu cầu đọc không được đáp ứng | Thông báo giới hạn và giữ trải nghiệm đọc gốc |
| RSK-07 | Gợi ý chủ động gây gián đoạn | Người dùng khó tập trung hoặc không muốn tiếp tục sử dụng | Bật mặc định nhưng cho phép điều chỉnh mức độ hoặc tắt |
| RSK-08 | Cá nhân hóa không phù hợp với nhu cầu thực tế | Kết quả hỗ trợ gây khó chịu hoặc không hữu ích | Cho phép người dùng tùy chỉnh và kiểm soát việc sử dụng dữ liệu |
| RSK-09 | Khảo sát thử nghiệm không đại diện cho mọi nhóm | Khó khẳng định hiệu quả cho tất cả người dùng | Trình bày kết quả như đánh giá sơ bộ, không khái quát quá mức |
| RSK-10 | Phạm vi demo rộng nhưng nguồn lực chưa rõ | Khó xác định mức độ hoàn thiện và khả năng trình diễn | Giữ nguyên phạm vi nghiệp vụ; xác định kế hoạch và mức hoàn thiện ở giai đoạn lập kế hoạch triển khai |

## **6.2 Business opportunities**

**OPP-01 — Trải nghiệm đọc liền mạch:** Giảm nhu cầu rời khỏi trang đang đọc để tìm kiếm hỗ trợ.

**OPP-02 — Phục vụ nhiều nhu cầu:** Một sản phẩm có thể hỗ trợ người học, người nghiên cứu, người đọc ngoại ngữ, người đọc nội dung chuyên ngành và người có nhu cầu tiếp cận khác nhau.

**OPP-03 — Cá nhân hóa trải nghiệm:** Cho phép người dùng tùy chỉnh cách hỗ trợ và lựa chọn LLM theo nhu cầu cá nhân.

**OPP-04 — Cải thiện tính liên tục:** Ghi nhớ ngữ cảnh và lịch sử đọc có thể giúp người dùng tiếp tục quá trình đọc thuận tiện hơn.

**OPP-05 — Phát triển theo phản hồi:** Kết quả thử nghiệm và khảo sát tạo cơ sở xác định những tính năng có giá trị thực tế và những điểm cần cải tiến.

**OPP-06 — Mở rộng nền tảng:** Sau demo, sản phẩm có thể xem xét hỗ trợ thêm trình duyệt, ngôn ngữ và các dạng nội dung khác dựa trên kết quả thử nghiệm và nguồn lực.

---

# **7\. Verification and Validation Criteria**

## **7.1 Verification criteria**

Verification ở cấp BRS nhằm xác nhận rằng phạm vi và các yêu cầu nghiệp vụ đã được phản ánh đầy đủ, nhất quán trong các tài liệu và kết quả triển khai.

Các tiêu chí rà soát gồm:

**VVC-01 — Bao phủ phạm vi:** Các nhóm năng lực đã thống nhất được phản ánh trong BRS và được truy vết tới yêu cầu ở tầng phù hợp.

**VVC-02 — Nhất quán mục tiêu:** Các yêu cầu nghiệp vụ hỗ trợ các mục tiêu đã xác định tại Mục 3.1.

**VVC-03 — Phân biệt tầng yêu cầu:** Các nội dung thuộc thiết kế, kiến trúc và tiêu chí kỹ thuật chi tiết không bị trình bày như các quyết định nghiệp vụ đã được phê duyệt.

**VVC-04 — Nhất quán quyền riêng tư:** Các yêu cầu về đồng ý, rút lại sự đồng ý, sử dụng dữ liệu, chế độ riêng tư và nhà cung cấp AI không mâu thuẫn với nhau.

**VVC-05 — Xử lý ngoại lệ:** Các trường hợp AI không khả dụng, không thể đọc nội dung, thiếu thông tin và lỗi API được phản ánh trong yêu cầu nghiệp vụ.

**VVC-06 — Nhận diện nội dung chưa xác định:** Các vấn đề còn mở được ghi nhận là chưa xác định, không bị chuyển thành cam kết không có căn cứ.

**VVC-07 — Khả năng truy vết:** Các yêu cầu nghiệp vụ được đánh mã định danh để làm cơ sở xây dựng ma trận truy vết với các tài liệu tiếp theo.

## **7.2 Validation criteria**

Validation nhằm xác nhận rằng sản phẩm đáp ứng nhu cầu thực tế của người dùng và tạo ra giá trị kỳ vọng.

### **7.2.1 Phương pháp đánh giá**

* Thực hiện thử nghiệm với người dùng nói chung.  
* Thu thập phản hồi bằng khảo sát sau khi người dùng trải nghiệm demo.  
* Đánh giá cảm nhận về mức độ hữu ích, khả năng hỗ trợ hiểu nội dung, tính thuận tiện, khả năng cá nhân hóa và quyền kiểm soát.  
* Ghi nhận các vấn đề trong quá trình sử dụng, bao gồm lỗi tương thích, kết quả AI chưa phù hợp và những điểm gây gián đoạn.  
* Sử dụng kết quả để xác định điểm mạnh, hạn chế và hướng phát triển tiếp theo.

Demo không yêu cầu phải tuyển đủ người dùng đại diện cho tất cả nhóm đối tượng. Vì vậy, kết quả khảo sát chỉ được sử dụng để đánh giá sơ bộ và không mặc nhiên đại diện cho toàn bộ người dùng mục tiêu.

### **7.2.2 Điều kiện đánh giá nghiệp vụ**

| ID | Tiêu chí | Bằng chứng dự kiến |
| ----- | ----- | ----- |
| VAL-01 | Người dùng có thể sử dụng các nhóm năng lực hỗ trợ đọc thuộc phạm vi demo | Kết quả trình diễn và thử nghiệm chức năng |
| VAL-02 | Hỗ trợ AI giúp giảm thao tác thủ công hoặc thuận tiện hơn khi đọc | Phản hồi khảo sát và quan sát thử nghiệm |
| VAL-03 | Người dùng có thể tùy chỉnh cách AI hỗ trợ | Kết quả thử nghiệm các lựa chọn cá nhân hóa |
| VAL-04 | Hệ thống phân biệt nội dung nguồn và thông tin bổ sung, đồng thời thể hiện giới hạn khi thiếu thông tin | Kiểm tra các tình huống sử dụng và phản hồi thử nghiệm |
| VAL-05 | Người dùng có thể quản lý các lựa chọn liên quan đến tài khoản, cấu hình AI và quyền riêng tư trong phạm vi demo | Kết quả kiểm tra các luồng sử dụng tương ứng |
| VAL-06 | Khi dịch vụ AI không khả dụng, người dùng được thông báo và vẫn có thể tiếp tục đọc website gốc nếu website còn hoạt động | Kiểm tra tình huống lỗi và kết quả trình diễn |
| VAL-07 | Người dùng có thể đưa ra phản hồi đơn giản về kết quả AI | Kết quả thử nghiệm chức năng phản hồi |
| VAL-08 | Sản phẩm cung cấp đủ thông tin để nhóm đánh giá và xác định hướng cải tiến sau thử nghiệm | Báo cáo tổng hợp khảo sát và các vấn đề ghi nhận |

Các tiêu chí này là cơ sở đánh giá nghiệp vụ ban đầu, chưa phải bộ tiêu chí nghiệm thu kỹ thuật hoàn chỉnh. Ngưỡng định lượng, cỡ mẫu khảo sát, cách đo lường và điều kiện đạt/chưa đạt sẽ được xác định khi có kế hoạch thử nghiệm cụ thể.

## **7.3 Outstanding issues**

Các nội dung sau cần được làm rõ trong các giai đoạn tiếp theo:

1. Các chỉ số và phương pháp đánh giá hiệu quả đọc, mức độ hiểu và trải nghiệm người dùng.  
2. Mức độ hoàn thiện và tiêu chí nghiệm thu cụ thể của từng năng lực trong demo.  
3. Danh sách nhà cung cấp LLM, điều kiện kết nối và tiêu chí đánh giá chính sách xử lý dữ liệu.  
4. Phạm vi dữ liệu được lưu trữ, thời hạn lưu, quyền truy cập, cơ chế xóa và ảnh hưởng của việc xóa lịch sử đọc tới dữ liệu cá nhân hóa.  
5. Các điều kiện chính xác khi bật chế độ riêng tư, đặc biệt là điều kiện gửi nội dung đến LLM bên ngoài.  
6. Các thông tin về chi phí, token và hạn mức có thể cung cấp từ nhà cung cấp LLM.  
7. Phạm vi hỗ trợ website và nội dung nhúng có thể đáp ứng trong demo.  
8. Kế hoạch, tiến độ, nguồn lực và ngân sách triển khai.

Các vấn đề trên không làm mất hiệu lực của định hướng nghiệp vụ trong tài liệu này, nhưng cần được xử lý trước khi đưa ra cam kết chi tiết về triển khai, bảo mật, tuân thủ hoặc nghiệm thu.

---

**Kết thúc tài liệu BRS — Intelligent Reading Assistant, phiên bản 0.1.**

