# **Stakeholder Requirements Specification (StRS)**

## **Intelligent Reading Assistant**

**Trạng thái:** Draft  
**Phiên bản:** 0.1  
**Loại tài liệu:** Stakeholder Requirements Specification  
**Dự án:** Intelligent Reading Assistant  
**Phạm vi:** Chrome Extension và website hướng dẫn tĩnh phục vụ bản demo MVP

---

# **1\. Introduction**

## **1.1. Purpose**

Tài liệu Stakeholder Requirements Specification (StRS) xác định các nhu cầu, kỳ vọng, ràng buộc và tiêu chí đánh giá của những bên liên quan đối với hệ thống Intelligent Reading Assistant.

Tài liệu đóng vai trò cầu nối giữa mục tiêu kinh doanh được xác định trong Business Requirements Specification (BRS) và các yêu cầu hệ thống, yêu cầu phần mềm được cụ thể hóa trong những tài liệu thiết kế tiếp theo.

Các mục tiêu chính của tài liệu bao gồm:

* Xác định các nhóm stakeholder và đặc điểm của người dùng mục tiêu.  
* Làm rõ những kết quả mà người dùng kỳ vọng khi sử dụng trợ lý đọc thông minh.  
* Xác định các kỳ vọng về chức năng, chất lượng, trải nghiệm, quyền riêng tư và môi trường sử dụng.  
* Ghi nhận những ràng buộc và rủi ro có ảnh hưởng đến việc phát triển, trình diễn và đánh giá hệ thống.  
* Thiết lập cơ sở để xác minh hệ thống có đáp ứng các nhu cầu của stakeholder hay không.

StRS tập trung vào **điều các bên liên quan cần hệ thống đáp ứng và lý do của những nhu cầu đó**, không quy định chi tiết cách hiện thực hóa về mặt kỹ thuật. Các quyết định kiến trúc, giao thức, mô hình AI và cấu trúc dữ liệu sẽ được trình bày trong các tài liệu phù hợp ở những giai đoạn tiếp theo.

## **1.2. Scope**

### **1.2.1. Product scope**

Intelligent Reading Assistant là hệ thống hỗ trợ đọc hiểu thông minh, hướng đến việc giúp người dùng tiếp cận, hiểu và xử lý nội dung văn bản hiệu quả hơn thông qua sự kết hợp giữa các chức năng hỗ trợ đọc và khả năng trí tuệ nhân tạo.

Sản phẩm hướng đến việc tích hợp hỗ trợ đọc ngay trong môi trường trình duyệt, hạn chế việc người dùng phải liên tục chuyển đổi giữa trang đang đọc và các công cụ bên ngoài.

Phạm vi sản phẩm dự kiến gồm:

**a. Chrome Extension**

Là thành phần ứng dụng chính, cung cấp các chức năng:

* Hỗ trợ giải thích từ vựng, thuật ngữ và khái niệm khó.  
* Hỏi đáp dựa trên đoạn văn được chọn hoặc nội dung trang đang đọc.  
* Tóm tắt và trích xuất các ý chính.  
* Dịch thuật và hỗ trợ đọc nội dung bằng ngoại ngữ.  
* Tạo câu hỏi kiểm tra mức độ hiểu nội dung sau khi người dùng đọc một lượng nội dung nhất định.  
* Cá nhân hóa cách hỗ trợ đọc và đưa ra gợi ý dựa trên hành vi đọc.  
* Hỗ trợ điều chỉnh cách trình bày để nội dung dễ tiếp cận hơn.  
* Quản lý tài khoản, cấu hình, API Key, lịch sử hội thoại và các tùy chọn cá nhân.  
* Hỗ trợ kiểm soát dữ liệu được gửi đến các dịch vụ AI bên ngoài.

**b. Website hướng dẫn tĩnh**

Website cung cấp thông tin hướng dẫn sử dụng, quy trình bắt đầu, xử lý những vấn đề thường gặp và thông tin hỗ trợ người dùng. Website này không phải thành phần chính để quản lý tài khoản hoặc cấu hình ứng dụng.

**c. Dịch vụ dùng chung**

Hệ thống có các dịch vụ dùng chung do nhóm phát triển triển khai và vận hành, phục vụ những chức năng cần thiết như quản lý dữ liệu tài khoản, lưu trữ lịch sử và đồng bộ thông tin. Các chức năng AI có thể phụ thuộc vào dịch vụ AI bên ngoài và cấu hình do người dùng cung cấp.

### **1.2.2. MVP scope**

Phiên bản MVP tập trung vào Chrome Extension trên máy tính để bàn hoặc laptop. Nhóm dự án hướng đến triển khai và trình diễn các nhóm tính năng đã xác định, ưu tiên các luồng chính và các nhánh lỗi quan trọng.

MVP không cam kết hỗ trợ chuyên sâu tất cả nhóm người dùng, mọi loại nội dung, mọi ngôn ngữ hoặc mọi tình huống sử dụng. Các chức năng chưa hoàn thiện hoặc chỉ được mô phỏng trong demo phải được phân biệt rõ với chức năng đã triển khai thực tế.

Khả năng mở rộng sang trình duyệt khác và thiết bị di động là định hướng dài hạn, không phải cam kết triển khai trong MVP.

### **1.2.3. Out of scope**

Các nội dung sau không thuộc cam kết của StRS đối với MVP:

* Bảo đảm hệ thống đọc và xử lý thành công mọi trang web, định dạng nhúng hoặc tài liệu PDF.  
* Bảo đảm mức cải thiện tốc độ đọc hoặc khả năng đọc hiểu bằng một tỷ lệ định lượng chưa được kiểm chứng.  
* Bảo đảm mọi ngôn ngữ đều được hỗ trợ đầy đủ và có chất lượng tương đương.  
* Cung cấp giải pháp chuyên biệt, được kiểm chứng lâm sàng hoặc chuyên môn cho một nhóm khuyết tật cụ thể.  
* Bảo đảm mô hình AI không bao giờ đưa ra câu trả lời sai.  
* Kiểm soát hoàn toàn chính sách lưu trữ hoặc sử dụng dữ liệu nội bộ của nhà cung cấp AI bên ngoài.  
* Cam kết cung cấp các chỉ tiêu vận hành như thời gian khôi phục, độ sẵn sàng hoặc thời gian phản hồi định lượng khi chưa được xác định.  
* Triển khai ứng dụng di động hoặc tiện ích mở rộng cho nhiều trình duyệt trong MVP.

## **1.3. System Overview**

### **1.3.1. System context**

Intelligent Reading Assistant hoạt động trong môi trường trình duyệt web. Người dùng đọc nội dung trên trang web, lựa chọn nội dung cần hỗ trợ hoặc đặt câu hỏi về trang đang đọc. Hệ thống cung cấp hỗ trợ phù hợp dựa trên nội dung có thể truy cập, câu hỏi, ngữ cảnh hội thoại và các tùy chọn của người dùng.

Hệ thống có mối quan hệ với các thành phần bên ngoài sau:

* **Người dùng:** Tương tác với tiện ích mở rộng, cung cấp cấu hình và quyết định cách sử dụng kết quả AI.  
* **Website nguồn:** Cung cấp nội dung mà người dùng đang đọc. Khả năng truy xuất phụ thuộc vào cấu trúc trang và giới hạn kỹ thuật.  
* **Dịch vụ AI bên ngoài:** Xử lý yêu cầu AI khi phù hợp với cấu hình và chính sách bảo vệ dữ liệu.  
* **Dịch vụ dùng chung của dự án:** Phục vụ các chức năng tài khoản, dữ liệu, lịch sử và đồng bộ.  
* **Nhóm phát triển/vận hành:** Triển khai, giám sát, bảo trì và xử lý sự cố hệ thống.  
* **Website hướng dẫn:** Cung cấp tài liệu sử dụng và hướng dẫn khắc phục vấn đề phổ biến.

### **1.3.2. Operating concept**

Quy trình sử dụng điển hình được mô tả như sau:

1. Người dùng cài đặt tiện ích mở rộng và đăng ký hoặc đăng nhập.  
2. Người dùng cấu hình API Key cho nhà cung cấp AI và thiết lập các tùy chọn cần thiết.  
3. Người dùng mở trang web và đọc nội dung như bình thường.  
4. Khi cần hỗ trợ, người dùng chọn một đoạn văn để yêu cầu giải thích, dịch thuật, tóm tắt hoặc hỏi đáp; hoặc đặt câu hỏi về toàn bộ trang trong phạm vi nội dung hệ thống có thể đọc.  
5. Hệ thống kiểm tra khả năng truy xuất nội dung và áp dụng chính sách bảo vệ dữ liệu trước khi gửi yêu cầu đến dịch vụ AI.  
6. Hệ thống trả lời dựa trên nội dung có sẵn, nêu rõ giới hạn khi thiếu bằng chứng và cho phép người dùng tiếp tục hội thoại.  
7. Hệ thống có thể đề xuất câu hỏi kiểm tra mức độ hiểu nội dung hoặc phương thức hỗ trợ khác. Người dùng có quyền chấp nhận, bỏ qua hoặc điều chỉnh.  
8. Lịch sử hội thoại và các tùy chọn được lưu theo chính sách đã xác định, cho phép người dùng quản lý và tiếp tục sử dụng trên các thiết bị đã đăng nhập.  
9. Khi xảy ra lỗi, hệ thống thông báo dễ hiểu, giữ lại dữ liệu phù hợp và yêu cầu người dùng thực hiện hành động cần thiết thay vì tự động gửi lại dữ liệu có nguy cơ vi phạm chính sách bảo vệ.

### **1.3.3. Key operating principles**

Hệ thống cần tuân thủ các nguyên tắc vận hành sau:

* Hỗ trợ đọc là mục tiêu chính; AI là phương tiện hỗ trợ, không thay thế hoàn toàn khả năng đọc hiểu và đánh giá của người dùng.  
* Nội dung nguồn cần được ưu tiên khi trả lời câu hỏi liên quan đến trang đang đọc.  
* Không được tuyên bố đã đọc hoặc xác minh nội dung khi hệ thống chưa truy xuất thành công.  
* Gợi ý chủ động không được gây cản trở việc đọc hoặc tự ý thực hiện hành động thay người dùng.  
* Quyền kiểm soát dữ liệu của người dùng là nguyên tắc xuyên suốt.  
* Các lỗi quan trọng phải được xử lý có thể hiểu và có thể khôi phục khi phù hợp.  
* Khả năng thực tế của bản demo phải được công bố trung thực.

---

# **2\. Referenced Documents**

Các tài liệu dưới đây là cơ sở tham chiếu cho StRS. Những tài liệu chưa được hoàn thiện hoặc chưa được phê duyệt chính thức cần được cập nhật phiên bản và trạng thái trước khi phát hành StRS chính thức.

| ID | Tài liệu | Mục đích tham chiếu |
| ----- | ----- | ----- |
| REF-01 | Business Requirements Specification (BRS) – Intelligent Reading Assistant | Xác định mục tiêu kinh doanh, nhu cầu và phạm vi sản phẩm. |
| REF-02 | Operational Concept (OpsCon) – Intelligent Reading Assistant | Mô tả môi trường vận hành, các vai trò và cách hệ thống được sử dụng, hỗ trợ và bảo trì. |
| REF-03 | Stakeholder Requirements Specification (StRS) template | Xác định cấu trúc và các mục nội dung của tài liệu này. |
| REF-04 | System Requirements Specification (SyRS) – dự kiến | Cụ thể hóa các yêu cầu hệ thống phát sinh từ nhu cầu stakeholder. |
| REF-05 | Software Requirements Specification (SRS) – dự kiến | Đặc tả các yêu cầu phần mềm có thể kiểm thử. |
| REF-06 | Architecture Design Document (ADD) – dự kiến | Mô tả kiến trúc và các quyết định thiết kế hệ thống. |

---

# **3\. Stakeholder Identification and Characteristics**

## **3.1. Stakeholder Profiles**

### **3.1.1. STK-01 – General readers and learners**

**Vai trò:** Nhóm stakeholder chính và đối tượng sử dụng trực tiếp.

**Đặc điểm:**

Nhóm này bao gồm người học, sinh viên, người tự học, người đọc tài liệu chuyên môn, người đọc nội dung bằng ngoại ngữ, người gặp khó khăn khi đọc hiểu và người đọc phổ thông.

Các thành viên có thể khác nhau đáng kể về trình độ đọc, vốn kiến thức nền, tốc độ đọc, mục tiêu sử dụng và nhu cầu hỗ trợ. Một người có thể đồng thời thuộc nhiều nhóm, chẳng hạn vừa là người học ngoại ngữ vừa là người thường xuyên đọc tài liệu chuyên môn.

**Nhu cầu và kỳ vọng:**

* Hiểu nội dung và khái niệm khó dễ dàng hơn.  
* Tiết kiệm thời gian xử lý tài liệu.  
* Được lựa chọn cách hỗ trợ phù hợp với từng tình huống.  
* Có thể yêu cầu trợ giúp ngay trong môi trường đọc.  
* Nhận câu trả lời hữu ích, bám sát nội dung và thể hiện giới hạn khi thông tin không đủ.  
* Kiểm soát các tùy chọn cá nhân, lịch sử và dữ liệu được gửi đến dịch vụ bên ngoài.

**Mức độ ảnh hưởng:** Rất cao. Mức độ đáp ứng nhu cầu của nhóm này quyết định giá trị và tính hữu ích của sản phẩm.

### **3.1.2. STK-02 – Development and operations team**

**Vai trò:** Bên phát triển, triển khai, vận hành và bảo trì hệ thống.

**Đặc điểm:**

Nhóm chịu trách nhiệm xây dựng sản phẩm, triển khai dịch vụ dùng chung, theo dõi tình trạng hoạt động, xử lý sự cố và duy trì khả năng sử dụng của hệ thống.

**Nhu cầu và kỳ vọng:**

* Có phạm vi MVP đủ rõ để lập kế hoạch phát triển và trình diễn.  
* Các yêu cầu có thể truy vết đến mục tiêu và kiểm thử.  
* Các lỗi tài khoản, cấu hình AI, kết nối và đồng bộ có thể được nhận biết và xử lý.  
* Có cơ chế bảo vệ dữ liệu phù hợp với trách nhiệm của sản phẩm.  
* Các giới hạn triển khai và các chức năng mô phỏng được ghi nhận rõ ràng.  
* Có thể sao lưu, khôi phục và xử lý các sự cố vận hành cơ bản.

**Mức độ ảnh hưởng:** Cao. Nhóm này chịu trách nhiệm hiện thực hóa các kỳ vọng và bảo đảm hệ thống có thể được trình diễn, kiểm thử và duy trì.

### **3.1.3. STK-03 – External AI service providers**

**Vai trò:** Bên cung cấp năng lực xử lý AI cho hệ thống.

**Đặc điểm:**

Các nhà cung cấp AI bên ngoài tiếp nhận và xử lý yêu cầu theo API, cấu hình và điều khoản dịch vụ của họ. Khả năng cung cấp dịch vụ, giới hạn sử dụng, độ trễ và chất lượng đầu ra có thể thay đổi theo nhà cung cấp.

**Nhu cầu và kỳ vọng liên quan:**

* Các yêu cầu gửi đến dịch vụ phải tuân theo giao thức và điều kiện sử dụng của nhà cung cấp.  
* Hệ thống cần xử lý các tình huống như API Key không hợp lệ, hết hạn mức, dịch vụ không khả dụng hoặc phản hồi lỗi.  
* Việc tích hợp phải cho phép nhận biết các lỗi có thể xác định được và thông báo cho người dùng.  
* Việc sử dụng dịch vụ phải tuân thủ chính sách bảo vệ dữ liệu của sản phẩm.

Nhà cung cấp AI không phải nhóm người dùng mục tiêu và không được xem là bên quyết định mục tiêu đọc hiểu của sản phẩm.

**Mức độ ảnh hưởng:** Cao đối với các chức năng AI, nhưng không quyết định toàn bộ khả năng sử dụng của trang web gốc hoặc các chức năng cục bộ.

### **3.1.4. STK-04 – Content and web platform providers**

**Vai trò:** Bên cung cấp môi trường và nội dung mà người dùng đọc.

**Đặc điểm:**

Bao gồm các website, nền tảng nội dung và nguồn tài liệu mà người dùng truy cập thông qua trình duyệt.

**Nhu cầu và kỳ vọng liên quan:**

* Tiện ích mở rộng không được mặc định giả định có thể đọc mọi nội dung.  
* Việc trích xuất nội dung phải tôn trọng các giới hạn kỹ thuật và quyền truy cập.  
* Khi không thể truy xuất nội dung, hệ thống cần thông báo thay vì tạo ấn tượng sai rằng nội dung đã được xử lý.

**Mức độ ảnh hưởng:** Trung bình đến cao, tùy vào loại website và nội dung người dùng sử dụng.

### **3.1.5. Stakeholder priority**

Thứ tự ưu tiên khi cân nhắc các nhu cầu xung đột:

1. Bảo vệ người dùng và dữ liệu của họ.  
2. Đảm bảo tính hữu ích, tính trung thực và khả năng hiểu đúng nội dung.  
3. Duy trì các luồng sử dụng chính và xử lý lỗi quan trọng.  
4. Cải thiện hiệu quả đọc và trải nghiệm tương tác.  
5. Tối ưu khả năng vận hành, bảo trì và mở rộng trong giới hạn nguồn lực của dự án.

Thứ tự này là nguyên tắc định hướng để xử lý xung đột yêu cầu, không thay thế việc đánh giá từng quyết định cụ thể.

## **3.2. User Characteristics & Ergonomic Needs**

### **3.2.1. Diversity of reading abilities and goals**

Hệ thống phải được định hướng theo nguyên tắc thiết kế bao quát, phục vụ người dùng có trình độ đọc hiểu, vốn kiến thức và mục tiêu đọc khác nhau.

Các nhóm nhu cầu tiêu biểu gồm:

* Người đọc cần giải thích từ vựng, khái niệm hoặc kiến thức nền.  
* Người đọc nhiều tài liệu và muốn nhanh chóng nắm bắt ý chính.  
* Người học ngoại ngữ cần dịch thuật, giải thích từ vựng và hỗ trợ hiểu nội dung.  
* Người gặp khó khăn khi đọc cần cách trình bày dễ tiếp cận và sự hỗ trợ phù hợp.  
* Người đọc phổ thông cần hỏi đáp hoặc làm rõ một nội dung cụ thể.

Hệ thống không được giả định rằng tất cả người dùng đều có cùng tốc độ đọc, mức độ tập trung hoặc nhu cầu hỗ trợ.

### **3.2.2. Customization and personalization**

Người dùng cần có khả năng lựa chọn, điều chỉnh và tắt những phương thức hỗ trợ không phù hợp. Hệ thống cần ghi nhớ các tùy chọn thích hợp để áp dụng trong những lần sử dụng sau.

Ngoài tùy chỉnh thủ công, hệ thống có thể đưa ra gợi ý dựa trên hành vi đọc hoặc ngữ cảnh hiện tại. Tuy nhiên:

* Gợi ý phải nhẹ nhàng và không che khuất nội dung chính.  
* Người dùng phải có quyền chấp nhận, bỏ qua hoặc điều chỉnh.  
* Hệ thống không được tự ý thực hiện hành động thay người dùng.  
* Hành vi đọc không được xem là bằng chứng chắc chắn về năng lực hoặc nhu cầu của người dùng.

### **3.2.3. Ease of use and interaction**

Hệ thống cần hỗ trợ người dùng bắt đầu sử dụng với các hướng dẫn ban đầu dễ hiểu, đặc biệt đối với đăng ký, đăng nhập, thiết lập API Key và các cấu hình cần thiết.

Trong quá trình đọc, người dùng cần có khả năng yêu cầu hỗ trợ mà không phải thực hiện quá nhiều thao tác chuyển đổi giữa các công cụ.

Các chức năng và thông báo cần dễ hiểu đối với người không có kiến thức kỹ thuật chuyên sâu. Khi xảy ra lỗi, hệ thống cần chỉ rõ vấn đề có thể xác định được và hướng xử lý phù hợp.

### **3.2.4. Language and comprehension**

Tiếng Việt là ngôn ngữ giao diện và ngôn ngữ phản hồi AI mặc định.

Hệ thống cần ưu tiên phục vụ nhu cầu đọc nội dung tiếng Việt. Với người đọc ngoại ngữ, các khả năng như dịch thuật, giải thích từ vựng và làm rõ ngữ cảnh là những nhu cầu quan trọng.

Phạm vi ngôn ngữ thực tế được hỗ trợ phụ thuộc vào năng lực triển khai của bản demo và dịch vụ AI được cấu hình. Hệ thống không được tạo kỳ vọng rằng mọi ngôn ngữ đều được hỗ trợ đầy đủ.

### **3.2.5. Accessibility and readability**

Hệ thống cần hướng đến việc giúp người dùng đọc dễ dàng hơn, duy trì việc đọc hiệu quả và tiếp cận nội dung theo cách phù hợp với nhu cầu cá nhân.

Các kỳ vọng ở cấp stakeholder bao gồm:

* Có thể nhận hỗ trợ để hiểu nội dung khó.  
* Có thể lựa chọn phương thức hỗ trợ phù hợp với mục tiêu đọc.  
* Có khả năng điều chỉnh cách trình bày khi cần.  
* Không bị các gợi ý hoặc thành phần giao diện gây cản trở việc đọc.

StRS không cam kết rằng MVP sẽ đáp ứng đầy đủ các tiêu chuẩn tiếp cận cho mọi dạng khuyết tật hoặc nhu cầu chuyên biệt. Những yêu cầu chi tiết về giao diện và khả năng tiếp cận cần được cụ thể hóa và đánh giá trong các tài liệu yêu cầu tiếp theo.

---

# **4\. Stakeholder Requirements**

## **4.1. Operational Requirements**

Các yêu cầu trong mục này mô tả những điều kiện và khả năng vận hành mà stakeholder mong muốn hệ thống đáp ứng.

| ID | Yêu cầu | Ưu tiên |
| ----- | ----- | ----- |
| STR-OPS-001 | Người dùng phải có thể sử dụng hệ thống thông qua Chrome Extension trên máy tính để bàn hoặc laptop trong phạm vi MVP. | Must |
| STR-OPS-002 | Người dùng phải có tài khoản và đăng nhập để sử dụng các chức năng phụ thuộc vào tài khoản, lịch sử và đồng bộ. | Must |
| STR-OPS-003 | Người dùng phải có thể tự cấu hình API Key và lựa chọn nhà cung cấp AI được hỗ trợ. | Must |
| STR-OPS-004 | Nhóm phát triển phải chịu trách nhiệm triển khai, giám sát, bảo trì và xử lý sự cố của các dịch vụ dùng chung. | Must |
| STR-OPS-005 | Người dùng phải có thể sử dụng website gốc bình thường khi các chức năng AI hoặc dịch vụ dùng chung gặp lỗi, trong phạm vi mà website gốc vẫn hoạt động. | Must |
| STR-OPS-006 | Khi không thể truy xuất nội dung trang hoặc tài liệu, hệ thống phải thông báo giới hạn và không được tuyên bố đã đọc nội dung đó. | Must |
| STR-OPS-007 | Hệ thống phải phân biệt lỗi cấu hình hoặc API Key của người dùng với các lỗi dịch vụ có thể xác định từ hệ thống hoặc nhà cung cấp. | Must |
| STR-OPS-008 | Hệ thống phải hỗ trợ người dùng tiếp tục xử lý các yêu cầu AI chưa hoàn thành theo cách an toàn, không tự động gửi lại dữ liệu có nguy cơ vi phạm chính sách bảo vệ. | Must |
| STR-OPS-009 | Lịch sử hội thoại đã lưu phải có thể được truy cập theo tài khoản, phù hợp với các quy tắc quản lý lịch sử và đồng bộ đã xác định. | Must |
| STR-OPS-010 | Nhóm phát triển phải có cơ chế sao lưu và phương án khôi phục dữ liệu phù hợp với khả năng vận hành của dự án. | Should |
| STR-OPS-011 | Hệ thống nên cung cấp hướng dẫn khắc phục các vấn đề thường gặp thông qua website hướng dẫn tĩnh. | Should |
| STR-OPS-012 | Hệ thống nên hỗ trợ khả năng mở rộng sang các trình duyệt và thiết bị khác trong định hướng phát triển dài hạn. | Should |

**Ghi chú:** Các mức ưu tiên được dùng để thể hiện mức độ cần thiết trong phạm vi MVP. Những yêu cầu vận hành đã được thống nhất trong OpsCon cần tiếp tục được phân rã thành các yêu cầu hệ thống và phần mềm cụ thể.

## **4.2. Functional Expectations**

Mục này mô tả các chức năng mà người dùng kỳ vọng sản phẩm cung cấp, ở mức chức năng nghiệp vụ, không quy định cách triển khai kỹ thuật.

### **4.2.1. Reading assistance**

| ID | Kỳ vọng chức năng | Ưu tiên |
| ----- | ----- | ----- |
| STR-FUN-001 | Người dùng có thể chọn một đoạn văn để yêu cầu giải thích, dịch thuật, tóm tắt hoặc hỏi đáp. | Must |
| STR-FUN-002 | Người dùng có thể đặt câu hỏi về nội dung trang đang đọc trong phạm vi hệ thống truy xuất được. | Must |
| STR-FUN-003 | Người dùng có thể tiếp tục hội thoại nhiều lượt về cùng chủ đề và ngữ cảnh đọc hiện tại. | Must |
| STR-FUN-004 | Hệ thống có thể hỗ trợ giải thích từ vựng, thuật ngữ và khái niệm khó. | Must |
| STR-FUN-005 | Hệ thống có thể tóm tắt và trích xuất các ý chính để người dùng nắm bắt nội dung nhanh hơn. | Must |
| STR-FUN-006 | Hệ thống có thể hỗ trợ dịch thuật và giải thích nội dung phục vụ việc đọc ngoại ngữ. | Must |
| STR-FUN-007 | Hệ thống có thể đề xuất câu hỏi kiểm tra mức độ hiểu nội dung sau khi người dùng đọc một lượng nội dung nhất định. | Should |
| STR-FUN-008 | Người dùng có thể trả lời, bỏ qua hoặc điều chỉnh các câu hỏi kiểm tra mức độ hiểu nội dung. | Should |

### **4.2.2. Personalization and proactive assistance**

| ID | Kỳ vọng chức năng | Ưu tiên |
| ----- | ----- | ----- |
| STR-FUN-009 | Người dùng có thể lựa chọn và điều chỉnh cách hỗ trợ đọc phù hợp với mục tiêu cá nhân. | Must |
| STR-FUN-010 | Hệ thống ghi nhớ các tùy chọn cá nhân phù hợp để sử dụng trong các lần tiếp theo. | Must |
| STR-FUN-011 | Hệ thống có thể chủ động gợi ý phương thức hỗ trợ dựa trên ngữ cảnh hoặc hành vi đọc. | Should |
| STR-FUN-012 | Người dùng có thể tắt hoặc điều chỉnh các gợi ý chủ động. | Must |
| STR-FUN-013 | Hệ thống có thể hỗ trợ điều chỉnh cách trình bày nội dung để cải thiện khả năng đọc và tiếp cận. | Should |

### **4.2.3. Account, configuration and history**

| ID | Kỳ vọng chức năng | Ưu tiên |
| ----- | ----- | ----- |
| STR-FUN-014 | Người dùng có thể đăng ký, đăng nhập và đăng xuất tài khoản. | Must |
| STR-FUN-015 | Người dùng có thể quản lý API Key cho các nhà cung cấp AI được hỗ trợ. | Must |
| STR-FUN-016 | Người dùng có thể chọn nhà cung cấp AI mặc định và chuyển nhà cung cấp cho một yêu cầu khi cần. | Must |
| STR-FUN-017 | Người dùng có thể xem, mở lại và tiếp tục các hội thoại đã lưu. | Must |
| STR-FUN-018 | Người dùng có thể xóa một hội thoại hoặc xóa toàn bộ lịch sử theo phạm vi hệ thống hỗ trợ. | Must |
| STR-FUN-019 | Người dùng có thể sử dụng lịch sử và các tùy chọn đã lưu trên thiết bị khác khi đăng nhập cùng tài khoản, trong phạm vi dữ liệu được đồng bộ. | Must |
| STR-FUN-020 | Người dùng có thể quản lý các tùy chọn liên quan đến quyền riêng tư và chế độ bảo vệ dữ liệu. | Must |

### **4.2.4. Data protection and failure handling**

| ID | Kỳ vọng chức năng | Ưu tiên |
| ----- | ----- | ----- |
| STR-FUN-021 | Hệ thống phải có chế độ bảo vệ dữ liệu được bật mặc định và cho phép người dùng chủ động thay đổi trạng thái theo quy tắc đã xác định. | Must |
| STR-FUN-022 | Khi nội dung có dấu hiệu nhạy cảm hoặc chưa thể xác định mức độ nhạy cảm, hệ thống phải ngăn gửi dữ liệu ra ngoài theo chính sách bảo vệ và thông báo cho người dùng. | Must |
| STR-FUN-023 | Hệ thống phải cho phép người dùng xem xét, chỉnh sửa hoặc loại bỏ nội dung nhạy cảm trước khi thử lại yêu cầu bị chặn. | Must |
| STR-FUN-024 | Việc tắt chế độ bảo vệ phải đi kèm cảnh báo rủi ro và xác nhận chủ động của người dùng. | Must |
| STR-FUN-025 | Hệ thống không được tự động gửi lại yêu cầu bị chặn do chính sách bảo vệ dữ liệu. | Must |
| STR-FUN-026 | Hệ thống phải thông báo các lỗi quan trọng bằng ngôn ngữ dễ hiểu và cung cấp hướng xử lý phù hợp khi có thể. | Must |
| STR-FUN-027 | Hệ thống phải thông báo khi xảy ra lỗi đồng bộ và bảo vệ dữ liệu đã lưu trên thiết bị trong phạm vi hệ thống kiểm soát được. | Must |
| STR-FUN-028 | Hệ thống phải tách biệt dữ liệu làm việc của các tài khoản khi người dùng chuyển đổi tài khoản trên cùng trình duyệt. | Must |
| STR-FUN-029 | Hệ thống phải cung cấp phản hồi về mức độ không chắc chắn hoặc thiếu bằng chứng khi AI không có đủ thông tin để trả lời đáng tin cậy. | Must |

Các yêu cầu về phát hiện dữ liệu nhạy cảm không hàm ý rằng hệ thống có thể nhận diện hoàn hảo mọi loại dữ liệu nhạy cảm. Mức độ phát hiện, các trường hợp ngoại lệ và chính sách xử lý phải được cụ thể hóa trong SyRS/SRS.

## **4.3. Quality / Non-Functional Expectations**

### **4.3.1. Quality expectations**

| ID | Kỳ vọng chất lượng | Ưu tiên |
| ----- | ----- | ----- |
| STR-QLT-001 | Câu trả lời liên quan đến nội dung đang đọc phải ưu tiên căn cứ từ nội dung nguồn có thể truy xuất. | Must |
| STR-QLT-002 | Hệ thống phải phân biệt thông tin được hỗ trợ bởi nguồn hiện tại với kiến thức nền bổ sung của AI khi sự phân biệt này có ý nghĩa đối với người đọc. | Must |
| STR-QLT-003 | Khi thiếu bằng chứng hoặc không đủ ngữ cảnh, hệ thống phải thể hiện giới hạn thay vì trình bày suy đoán như thông tin đã được xác minh. | Must |
| STR-QLT-004 | Câu trả lời nên phù hợp với mục tiêu, mức độ chi tiết và sở thích của người dùng. | Should |
| STR-QLT-005 | Hệ thống nên giảm số thao tác chuyển đổi giữa trang đang đọc và công cụ hỗ trợ. | Should |
| STR-QLT-006 | Gợi ý chủ động không được che khuất nội dung quan trọng hoặc gây gián đoạn đáng kể cho việc đọc. | Must |
| STR-QLT-007 | Thông báo lỗi phải dễ hiểu đối với người dùng không có chuyên môn kỹ thuật. | Must |
| STR-QLT-008 | Hệ thống phải xử lý các lỗi quan trọng theo cách có thể hiểu, không âm thầm mất dữ liệu và không tạo ra trạng thái sử dụng gây hiểu nhầm. | Must |
| STR-QLT-009 | Hệ thống phải hướng đến khả năng sử dụng với nhiều trình độ đọc hiểu và nhu cầu khác nhau. | Must |
| STR-QLT-010 | Các chức năng chính nên duy trì tính hữu ích trong giới hạn cho phép khi một thành phần phụ thuộc không khả dụng. | Should |

### **4.3.2. Performance**

Hệ thống cần cung cấp phản hồi đủ kịp thời để không làm giảm đáng kể tính liên tục của trải nghiệm đọc.

Tuy nhiên, tại thời điểm lập tài liệu này, chưa có cơ sở để xác định các chỉ tiêu định lượng về thời gian phản hồi, độ trễ tối đa, thông lượng hoặc số lượng người dùng đồng thời.

Các chỉ tiêu hiệu năng cần được xác định sau khi nhóm thống nhất môi trường thử nghiệm, cấu hình dịch vụ AI và phạm vi demo. Độ trễ của nhà cung cấp AI bên ngoài cần được xem xét riêng với độ trễ do chính hệ thống gây ra.

### **4.3.3. Security and privacy**

Quyền riêng tư là nguyên tắc thiết kế cốt lõi của sản phẩm.

Các kỳ vọng bao gồm:

* Dữ liệu chỉ được thu thập, lưu trữ, sử dụng và truyền đi trong phạm vi chức năng và chính sách đã xác định.  
* Nội dung trang, đoạn văn được chọn, câu hỏi và ngữ cảnh hội thoại có khả năng chứa dữ liệu nhạy cảm phải thuộc phạm vi xem xét của chế độ bảo vệ.  
* Khi chế độ bảo vệ được bật và không thể xác định an toàn mức độ nhạy cảm, hệ thống phải mặc định ngăn truyền nội dung liên quan ra ngoài và yêu cầu người dùng xem xét.  
* Việc tắt chế độ bảo vệ phải có cảnh báo và xác nhận rõ ràng.  
* Việc bị chặn không được dẫn đến gửi lại dữ liệu ngầm hoặc tự động gửi khi kết nối được khôi phục.  
* Dữ liệu của các tài khoản phải được cô lập theo đúng phạm vi truy cập.  
* Người dùng phải có khả năng quản lý lịch sử và các tùy chọn dữ liệu theo phạm vi hệ thống hỗ trợ.  
* Các thông tin xác thực và API Key phải được bảo vệ theo yêu cầu an toàn tương ứng.

Hệ thống chỉ có thể cam kết về các luồng dữ liệu nằm trong phạm vi kiểm soát của sản phẩm. Chính sách lưu trữ, sử dụng hoặc huấn luyện mô hình của nhà cung cấp AI bên ngoài cần được xem xét theo điều khoản và chính sách riêng của nhà cung cấp; StRS không mặc định cam kết kiểm soát được các hoạt động nội bộ đó.

### **4.3.4. Usability and accessibility**

Hệ thống cần hỗ trợ người dùng sử dụng chức năng đọc hiểu mà không cần hiểu sâu về kỹ thuật AI.

Các kỳ vọng chính gồm:

* Quy trình bắt đầu sử dụng dễ hiểu.  
* Các lựa chọn hỗ trợ có thể được tùy chỉnh.  
* Các thông báo về lỗi, giới hạn truy xuất nội dung và giới hạn của AI dễ hiểu.  
* Các gợi ý chủ động có thể được điều chỉnh hoặc tắt.  
* Cách trình bày hỗ trợ khả năng đọc và tiếp cận trong phạm vi tính năng được triển khai.  
* Các chức năng được giới thiệu phải phản ánh đúng trạng thái triển khai thực tế.

Không đặt ra cam kết tuân thủ một tiêu chuẩn tiếp cận cụ thể hoặc một mức điểm khả dụng định lượng khi chưa có tiêu chuẩn và kế hoạch đánh giá được phê duyệt.

## **4.4. Environmental & Integration Expectations**

### **4.4.1. Operating environment**

| ID | Kỳ vọng môi trường | Ưu tiên |
| ----- | ----- | ----- |
| STR-ENV-001 | MVP phải hỗ trợ môi trường Chrome trên máy tính để bàn hoặc laptop. | Must |
| STR-ENV-002 | Hệ thống nên có khả năng mở rộng sang các trình duyệt và thiết bị khác trong định hướng dài hạn. | Should |
| STR-ENV-003 | Người dùng có thể đọc nội dung trên các website mà trình duyệt truy cập được, trong giới hạn kỹ thuật và quyền truy cập. | Must |
| STR-ENV-004 | Hệ thống nên hỗ trợ các định dạng nội dung nhúng phổ biến, bao gồm PDF, khi có thể trích xuất nội dung. | Should |
| STR-ENV-005 | Khi nội dung không thể được trích xuất, hệ thống phải thông báo giới hạn thay vì tạo câu trả lời dựa trên giả định rằng nội dung đã được đọc. | Must |

### **4.4.2. External service integration**

Hệ thống cần tương tác với các dịch vụ bên ngoài và thành phần liên quan theo các kỳ vọng sau:

* Tích hợp với các nhà cung cấp AI được hỗ trợ thông qua cấu hình của người dùng.  
* Cho phép kiểm tra API Key mới trước khi xác nhận thay thế cấu hình hiện tại.  
* Nhận diện và thông báo lỗi API Key, giới hạn sử dụng hoặc lỗi dịch vụ khi có đủ thông tin.  
* Cho phép các nhà cung cấp AI khác nhau được cấu hình độc lập theo phạm vi sản phẩm.  
* Không tự động gửi lại các yêu cầu thất bại nếu việc gửi lại có thể vi phạm chính sách bảo vệ dữ liệu.  
* Hỗ trợ các dịch vụ dùng chung cần thiết cho tài khoản, lưu trữ và đồng bộ.  
* Phân biệt trạng thái của dịch vụ AI với trạng thái hoạt động của website gốc và các chức năng khác không phụ thuộc trực tiếp vào dịch vụ đó.

### **4.4.3. Data synchronization and session context**

Hệ thống cần đáp ứng các kỳ vọng sau:

* Lịch sử hội thoại đã lưu được gắn với tài khoản và có thể đồng bộ giữa các thiết bị.  
* Ngữ cảnh đọc đang hoạt động được quản lý theo từng tab hoặc phiên đọc; không tự động trộn ngữ cảnh giữa các tab.  
* Khi mở lại tiện ích mở rộng, hệ thống khôi phục hội thoại gần nhất theo phạm vi đã thống nhất nhưng không tự động khôi phục trang đọc hoặc ngữ cảnh đọc cũ.  
* Khi mở lại một hội thoại cũ, hệ thống không tự động truy xuất lại nội dung trang cũ; người dùng có thể chủ động cung cấp hoặc khôi phục ngữ cảnh.  
* Yêu cầu chưa được xử lý có thể được giữ tạm thời trên thiết bị hiện tại để người dùng xử lý tiếp; không tự động đồng bộ các yêu cầu đang chờ gửi.  
* Khi xảy ra xung đột đồng bộ, hệ thống phải bảo toàn dữ liệu có thể bảo toàn, thông báo và cho phép người dùng quyết định thay vì âm thầm ghi đè.

---

# **5\. Constraints**

Các ràng buộc dưới đây giới hạn phạm vi mà nhóm dự án có thể cam kết với stakeholder. Chúng không nhất thiết là quyết định kiến trúc cuối cùng.

## **5.1. Scope and schedule constraints**

| ID | Ràng buộc | Ảnh hưởng |
| ----- | ----- | ----- |
| STR-CON-001 | MVP tập trung vào Chrome Extension trên máy tính để bàn/laptop. | Giới hạn phạm vi triển khai và kiểm thử đa nền tảng. |
| STR-CON-002 | Nhóm hướng đến triển khai các nhóm tính năng đã xác định, nhưng độ hoàn thiện giữa các chức năng có thể khác nhau. | Cần công bố rõ trạng thái triển khai thực tế, mô phỏng hoặc chưa hoàn thiện. |
| STR-CON-003 | Không cam kết tối ưu chuyên sâu cho mọi nhóm người dùng trong MVP. | Cần chọn các tình huống đại diện để triển khai và đánh giá. |
| STR-CON-004 | Chưa có thời hạn hoàn thành chi tiết được xác nhận trong StRS. | Kế hoạch và mốc phát hành cần được quản lý trong kế hoạch dự án. |

## **5.2. Technology and integration constraints**

| ID | Ràng buộc | Ảnh hưởng |
| ----- | ----- | ----- |
| STR-CON-005 | Khả năng xử lý AI phụ thuộc vào nhà cung cấp được tích hợp, cấu hình người dùng và trạng thái dịch vụ. | Không thể cam kết AI luôn sẵn sàng hoặc có độ trễ cố định. |
| STR-CON-006 | Người dùng cung cấp API Key để sử dụng các chức năng AI yêu cầu cấu hình này. | Lỗi cấu hình và hạn mức có thể ảnh hưởng đến khả năng sử dụng AI. |
| STR-CON-007 | Khả năng đọc nội dung phụ thuộc vào website, định dạng tài liệu và khả năng trích xuất thực tế. | Không thể bảo đảm xử lý thành công mọi nguồn nội dung. |
| STR-CON-008 | Hỗ trợ đa ngôn ngữ phụ thuộc vào phạm vi triển khai và khả năng của dịch vụ AI. | Không thể cam kết chất lượng đồng đều trên mọi ngôn ngữ. |
| STR-CON-009 | Các cam kết quyền riêng tư đối với nhà cung cấp bên ngoài bị giới hạn bởi phạm vi kiểm soát của sản phẩm và chính sách nhà cung cấp. | Cần phân biệt rõ trách nhiệm của sản phẩm với hoạt động nội bộ của dịch vụ bên ngoài. |

## **5.3. Budget and resource constraints**

Tại thời điểm lập tài liệu:

* Chưa có ngân sách được xác nhận.  
* Chưa có giới hạn chi phí vận hành định lượng được thống nhất.  
* Chưa có mục tiêu định lượng về hiệu năng hoặc quy mô người dùng.  
* Chưa có yêu cầu RTO/RPO cụ thể cho sao lưu và khôi phục.  
* Chưa có phạm vi triển khai chi tiết cho từng tính năng trong toàn bộ nhóm tính năng đã thảo luận.

Do đó, những nội dung trên cần được xác nhận trong kế hoạch dự án và các tài liệu yêu cầu hoặc thiết kế tiếp theo. StRS không tự đặt ra giá trị giả định cho các thông số này.

---

# **6\. Stakeholder Risks**

Các rủi ro dưới đây được xác định từ góc nhìn stakeholder, tập trung vào ảnh hưởng đối với người dùng và khả năng hoàn thành mục tiêu của dự án.

| ID | Rủi ro | Khả năng / ảnh hưởng | Biện pháp định hướng |
| ----- | ----- | ----- | ----- |
| STR-RSK-001 | AI giải thích sai, tạo thông tin không có căn cứ hoặc khiến người dùng hiểu sai nội dung nguồn. | Khả năng: chưa định lượng. Ảnh hưởng: cao. | Ưu tiên nguồn, thể hiện giới hạn bằng chứng, xây dựng bộ kịch bản đánh giá chất lượng AI. |
| STR-RSK-002 | Dịch vụ AI bên ngoài chậm, không khả dụng hoặc bị giới hạn sử dụng. | Khả năng: phụ thuộc nhà cung cấp. Ảnh hưởng: cao đối với chức năng AI. | Thông báo lỗi rõ ràng, giữ yêu cầu chưa xử lý theo chính sách an toàn, cho phép người dùng thử lại chủ động. |
| STR-RSK-003 | Khả năng thực tế của bản demo thấp hơn kỳ vọng tạo ra bởi danh sách tính năng được giới thiệu. | Khả năng: cần theo dõi trong quá trình phát triển. Ảnh hưởng: cao. | Theo dõi trạng thái triển khai, công bố giới hạn và phân biệt chức năng thực tế với chức năng mô phỏng. |
| STR-RSK-004 | Nội dung nhạy cảm bị gửi ra ngoài do nhận diện sai, chính sách bảo vệ không được áp dụng đúng hoặc luồng gửi dữ liệu bị lỗi. | Khả năng: chưa định lượng. Ảnh hưởng: rất cao. | Bật chế độ bảo vệ mặc định, chặn khi không chắc chắn, không tự động gửi lại, ưu tiên kiểm thử các trường hợp rủi ro cao. |
| STR-RSK-005 | Hệ thống không trích xuất được nội dung nhưng người dùng hiểu nhầm rằng câu trả lời AI đã dựa trên nội dung đó. | Khả năng: có thể xảy ra tùy nguồn. Ảnh hưởng: cao. | Thông báo thất bại khi trích xuất và phân biệt nội dung nguồn với thông tin do người dùng cung cấp hoặc kiến thức nền. |
| STR-RSK-006 | Các chức năng hỗ trợ hoặc gợi ý chủ động làm gián đoạn việc đọc. | Khả năng: chưa định lượng. Ảnh hưởng: trung bình đến cao. | Thiết kế gợi ý không gây cản trở, cho phép bỏ qua và điều chỉnh mức độ chủ động. |
| STR-RSK-007 | Lỗi đồng bộ hoặc xung đột dữ liệu khiến người dùng mất dữ liệu hoặc thấy dữ liệu không nhất quán. | Khả năng: chưa định lượng. Ảnh hưởng: cao. | Bảo toàn dữ liệu hiện có, thông báo xung đột, tránh ghi đè âm thầm và hỗ trợ thử lại đồng bộ phù hợp. |
| STR-RSK-008 | Khác biệt về trình độ đọc hiểu và nhu cầu khiến một số nhóm không nhận được đủ lợi ích từ bản demo. | Khả năng: cao do phạm vi người dùng đa dạng. Ảnh hưởng: trung bình đến cao. | Cho phép tùy chỉnh, lựa chọn tình huống thử nghiệm đại diện và thu thập phản hồi từ người dùng. |
| STR-RSK-009 | API Key không hợp lệ, hết hạn mức hoặc cấu hình sai làm gián đoạn trải nghiệm. | Khả năng: có thể xảy ra. Ảnh hưởng: trung bình đến cao. | Kiểm tra cấu hình khi phù hợp, thông báo nguyên nhân nhận diện được và hướng dẫn người dùng khắc phục. |
| STR-RSK-010 | Kết quả đánh giá không phản ánh được tính hữu ích thực tế do chỉ kiểm thử theo kịch bản nội bộ. | Khả năng: có thể xảy ra nếu không có người dùng tham gia. Ảnh hưởng: trung bình. | Kết hợp kiểm thử theo kịch bản với trải nghiệm thử của một số người dùng thực tế. |

**Lưu ý về đánh giá rủi ro:** Các mức khả năng xảy ra chưa được định lượng do chưa có dữ liệu vận hành. Chúng cần được xem xét lại khi có kết quả kiểm thử, dữ liệu thực nghiệm và thông tin về môi trường triển khai.

---

# **7\. Verification Criteria**

Mục này xác định cách đánh giá liệu các nhu cầu và kỳ vọng của stakeholder đã được đáp ứng hay chưa. Các tiêu chí dưới đây là tiêu chí xác minh ở cấp stakeholder; các bước kiểm thử chi tiết, dữ liệu đầu vào, điều kiện tiên quyết và kết quả mong đợi cho từng trường hợp sẽ được xây dựng trong kế hoạch kiểm thử và tài liệu yêu cầu phần mềm.

## **7.1. VC-01 – Main workflow verification**

**Mục tiêu:** Xác minh các luồng sử dụng chính có thể được thực hiện trong phạm vi demo.

**Phương pháp:** Kiểm thử theo kịch bản.

**Các nội dung cần đánh giá:**

* Người dùng có thể đăng ký, đăng nhập và cấu hình API Key theo phạm vi triển khai.  
* Người dùng có thể yêu cầu hỗ trợ đọc từ một đoạn văn được chọn hoặc nội dung trang hiện tại khi nội dung có thể truy xuất.  
* Người dùng có thể tiếp tục hội thoại trong cùng ngữ cảnh đọc.  
* Người dùng có thể sử dụng các chức năng đọc hiểu được công bố là đã triển khai.  
* Người dùng có thể quản lý lịch sử và các tùy chọn trong phạm vi chức năng thực tế.  
* Người dùng có thể thực hiện các thao tác được hỗ trợ mà không cần rời khỏi môi trường đọc một cách không cần thiết.

**Tiêu chí đạt:**

Các luồng chính đã chọn có thể hoàn thành theo kịch bản, và các giới hạn của chức năng chưa triển khai được ghi nhận rõ ràng.

## **7.2. VC-02 – Error handling and recoverability**

**Mục tiêu:** Xác minh hệ thống xử lý phù hợp những lỗi quan trọng thay vì chỉ hoạt động trong điều kiện lý tưởng.

**Phương pháp:** Kiểm thử các tình huống lỗi và khôi phục.

**Các nội dung cần đánh giá:**

* API Key không hợp lệ hoặc nhà cung cấp AI không khả dụng.  
* Mất kết nối mạng hoặc dịch vụ dùng chung bị gián đoạn.  
* Không thể trích xuất nội dung trang hoặc tài liệu.  
* Đồng bộ dữ liệu thất bại hoặc phát hiện xung đột.  
* Phiên đăng nhập hết hạn hoặc người dùng chuyển tài khoản.  
* Yêu cầu bị chặn bởi chính sách bảo vệ dữ liệu.  
* Yêu cầu AI không đủ bằng chứng để trả lời đáng tin cậy.

**Tiêu chí đạt:**

Hệ thống đưa ra thông báo dễ hiểu, không tuyên bố thành công khi thao tác thất bại, bảo toàn dữ liệu phù hợp trong phạm vi kiểm soát và không tự động thực hiện hành động có nguy cơ vi phạm chính sách bảo vệ.

## **7.3. VC-03 – Reading experience and usability**

**Mục tiêu:** Xác minh hệ thống hỗ trợ người dùng đọc và hiểu nội dung thuận tiện hơn.

**Phương pháp:** Quan sát quá trình sử dụng và thu thập phản hồi.

**Các nội dung cần đánh giá:**

* Người dùng có thể yêu cầu hỗ trợ đọc trong môi trường trình duyệt.  
* Các thao tác hỗ trợ có dễ hiểu và phù hợp với mục tiêu đọc hay không.  
* Gợi ý chủ động có gây cản trở hoặc che khuất nội dung hay không.  
* Người dùng có thể điều chỉnh hoặc bỏ qua các gợi ý.  
* Người dùng có nhận thấy lợi ích trong việc hiểu nội dung khó hoặc tiết kiệm thời gian xử lý tài liệu hay không.  
* Các chức năng có phù hợp với những nhu cầu đọc khác nhau trong phạm vi thử nghiệm hay không.

**Tiêu chí đạt:**

Người tham gia có thể thực hiện các tác vụ được giao với mức độ hỗ trợ phù hợp; những vấn đề đáng kể về thao tác, mức độ gián đoạn và tính hữu ích được ghi nhận để cải thiện.

Không đặt mục tiêu tăng tốc độ đọc theo phần trăm nếu chưa có phương pháp đo và dữ liệu nền phù hợp.

## **7.4. VC-04 – AI answer quality and source grounding**

**Mục tiêu:** Xác minh chất lượng câu trả lời AI đối với các tình huống đọc hiểu.

**Phương pháp:** Đánh giá trên tập kịch bản kiểm thử được xây dựng cho demo.

**Các nội dung cần đánh giá:**

* Mức độ phù hợp của câu trả lời với câu hỏi.  
* Mức độ bám sát nội dung nguồn khi người dùng hỏi về trang đang đọc.  
* Khả năng phân biệt thông tin có căn cứ từ nguồn với kiến thức nền bổ sung.  
* Khả năng thể hiện sự không chắc chắn hoặc thừa nhận thiếu thông tin.  
* Khả năng phản hồi phù hợp khi nội dung nguồn không đầy đủ hoặc không truy xuất được.  
* Mức độ hữu ích của giải thích, tóm tắt và hỗ trợ đọc đối với mục tiêu của từng kịch bản.

**Tiêu chí đạt:**

Các kịch bản kiểm thử phải cho phép phát hiện câu trả lời sai lệch nguồn, không phù hợp hoặc thể hiện mức độ chắc chắn không tương xứng với bằng chứng. Kết quả đánh giá và các trường hợp thất bại phải được ghi nhận.

Ngưỡng chấp nhận định lượng và phương pháp chấm điểm cần được xác định trong kế hoạch đánh giá AI; chưa được mặc định trong StRS.

## **7.5. VC-05 – Privacy and data protection**

**Mục tiêu:** Xác minh các kỳ vọng về bảo vệ dữ liệu được thực hiện trong những luồng thuộc phạm vi kiểm soát của sản phẩm.

**Phương pháp:** Kiểm thử theo kịch bản, kiểm tra luồng dữ liệu và kiểm tra hành vi khi chính sách bảo vệ được kích hoạt.

**Các nội dung cần đánh giá:**

* Chế độ bảo vệ được bật mặc định theo yêu cầu.  
* Khi nội dung nhạy cảm hoặc chưa xác định được độ nhạy cảm, dữ liệu liên quan bị chặn gửi ra ngoài theo chính sách.  
* Người dùng nhận được thông báo và có khả năng xem xét nội dung bị chặn.  
* Việc gửi lại yêu cầu bị chặn chỉ xảy ra sau hành động chủ động phù hợp của người dùng.  
* Tắt chế độ bảo vệ phải có cảnh báo và xác nhận.  
* Các yêu cầu đang chờ không được tự động gửi đi khi kết nối được khôi phục nếu điều đó có thể vi phạm chính sách bảo vệ.  
* Dữ liệu làm việc của tài khoản này không bị lộ cho tài khoản khác trên cùng trình duyệt.  
* Việc xóa lịch sử và xử lý dữ liệu tuân theo phạm vi đã thông báo cho người dùng.

**Tiêu chí đạt:**

Không có trường hợp kiểm thử bắt buộc nào cho thấy hệ thống tự ý gửi nội dung đã bị chặn hoặc để lộ dữ liệu giữa các tài khoản trong những luồng được kiểm thử. Mọi giới hạn phát hiện dữ liệu nhạy cảm phải được ghi nhận; kết quả kiểm thử không được diễn giải thành cam kết rằng hệ thống nhận diện được mọi dữ liệu nhạy cảm.

## **7.6. VC-06 – Real-user feedback**

**Mục tiêu:** Thu thập bằng chứng về tính hữu ích và trải nghiệm sử dụng từ người dùng thực tế.

**Phương pháp:** Mời một số người dùng phù hợp trải nghiệm bản demo và cung cấp phản hồi.

**Các nội dung cần đánh giá:**

* Người dùng có hiểu cách sử dụng những chức năng chính hay không.  
* Các chức năng hỗ trợ có giúp họ hiểu nội dung hoặc xử lý tài liệu hiệu quả hơn theo mục tiêu cá nhân hay không.  
* Những điểm gây khó hiểu, thao tác thừa hoặc gián đoạn khi đọc.  
* Mức độ tin cậy của người dùng đối với câu trả lời AI và khả năng nhận biết giới hạn của câu trả lời.  
* Những tính năng có giá trị cao và những điểm cần cải thiện.

**Tiêu chí đạt:**

Phản hồi được ghi nhận và tổng hợp thành các vấn đề, điểm mạnh và đề xuất cải tiến. Kết quả thử nghiệm với một số người dùng chỉ cung cấp bằng chứng ban đầu về trải nghiệm, không mặc định đại diện cho toàn bộ người dùng mục tiêu.

## **7.7. VC-07 – Demo transparency and scope compliance**

**Mục tiêu:** Đảm bảo bản demo thể hiện trung thực khả năng thực tế của sản phẩm.

**Phương pháp:** Đối chiếu danh sách tính năng đã công bố với trạng thái triển khai và kết quả trình diễn.

**Các nội dung cần đánh giá:**

* Những chức năng được công bố là đã triển khai có thể được kiểm tra hoặc trình diễn theo phạm vi đã nêu.  
* Những chức năng chỉ mô phỏng hoặc chưa hoàn thiện được ghi nhận rõ ràng.  
* Các giới hạn về ngôn ngữ, loại nội dung, tích hợp và chất lượng AI không bị che giấu.  
* Những nhánh lỗi quan trọng được xử lý hoặc mô phỏng có chủ đích và được phân biệt với khả năng vận hành thực tế.

**Tiêu chí đạt:**

Danh sách trạng thái tính năng và kết quả trình diễn nhất quán với khả năng thực tế của bản demo; không đưa ra tuyên bố vượt quá bằng chứng kiểm thử.

## **7.8. Verification summary**

Việc xác minh StRS được xem là hoàn thành khi:

1. Các luồng chính trong phạm vi demo được kiểm thử và có kết quả ghi nhận.  
2. Những nhánh lỗi quan trọng có hành vi xử lý và thông báo phù hợp.  
3. Các kỳ vọng về chất lượng câu trả lời và mức độ bám sát nguồn được đánh giá theo kịch bản.  
4. Các luồng bảo vệ dữ liệu quan trọng được kiểm thử.  
5. Trải nghiệm của một số người dùng thực tế được thu thập và tổng hợp.  
6. Trạng thái triển khai, mô phỏng và giới hạn của demo được công bố rõ ràng.  
7. Các vấn đề chưa đáp ứng được ghi nhận để chuyển thành công việc cải tiến hoặc yêu cầu cho giai đoạn tiếp theo.

Việc đáp ứng StRS không có nghĩa hệ thống đã đạt chất lượng thương mại hoặc hỗ trợ chuyên sâu mọi nhóm người dùng. StRS cung cấp cơ sở đánh giá mức độ đáp ứng các nhu cầu stakeholder trong phạm vi đã thống nhất của dự án.

