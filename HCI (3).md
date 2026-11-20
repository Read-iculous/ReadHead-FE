# **SYSTEM REQUIREMENTS SPECIFICATION (SyRS)**

## **Intelligent Reading Assistant**

**Phiên bản:** Draft 0.1  
**Trạng thái:** Bản dự thảo – chờ rà soát  
**Loại tài liệu:** System Requirements Specification (SyRS)  
**Chuẩn tham chiếu:** ISO/IEC/IEEE 29148:2018  
**Phạm vi triển khai:** MVP phục vụ trình diễn và đánh giá hệ thống

---

# **1\. Introduction**

## **1.1. Purpose**

Tài liệu System Requirements Specification (SyRS) xác định các yêu cầu đối với hệ thống Intelligent Reading Assistant, bao gồm các chức năng, trạng thái vận hành, giao diện bên ngoài, đặc tính chất lượng, ràng buộc thiết kế và phương án kiểm chứng ở cấp hệ thống.

Tài liệu được sử dụng làm cơ sở để:

* Xác định ranh giới và trách nhiệm của các thành phần hệ thống.  
* Làm rõ những hành vi mà hệ thống phải cung cấp cho người dùng và các hệ thống tích hợp.  
* Định hướng thiết kế kiến trúc hệ thống trong tài liệu Architecture Design Document (ADD).  
* Làm cơ sở phân rã yêu cầu phần mềm trong Software Requirements Specification (SRS) và xây dựng kế hoạch kiểm thử.  
* Đánh giá mức độ hoàn thành của MVP, bao gồm cả những chức năng được triển khai thực tế và những chức năng được mô phỏng trong bản demo.

Các yêu cầu trong tài liệu phải có mã định danh duy nhất, rõ ràng, nhất quán, khả thi và có thể kiểm chứng. Những thông số chưa có căn cứ sẽ được ghi nhận là chưa xác định thay vì tự đặt ra giá trị định lượng.

## **1.2. Scope**

Intelligent Reading Assistant là hệ thống hỗ trợ người dùng đọc, hiểu và khai thác nội dung trên trình duyệt bằng cách kết hợp tương tác đọc với các năng lực AI.

Hệ thống hướng đến việc hỗ trợ người dùng:

* Hiểu từ vựng, thuật ngữ, khái niệm và ngữ cảnh.  
* Đặt câu hỏi về đoạn văn hoặc nội dung của trang đang đọc.  
* Tóm tắt và trích xuất ý chính.  
* Dịch nội dung.  
* Tạo câu hỏi kiểm tra mức độ hiểu bài và cho phép người dùng trả lời, bỏ qua hoặc điều chỉnh.  
* Nhận gợi ý hỗ trợ đọc dựa trên ngữ cảnh và hành vi đọc, trong phạm vi dữ liệu hệ thống có thể truy cập.  
* Cá nhân hóa cách giải thích, độ chi tiết và phong cách phản hồi.  
* Sử dụng các chức năng hỗ trợ khả năng tiếp cận và định dạng lại nội dung trong phạm vi MVP thực hiện được.  
* Quản lý hội thoại, lịch sử đọc, cấu hình và API Key của nhà cung cấp AI.  
* Tiếp tục sử dụng các chức năng đọc cơ bản khi chức năng AI hoặc dịch vụ dùng chung không khả dụng, nếu các chức năng đó không phụ thuộc vào dịch vụ bị lỗi.

**Ranh giới hệ thống**

Các thành phần chính nằm trong phạm vi SyRS:

1. **Chrome Extension:** giao diện tương tác chính với người dùng; quản lý ngữ cảnh đọc theo tab; trích xuất nội dung có thể truy cập; tiếp nhận yêu cầu và hiển thị kết quả.  
2. **Backend dùng chung:** quản lý tài khoản, lịch sử hội thoại đã lưu, cấu hình, đồng bộ và dữ liệu hệ thống thuộc trách nhiệm của Backend.  
3. **AI Service:** điều phối các luồng xử lý AI, quản lý ngữ cảnh được phép sử dụng và tương tác với nhà cung cấp mô hình.  
4. **Các hệ thống bên ngoài:** Chrome, nội dung web/PDF và API của các nhà cung cấp AI được cấu hình.

Website hướng dẫn/onboarding tĩnh nằm ngoài ranh giới hệ thống chính. Website này phục vụ hướng dẫn sử dụng, xử lý sự cố thường gặp và cung cấp thông tin hỗ trợ; việc xây dựng website không phải trọng tâm của SyRS.

MVP ưu tiên Chrome trên máy tính để bàn và laptop. Khả năng hỗ trợ nội dung web, văn bản nhúng và PDF phụ thuộc vào khả năng truy cập và trích xuất thực tế; hệ thống không cam kết đọc được mọi định dạng hoặc mọi trang web.

Tài liệu không quy định chi tiết cấu trúc mã nguồn, API endpoint, thuật toán nội bộ, công nghệ lưu trữ hoặc mô hình AI cụ thể. Các nội dung này thuộc ADD và SRS.

## **1.3. System Overview and Context**

### **1.3.1. System context**

Hệ thống hoạt động trong môi trường trình duyệt của người dùng, kết hợp với các dịch vụ dùng chung do nhóm phát triển triển khai và vận hành.

Luồng tương tác tổng quát:

1. Người dùng mở Chrome Extension và đăng nhập.  
2. Extension khôi phục hội thoại gần nhất theo quy định về phiên làm việc.  
3. Người dùng mở trang web hoặc PDF và yêu cầu hỗ trợ đọc.  
4. Extension trích xuất nội dung có thể truy cập và chuẩn bị ngữ cảnh liên quan.  
5. Hệ thống kiểm tra chế độ bảo vệ và xác định dữ liệu có được phép truyền tới dịch vụ AI bên ngoài hay không.  
6. Nếu được phép, yêu cầu được chuyển qua AI Service để xử lý bằng nhà cung cấp AI đã cấu hình.  
7. Kết quả được trả về Extension để hiển thị cho người dùng.  
8. Lịch sử và các dữ liệu thuộc phạm vi lưu trữ được lưu theo tài khoản và đồng bộ khi điều kiện cho phép.

Nếu không thể trích xuất nội dung, không thể kết nối dịch vụ hoặc yêu cầu bị chế độ bảo vệ chặn, hệ thống phải thông báo trạng thái tương ứng và không được giả định rằng thao tác AI đã thành công.

### **1.3.2. System elements**

| Thành phần | Trách nhiệm hệ thống |
| ----- | ----- |
| Chrome Extension | Tương tác người dùng; trích xuất nội dung; quản lý ngữ cảnh theo tab; kiểm tra điều kiện trước khi gửi; hiển thị kết quả và lỗi. |
| Backend | Xác thực tài khoản; quản lý lịch sử, cấu hình và dữ liệu đồng bộ; cô lập dữ liệu giữa các tài khoản. |
| AI Service | Điều phối xử lý AI; sử dụng ngữ cảnh được phép gửi; gọi nhà cung cấp mô hình phù hợp; trả kết quả và thông tin lỗi. |
| Nhà cung cấp AI | Xử lý yêu cầu mô hình thông qua API được hỗ trợ; trả kết quả, lỗi và thông tin sử dụng nếu có. |
| Chrome và nội dung trang | Cung cấp môi trường thực thi Extension và nội dung người dùng đang đọc trong phạm vi trình duyệt cho phép. |

### **1.3.3. Operational assumptions**

* Người dùng có Chrome tương thích và kết nối Internet khi sử dụng chức năng cần Backend hoặc AI Service.  
* Người dùng có tài khoản và cung cấp API Key hợp lệ cho nhà cung cấp AI muốn sử dụng.  
* Hệ thống không kiểm soát toàn bộ cách nhà cung cấp AI xử lý dữ liệu sau khi nhà cung cấp nhận được yêu cầu.  
* Khả năng truy cập nội dung trang phụ thuộc vào quyền trình duyệt, chính sách trang web và định dạng nội dung.  
* Bản demo có thể mô phỏng một số nhánh chưa triển khai, nhưng phải nhận diện rõ trạng thái thực tế của từng chức năng.

---

# **2\. Referenced Documents**

| Mã | Tài liệu | Vai trò |
| ----- | ----- | ----- |
| REF-01 | ISO/IEC/IEEE 29148:2018 – Requirements Engineering | Cơ sở tham chiếu cho cấu trúc và kỹ thuật đặc tả yêu cầu. |
| REF-02 | Stakeholder Requirements Specification (StRS) – Intelligent Reading Assistant | Nguồn yêu cầu và mục tiêu của các bên liên quan. |
| REF-03 | Operational Concept (OpsCon) – Intelligent Reading Assistant | Mô tả bối cảnh sử dụng, vận hành, trách nhiệm và các tình huống vận hành. |
| REF-04 | Architecture Design Document (ADD) – Intelligent Reading Assistant | Tài liệu thiết kế kiến trúc, sẽ được phát triển dựa trên các yêu cầu đã xác định. |
| REF-05 | Software Requirements Specification (SRS) – Intelligent Reading Assistant | Tài liệu phân rã yêu cầu phần mềm từ yêu cầu hệ thống, sẽ được phát triển tiếp. |

REF-02 và REF-03 là nguồn truy vết nội bộ của dự án. Phiên bản chính thức, ngày phát hành và mã định danh yêu cầu nguồn cần được bổ sung khi các tài liệu được quản lý phiên bản.

Việc tham chiếu ISO/IEC/IEEE 29148:2018 không có nghĩa sản phẩm đã được chứng nhận hoặc tuyên bố tuân thủ toàn bộ tiêu chuẩn.

---

# **3\. System Capabilities and Conditions**

## **3.1. System Operational Modes and States**

Các trạng thái dưới đây mô tả điều kiện vận hành quan trọng của hệ thống. Chúng không nhất thiết là các trạng thái loại trừ lẫn nhau: ví dụ, một phiên đang đăng nhập có thể đồng thời gặp lỗi kết nối hoặc đang đồng bộ dữ liệu.

| Mã | Trạng thái | Mô tả | Hành vi bắt buộc |
| ----- | ----- | ----- | ----- |
| SYS-STATE-01 | Chưa đăng nhập hoặc chưa thiết lập | Người dùng chưa đăng nhập hoặc chưa hoàn tất cấu hình AI cần thiết. | Hướng dẫn đăng nhập/cấu hình; không thực hiện chức năng phụ thuộc vào thông tin xác thực còn thiếu. |
| SYS-STATE-02 | Đã đăng nhập và đã cấu hình | Tài khoản hợp lệ và có cấu hình cần thiết cho chức năng được yêu cầu. | Cho phép sử dụng các chức năng theo quyền, cấu hình và tình trạng dịch vụ. |
| SYS-STATE-03 | Đang hỗ trợ đọc | Extension có ngữ cảnh đọc thuộc tab hiện tại và người dùng đang tương tác với hệ thống. | Duy trì ngữ cảnh riêng cho tab; không trộn ngữ cảnh của tab khác. |
| SYS-STATE-04 | Yêu cầu bị chặn bởi chế độ bảo vệ | Yêu cầu có thể chứa dữ liệu nhạy cảm hoặc không thể xác định độ nhạy cảm. | Không truyền dữ liệu bị chặn ra ngoài; giải thích nguyên nhân và cung cấp lựa chọn xử lý an toàn. |
| SYS-STATE-05 | Ngoại tuyến hoặc mất kết nối | Thiết bị không thể kết nối với Backend hoặc nhà cung cấp cần thiết. | Thông báo tình trạng; giữ dữ liệu đã lưu cục bộ; không tự động gửi các yêu cầu đang chờ khi kết nối phục hồi. |
| SYS-STATE-06 | Dịch vụ bên ngoài không khả dụng | Backend, AI Service hoặc nhà cung cấp AI gặp lỗi. | Thông báo lỗi theo thành phần nếu có thể xác định; duy trì các chức năng không phụ thuộc thành phần bị lỗi. |
| SYS-STATE-07 | Phiên hết hạn hoặc chuyển tài khoản | Phiên xác thực không còn hợp lệ hoặc người dùng chuyển sang tài khoản khác. | Yêu cầu đăng nhập lại; cô lập dữ liệu phiên giữa các tài khoản. |
| SYS-STATE-08 | Đang đồng bộ | Dữ liệu cục bộ và dữ liệu trên máy chủ đang được đồng bộ. | Bảo toàn dữ liệu đã lưu; thông báo lỗi đồng bộ khi cần; thử đồng bộ lại khi kết nối phục hồi. |
| SYS-STATE-09 | Xung đột dữ liệu | Phát hiện dữ liệu cục bộ và máy chủ không thống nhất sau các thao tác ngoại tuyến hoặc đồng bộ. | Phát hiện và thông báo xung đột; bảo toàn dữ liệu hiện có; yêu cầu người dùng lựa chọn thay vì ghi đè im lặng. |
| SYS-STATE-10 | Không trích xuất được nội dung | Extension không thể đọc nội dung trang hoặc PDF cần thiết. | Thông báo giới hạn; không giả định rằng AI đã đọc nội dung đó. |

### **3.1.1. State transition requirements**

**SYS-TRANS-01 — Khôi phục phiên:** Khi người dùng mở lại Extension, hệ thống phải khôi phục hội thoại gần nhất đã lưu nếu người dùng đã đăng nhập và dữ liệu có thể truy cập. Hệ thống không tự động khôi phục trang đọc trước đó hoặc ngữ cảnh đọc đang hoạt động.

**SYS-TRANS-02 — Chuyển tab:** Khi người dùng chuyển sang tab khác, hệ thống phải giữ nguyên ngữ cảnh và hội thoại của tab cũ trong phạm vi phiên tương ứng, đồng thời tạo hoặc duy trì ngữ cảnh riêng cho tab mới.

**SYS-TRANS-03 — Hết hạn phiên:** Khi phiên hết hạn, hệ thống phải dừng các chức năng phụ thuộc tài khoản và yêu cầu đăng nhập lại. Nội dung người dùng đang nhập và yêu cầu chưa gửi phải được giữ cục bộ trong phạm vi cho phép để người dùng tiếp tục sau khi đăng nhập.

**SYS-TRANS-04 — Chuyển tài khoản:** Khi chuyển từ tài khoản A sang tài khoản B, dữ liệu phiên đang hoạt động của A phải được loại khỏi vùng làm việc hiện hành; dữ liệu chưa gửi của A không được để B truy cập. Dữ liệu đã lưu trên máy chủ của A vẫn được giữ theo chính sách lưu trữ.

**SYS-TRANS-05 — Phục hồi kết nối:** Khi kết nối được phục hồi, hệ thống có thể tự thử đồng bộ lại dữ liệu đã lưu. Hệ thống không được tự động gửi yêu cầu AI đang chờ nếu việc gửi đó có thể vi phạm chế độ bảo vệ hoặc chưa có thao tác xác nhận phù hợp của người dùng.

## **3.2. System Functional Requirements**

Các yêu cầu chức năng được chia theo nhóm trách nhiệm hệ thống. Mỗi yêu cầu mô tả một hành vi cần đạt ở cấp hệ thống; phương án triển khai cụ thể sẽ được phân rã trong ADD/SRS.

### **3.2.1. Tài khoản và phiên làm việc**

**SYS-FUN-001 — Đăng ký và đăng nhập**

Hệ thống phải cho phép người dùng đăng ký, đăng nhập và truy cập các chức năng phụ thuộc tài khoản theo trạng thái xác thực hiện hành.

**SYS-FUN-002 — Thiết lập ban đầu**

Sau lần đăng nhập đầu tiên, hệ thống phải hướng dẫn người dùng thiết lập API Key và các cấu hình bắt buộc để sử dụng chức năng AI. Các tùy chọn cá nhân hóa không bắt buộc có thể được bỏ qua.

**SYS-FUN-003 — Quản lý phiên**

Hệ thống phải nhận biết phiên hết hạn hoặc không còn hợp lệ, dừng các chức năng phụ thuộc xác thực và hướng dẫn người dùng đăng nhập lại.

**SYS-FUN-004 — Cô lập tài khoản**

Hệ thống phải bảo đảm người dùng chỉ truy cập được dữ liệu thuộc tài khoản hiện hành theo phạm vi quyền của tài khoản đó.

**SYS-FUN-005 — Chuyển tài khoản an toàn**

Khi chuyển tài khoản trên cùng trình duyệt, hệ thống phải ngăn dữ liệu chưa gửi hoặc ngữ cảnh làm việc của tài khoản trước bị truy cập từ tài khoản sau.

### **3.2.2. Quản lý API Key và nhà cung cấp AI**

**SYS-FUN-006 — Cấu hình nhà cung cấp**

Hệ thống phải cho phép người dùng cấu hình API Key cho các nhà cung cấp AI được hỗ trợ.

**SYS-FUN-007 — Kiểm tra API Key**

Hệ thống phải kiểm tra API Key mới trước khi xác nhận cấu hình thành công. Nếu kiểm tra thất bại, hệ thống phải thông báo lỗi theo thông tin có thể xác định và không đánh dấu cấu hình đó là hợp lệ.

**SYS-FUN-008 — Thay thế API Key**

Hệ thống phải cho phép người dùng thay thế API Key của nhà cung cấp. Mỗi nhà cung cấp có tối đa một Key đang hoạt động trên mỗi tài khoản tại một thời điểm, theo phạm vi MVP.

**SYS-FUN-009 — Xử lý lỗi cấu hình**

Khi API Key không hợp lệ, hết hạn hoặc không còn hạn mức, hệ thống phải đánh dấu cấu hình của nhà cung cấp tương ứng là có lỗi nếu xác định được, thông báo nguyên nhân và ngừng sử dụng cấu hình đó cho tới khi được sửa.

**SYS-FUN-010 — Cô lập cấu hình nhà cung cấp**

Lỗi cấu hình của một nhà cung cấp không được tự động làm vô hiệu các nhà cung cấp khác đã được cấu hình hợp lệ.

**SYS-FUN-011 — Chọn nhà cung cấp**

Hệ thống phải cho phép người dùng chọn nhà cung cấp AI mặc định và chuyển sang nhà cung cấp khác cho từng yêu cầu khi nhà cung cấp đó đã được cấu hình hợp lệ.

**SYS-FUN-012 — Bảo vệ API Key**

Hệ thống phải bảo vệ API Key khỏi truy cập trái phép, không đưa Key vào log thông thường và chỉ sử dụng Key cho mục đích tích hợp với nhà cung cấp tương ứng. Chi tiết lưu trữ, mã hóa, truyền tải và quản lý bí mật phải được xác định trong ADD.

**SYS-FUN-013 — Xử lý yêu cầu thất bại**

Yêu cầu AI thất bại do lỗi API Key, hạn mức hoặc lỗi nhà cung cấp không được tự động gửi lại chỉ vì người dùng đã sửa cấu hình. Người dùng phải chủ động thử lại.

### **3.2.3. Trích xuất và quản lý ngữ cảnh đọc**

**SYS-FUN-014 — Trích xuất nội dung**

Extension phải cố gắng trích xuất văn bản từ trang web hoặc PDF trong phạm vi quyền truy cập và khả năng kỹ thuật của trình duyệt.

**SYS-FUN-015 — Thông báo lỗi trích xuất**

Nếu không thể trích xuất nội dung cần thiết, hệ thống phải thông báo rõ giới hạn và không được biểu thị rằng AI đã đọc thành công nội dung không có sẵn.

**SYS-FUN-016 — Hỗ trợ nội dung do người dùng cung cấp**

Khi trích xuất tự động thất bại, hệ thống phải cho phép người dùng cung cấp thủ công đoạn văn bản hoặc ngữ cảnh để yêu cầu hỗ trợ, trong phạm vi chức năng được triển khai.

**SYS-FUN-017 — Ngữ cảnh theo tab**

Hệ thống phải duy trì ngữ cảnh đọc riêng cho từng tab. Nội dung hoặc hội thoại của tab này không được tự động trộn với ngữ cảnh của tab khác.

**SYS-FUN-018 — Không khôi phục ngữ cảnh trang tự động**

Khi người dùng mở lại một hội thoại cũ, hệ thống phải tải hội thoại đã lưu nhưng không tự động truy xuất lại nội dung trang cũ. Người dùng có thể chủ động cung cấp hoặc khôi phục ngữ cảnh cần thiết.

**SYS-FUN-019 — Phạm vi nội dung trả lời**

Hệ thống phải cho phép người dùng yêu cầu AI chỉ dựa trên nội dung trang hiện tại khi chức năng tương ứng được hỗ trợ.

### **3.2.4. Hỗ trợ đọc bằng AI**

**SYS-FUN-020 — Giải thích nội dung**

Hệ thống phải hỗ trợ người dùng yêu cầu giải thích từ vựng, thuật ngữ, khái niệm hoặc ngữ cảnh từ đoạn văn được chọn hay nội dung trang có thể truy cập.

**SYS-FUN-021 — Hỏi đáp theo ngữ cảnh**

Hệ thống phải hỗ trợ hội thoại nhiều lượt dựa trên ngữ cảnh liên quan của phiên đọc hiện tại, trong phạm vi dữ liệu có sẵn và được phép sử dụng.

**SYS-FUN-022 — Tóm tắt và trích xuất ý chính**

Hệ thống phải hỗ trợ yêu cầu tóm tắt hoặc trích xuất ý chính từ nội dung người dùng cung cấp hoặc nội dung trang đã trích xuất thành công.

**SYS-FUN-023 — Dịch nội dung**

Hệ thống phải hỗ trợ yêu cầu dịch đoạn văn được chọn hoặc nội dung có thể truy cập theo ngôn ngữ đích mà người dùng lựa chọn hoặc đã cấu hình.

**SYS-FUN-024 — Câu hỏi kiểm tra hiểu bài**

Hệ thống phải hỗ trợ đề xuất câu hỏi kiểm tra mức độ hiểu bài sau một lượng nội dung đọc nhất định theo cấu hình hoặc quy tắc được triển khai.

**SYS-FUN-025 — Quyền kiểm soát câu hỏi**

Người dùng phải có khả năng trả lời, bỏ qua hoặc điều chỉnh câu hỏi kiểm tra hiểu bài khi chức năng này được cung cấp.

**SYS-FUN-026 — Gợi ý chủ động**

Hệ thống phải có khả năng đề xuất hỗ trợ đọc dựa trên ngữ cảnh hoặc dấu hiệu hành vi đọc mà hệ thống có thể truy cập. Gợi ý phải không gây cản trở đáng kể đến việc đọc nội dung gốc.

**SYS-FUN-027 — Quyền kiểm soát gợi ý**

Hệ thống phải cho phép người dùng điều chỉnh hoặc tắt gợi ý chủ động. Gợi ý phải được phân biệt với hành động đã thực hiện; hệ thống không được tự ý thực hiện hành động thay người dùng nếu chưa có sự đồng ý phù hợp.

**SYS-FUN-028 — Cá nhân hóa**

Hệ thống phải hỗ trợ người dùng điều chỉnh mức độ chi tiết, phong cách phản hồi và các tùy chọn cá nhân hóa được triển khai.

**SYS-FUN-029 — Ngôn ngữ phản hồi mặc định**

Hệ thống phải sử dụng tiếng Việt làm ngôn ngữ phản hồi mặc định cho nội dung AI, đồng thời cho phép người dùng yêu cầu ngôn ngữ khác khi chức năng hỗ trợ.

**SYS-FUN-030 — Hỗ trợ khả năng tiếp cận**

Hệ thống phải cung cấp các chức năng định dạng hoặc trình bày lại nội dung phục vụ nhu cầu đọc và khả năng tiếp cận trong phạm vi được triển khai. Những tuyên bố về hiệu quả đối với một nhóm người dùng cụ thể phải được kiểm chứng riêng, không được suy ra chỉ từ việc có chức năng định dạng lại.

### **3.2.5. Grounding, độ tin cậy và tính minh bạch của câu trả lời**

**SYS-FUN-031 — Ưu tiên nguồn hiện tại**

Khi yêu cầu liên quan đến nội dung đang đọc, hệ thống phải ưu tiên sử dụng nội dung nguồn hiện có và được phép gửi làm căn cứ trả lời.

**SYS-FUN-032 — Phân biệt thông tin nguồn và kiến thức bổ sung**

Khi câu trả lời sử dụng thêm kiến thức nền ngoài nội dung nguồn, hệ thống phải phân biệt phần được hỗ trợ bởi nguồn hiện tại với phần kiến thức bổ sung, trong phạm vi chức năng được triển khai.

**SYS-FUN-033 — Thể hiện sự không chắc chắn**

Khi thông tin hiện có không đủ để trả lời đáng tin cậy, hệ thống phải biểu đạt giới hạn hoặc sự không chắc chắn thay vì trình bày kết luận thiếu căn cứ như một sự thật đã được xác nhận.

**SYS-FUN-034 — Hướng dẫn làm rõ**

Khi câu hỏi chưa đủ rõ hoặc thiếu ngữ cảnh cần thiết, hệ thống phải có khả năng yêu cầu người dùng bổ sung thông tin hoặc gợi ý phần nội dung cần kiểm tra lại.

**SYS-FUN-035 — Không giả định thành công**

Hệ thống không được thông báo hoặc biểu thị rằng AI đã xử lý thành công một yêu cầu khi thao tác chưa được thực hiện, bị chặn hoặc thất bại.

### **3.2.6. Chế độ bảo vệ dữ liệu**

**SYS-FUN-036 — Mặc định bật chế độ bảo vệ**

Chế độ bảo vệ phải được bật mặc định cho tài khoản mới và trạng thái được lưu theo tài khoản, đồng bộ giữa các thiết bị.

**SYS-FUN-037 — Phạm vi bảo vệ**

Chế độ bảo vệ phải áp dụng cho nội dung trang, đoạn văn được chọn, câu hỏi và phần ngữ cảnh hội thoại/lịch sử liên quan nếu những dữ liệu đó có thể được gửi tới dịch vụ AI bên ngoài.

**SYS-FUN-038 — Phát hiện dấu hiệu nhạy cảm**

Hệ thống phải có khả năng phát hiện hoặc đánh giá dấu hiệu nội dung nhạy cảm để hỗ trợ quyết định truyền dữ liệu. Cơ chế này không được coi là hoàn hảo hoặc bảo đảm phát hiện mọi nội dung nhạy cảm.

**SYS-FUN-039 — Đề xuất khi phát hiện dấu hiệu nhạy cảm**

Hệ thống phải có khả năng cảnh báo người dùng khi phát hiện dấu hiệu nhạy cảm, kể cả khi người dùng đang ở trạng thái bảo vệ đã tắt, nếu chức năng phát hiện tương ứng được cung cấp.

**SYS-FUN-040 — Chặn khi không xác định được độ nhạy cảm**

Khi chế độ bảo vệ đang bật và hệ thống không thể xác định độ nhạy cảm của dữ liệu được dự định gửi ra ngoài, hệ thống phải mặc định chặn truyền dữ liệu đó và thông báo cho người dùng.

**SYS-FUN-041 — Chặn yêu cầu có nội dung nhạy cảm**

Khi chế độ bảo vệ đang bật và yêu cầu có dữ liệu thuộc phạm vi bị bảo vệ, hệ thống phải ngăn truyền dữ liệu đó tới dịch vụ bên ngoài cho tới khi điều kiện xử lý được thỏa mãn theo chính sách bảo vệ.

**SYS-FUN-042 — Giải thích yêu cầu bị chặn**

Hệ thống phải thông báo rằng yêu cầu bị chặn và giải thích lý do trong phạm vi có thể xác định.

**SYS-FUN-043 — Sửa yêu cầu bị chặn**

Hệ thống phải cho phép người dùng chỉnh sửa hoặc loại bỏ phần nội dung nhạy cảm rồi chủ động gửi lại yêu cầu.

**SYS-FUN-044 — Không tự gửi lại yêu cầu bị chặn**

Hệ thống không được tự động gửi lại yêu cầu đã bị chế độ bảo vệ chặn, kể cả khi kết nối phục hồi hoặc trạng thái dịch vụ thay đổi.

**SYS-FUN-045 — Tắt chế độ bảo vệ**

Khi người dùng chủ động tắt chế độ bảo vệ, hệ thống phải cảnh báo rằng dữ liệu có thể được gửi tới nhà cung cấp AI bên ngoài và yêu cầu xác nhận. Việc tắt chế độ bảo vệ không tự động xóa lịch sử hoặc gửi lại các yêu cầu trước đó.

**SYS-FUN-046 — Không ảnh hưởng chức năng không liên quan**

Khi một yêu cầu AI bị chặn, hệ thống vẫn phải cho phép người dùng tiếp tục đọc trang, quản lý lịch sử đã lưu, thay đổi cấu hình và sử dụng các chức năng không cần truyền dữ liệu bị chặn.

**SYS-FUN-047 — Giới hạn trách nhiệm với nhà cung cấp**

Hệ thống phải bảo vệ dữ liệu trong các luồng mà sản phẩm kiểm soát theo chính sách đã xác định. Hệ thống không được tuyên bố kiểm soát hoàn toàn việc lưu trữ hoặc sử dụng dữ liệu bên trong hệ thống của nhà cung cấp AI sau khi dữ liệu đã được truyền tới nhà cung cấp.

**SYS-FUN-048 — Không tự gửi yêu cầu đang chờ**

Yêu cầu AI chưa được xử lý phải được giữ cục bộ trên thiết bị cho tới khi người dùng chủ động thử lại hoặc xóa. Yêu cầu đang chờ không được tự động đồng bộ lên máy chủ.

### **3.2.7. Lịch sử, cấu hình và đồng bộ**

**SYS-FUN-049 — Lưu lịch sử**

Hệ thống phải lưu lịch sử hội thoại và thông tin ngữ cảnh liên quan theo chính sách lưu trữ của sản phẩm, phục vụ việc tiếp tục hội thoại và cá nhân hóa.

**SYS-FUN-050 — Đồng bộ theo tài khoản**

Hệ thống phải hỗ trợ đồng bộ lịch sử hội thoại và các cấu hình được xác định là đồng bộ theo tài khoản để người dùng có thể truy cập trên thiết bị khác sau khi đăng nhập.

**SYS-FUN-051 — Phân biệt lịch sử và ngữ cảnh hoạt động**

Hệ thống phải phân biệt lịch sử hội thoại đã lưu với ngữ cảnh đọc đang hoạt động theo tab. Việc đồng bộ lịch sử không đồng nghĩa tự động đồng bộ hoặc khôi phục trạng thái đọc của tab.

**SYS-FUN-052 — Mở lại hội thoại**

Hệ thống phải cho phép người dùng mở lại hội thoại đã lưu mà không tự động tải lại nội dung trang cũ.

**SYS-FUN-053 — Xóa lịch sử**

Hệ thống phải cho phép người dùng xóa một hội thoại hoặc toàn bộ lịch sử theo phạm vi được hỗ trợ. Hệ thống phải thông báo phạm vi xóa và việc bản sao lưu có thể chưa bị xóa ngay lập tức.

**SYS-FUN-054 — Phân biệt xóa lịch sử và xóa tài khoản**

Hệ thống phải phân biệt thao tác xóa lịch sử với thao tác xóa tài khoản; không được coi hai thao tác là tương đương.

**SYS-FUN-055 — Xử lý lỗi đồng bộ**

Khi đồng bộ thất bại, hệ thống phải thông báo lỗi và giữ nguyên dữ liệu đã lưu trên thiết bị hiện tại. Hệ thống phải thử đồng bộ lại khi kết nối được phục hồi.

**SYS-FUN-056 — Phát hiện xung đột đồng bộ**

Khi phát hiện dữ liệu cục bộ và máy chủ xung đột, hệ thống phải thông báo cho người dùng và bảo toàn dữ liệu hiện có cho tới khi có lựa chọn xử lý. Hệ thống không được tự động ghi đè im lặng.

**SYS-FUN-057 — Trạng thái ngoại tuyến**

Khi ngoại tuyến, hệ thống phải thông báo tình trạng kết nối. Người dùng vẫn có thể đọc trang gốc, xem lịch sử cục bộ đã lưu nếu có và soạn yêu cầu mới; các chức năng phụ thuộc kết nối phải được thông báo là tạm thời không khả dụng.

### **3.2.8. Xử lý lỗi và hỗ trợ vận hành**

**SYS-FUN-058 — Phân loại lỗi**

Hệ thống phải phân biệt các loại lỗi có thể xác định, tối thiểu gồm lỗi xác thực/cấu hình người dùng, lỗi Backend, lỗi AI Service, lỗi nhà cung cấp AI, lỗi kết nối và lỗi trích xuất nội dung.

**SYS-FUN-059 — Hướng dẫn xử lý lỗi**

Thông báo lỗi phải giải thích trạng thái theo cách người dùng có thể hiểu và cung cấp hướng xử lý phù hợp khi có thể xác định nguyên nhân.

**SYS-FUN-060 — Duy trì trang gốc**

Lỗi AI hoặc lỗi dịch vụ dùng chung không được khiến Extension giả định rằng trang web gốc không thể sử dụng. Khi bản thân trang gốc vẫn hoạt động, người dùng phải có thể tiếp tục đọc trang đó.

**SYS-FUN-061 — Thử lại có chủ đích**

Hệ thống phải cho phép người dùng chủ động thử lại yêu cầu AI thất bại sau khi đã khắc phục nguyên nhân. Hệ thống không được tự động gửi lại nếu việc đó có thể vi phạm chế độ bảo vệ.

**SYS-FUN-062 — Giám sát cơ bản**

Dịch vụ dùng chung phải hỗ trợ giám sát cơ bản về khả dụng dịch vụ, lỗi hệ thống và khả năng kết nối tới các thành phần bắt buộc.

**SYS-FUN-063 — Sao lưu và phục hồi**

Nhóm vận hành phải có cơ chế sao lưu định kỳ và phương án phục hồi dữ liệu/dịch vụ phù hợp. Tần suất sao lưu, thời gian phục hồi và mức mất dữ liệu tối đa chưa được xác định trong bản SyRS này.

**SYS-FUN-064 — Cập nhật Extension**

Hệ thống phải tương thích với cơ chế cập nhật Extension do Chrome cung cấp trong phạm vi nền tảng MVP. Sau khi cập nhật hoàn tất, người dùng sử dụng phiên bản mới theo cơ chế của trình duyệt.

**SYS-FUN-065 — Hỗ trợ tự phục vụ**

Hệ thống phải cho phép người dùng tiếp cận các hướng dẫn hoặc thông tin xử lý sự cố thông qua kênh hỗ trợ của sản phẩm. Website hướng dẫn tĩnh nằm ngoài phạm vi hệ thống chính.

**SYS-FUN-066 — Phản hồi người dùng**

Bản demo phải hỗ trợ thu thập phản hồi của người dùng theo phương án được triển khai để phục vụ đánh giá hệ thống. Đây là chức năng phục vụ đánh giá, không phải cam kết về quy trình hỗ trợ hoặc thời hạn xử lý sự cố.

## **3.3. System External Interface Requirements**

### **3.3.1. Giao diện với Chrome**

**SYS-IF-001:** Extension phải hoạt động trong môi trường Chrome thuộc phạm vi MVP và sử dụng các giao diện trình duyệt được phép để tương tác với tab, nội dung trang và dữ liệu cục bộ.

**SYS-IF-002:** Extension chỉ được truy cập nội dung hoặc thực hiện thao tác cần quyền khi các quyền tương ứng đã được cấp và chính sách trình duyệt cho phép.

**SYS-IF-003:** Khi quyền truy cập không đủ để trích xuất nội dung, hệ thống phải thông báo giới hạn và không được giả định nội dung đã được đọc.

**SYS-IF-004:** Cơ chế cập nhật Extension phải phù hợp với phương thức phân phối và cập nhật của Chrome trong phạm vi triển khai MVP.

### **3.3.2. Giao diện với trang web và PDF**

**SYS-IF-005:** Hệ thống phải tiếp nhận được văn bản do Extension trích xuất từ các trang web hoặc PDF tương thích.

**SYS-IF-006:** Hệ thống phải thông báo khi nội dung không thể trích xuất hoặc chỉ trích xuất được một phần đáng kể làm ảnh hưởng đến yêu cầu của người dùng.

**SYS-IF-007:** Hệ thống không được coi việc trang web có thể hiển thị trên trình duyệt là bằng chứng rằng Extension có thể đọc toàn bộ nội dung của trang đó.

### **3.3.3. Giao diện với Backend**

**SYS-IF-008:** Extension phải trao đổi với Backend để thực hiện các chức năng tài khoản, truy cập lịch sử, lưu cấu hình và đồng bộ dữ liệu theo phạm vi quyền của người dùng.

**SYS-IF-009:** Backend phải bảo đảm dữ liệu được trả về phù hợp với tài khoản đã xác thực và không để người dùng truy cập dữ liệu của tài khoản khác.

**SYS-IF-010:** Khi Backend không khả dụng, Extension phải thông báo lỗi cho các chức năng bị ảnh hưởng và duy trì các chức năng không phụ thuộc Backend khi có thể.

**SYS-IF-011:** Hệ thống phải phát hiện hoặc xử lý tình trạng đồng bộ thất bại, bảo toàn dữ liệu cục bộ và tránh ghi đè im lặng khi có xung đột.

### **3.3.4. Giao diện với AI Service và nhà cung cấp AI**

**SYS-IF-012:** Hệ thống phải chuyển yêu cầu AI cùng ngữ cảnh cần thiết tới AI Service hoặc thành phần chịu trách nhiệm xử lý AI theo kiến trúc được phê duyệt.

**SYS-IF-013:** Trước khi truyền dữ liệu ra ngoài phạm vi hệ thống do nhóm kiểm soát, luồng xử lý phải áp dụng chính sách bảo vệ dữ liệu đã xác định.

**SYS-IF-014:** Hệ thống phải hỗ trợ gọi API của nhà cung cấp AI đã được cấu hình hợp lệ bằng thông tin xác thực tương ứng.

**SYS-IF-015:** Hệ thống phải xử lý các kết quả hoặc lỗi mà nhà cung cấp trả về, bao gồm lỗi xác thực, hạn mức, kết nối và lỗi xử lý nếu thông tin đó có thể nhận diện.

**SYS-IF-016:** Hệ thống phải hiển thị thông tin mức sử dụng hoặc tiêu thụ nếu nhà cung cấp hỗ trợ và hệ thống có thể tiếp nhận thông tin đó.

**SYS-IF-017:** Nếu nhà cung cấp không cung cấp một loại thông tin sử dụng, hệ thống không được giả định rằng có thể báo cáo chính xác thông tin đó.

### **3.3.5. Giao diện vận hành**

**SYS-IF-018:** Dịch vụ dùng chung phải cung cấp thông tin vận hành đủ để giám sát cơ bản tình trạng khả dụng, lỗi hệ thống và kết nối tới các thành phần bắt buộc.

**SYS-IF-019:** Thông tin vận hành và log phải tuân thủ các yêu cầu bảo vệ dữ liệu, không chứa API Key hoặc nội dung nhạy cảm bị chính sách hệ thống cấm ghi nhận.

---

# **4\. System Quality Characteristics**

## **4.1. Performance & Scalability**

Bản MVP chưa có số liệu đo lường đủ để xác định các ngưỡng hiệu năng định lượng. Vì vậy, các yêu cầu dưới đây xác định hành vi cần có; mục tiêu định lượng phải được thiết lập sau khi đo thử trên môi trường và kịch bản đại diện.

**SYS-QLT-PER-001 — Khả năng phản hồi**

Hệ thống phải cung cấp phản hồi trạng thái cho các thao tác cần xử lý hoặc chờ kết quả để người dùng nhận biết yêu cầu đang được tiếp nhận, xử lý, hoàn tất hay thất bại.

**SYS-QLT-PER-002 — Trạng thái chờ**

Trong khi chờ kết quả AI hoặc đồng bộ, giao diện phải thể hiện trạng thái phù hợp và không được biểu thị thao tác đã hoàn tất khi chưa có kết quả.

**SYS-QLT-PER-003 — Phân biệt thời gian xử lý**

Việc đánh giá hiệu năng phải phân biệt thời gian xử lý thuộc Extension/Backend/AI Service với thời gian chờ phụ thuộc nhà cung cấp AI hoặc mạng bên ngoài, trong phạm vi có thể đo lường.

**SYS-QLT-PER-004 — Hỗ trợ nội dung lớn**

Hệ thống phải xử lý hoặc thông báo giới hạn khi nội dung vượt quá khả năng xử lý của luồng hiện hành. Hệ thống không được âm thầm loại bỏ nội dung làm thay đổi đáng kể ngữ cảnh mà không có chỉ báo phù hợp.

**SYS-QLT-PER-005 — Khả năng mở rộng**

Kiến trúc phải cho phép đánh giá và điều chỉnh năng lực xử lý của Backend và AI Service khi số lượng người dùng hoặc khối lượng yêu cầu thay đổi. Quy mô mục tiêu và ngưỡng tải cụ thể chưa được xác định.

**SYS-QLT-PER-006 — Đo lường hiệu năng**

Trong giai đoạn đánh giá MVP, nhóm phát triển phải đo các thao tác đại diện, bao gồm mở giao diện, gửi yêu cầu AI, nhận phản hồi và đồng bộ lịch sử. Kết quả đo được dùng làm căn cứ xác lập mục tiêu định lượng cho phiên bản tiếp theo.

## **4.2. Reliability, Availability & Maintainability**

**SYS-QLT-RAM-001 — Cô lập lỗi**

Lỗi của một thành phần hoặc một nhà cung cấp AI không được làm gián đoạn các chức năng không phụ thuộc vào thành phần đó nếu các chức năng còn lại vẫn có thể hoạt động.

**SYS-QLT-RAM-002 — Bảo toàn dữ liệu cục bộ**

Khi mất kết nối hoặc đồng bộ thất bại, hệ thống phải giữ dữ liệu đã lưu trên thiết bị hiện tại và không được xóa dữ liệu chỉ vì không thể đồng bộ.

**SYS-QLT-RAM-003 — Phục hồi kết nối**

Khi kết nối phục hồi, hệ thống phải có khả năng thử đồng bộ lại dữ liệu đã lưu theo chính sách đồng bộ, đồng thời tuân thủ các quy tắc bảo vệ đối với yêu cầu AI đang chờ.

**SYS-QLT-RAM-004 — Phát hiện xung đột**

Hệ thống phải phát hiện và thông báo xung đột dữ liệu thuộc phạm vi được hỗ trợ; không được ghi đè im lặng dữ liệu đang xung đột.

**SYS-QLT-RAM-005 — Sao lưu và phục hồi**

Nhóm vận hành phải duy trì cơ chế sao lưu và phương án phục hồi cho dữ liệu, dịch vụ thuộc trách nhiệm của nhóm. Các chỉ tiêu RTO và RPO chưa được quy định.

**SYS-QLT-RAM-006 — Khả năng bảo trì**

Các thành phần hệ thống phải được phân chia trách nhiệm đủ rõ để nhóm phát triển có thể xác định, sửa chữa và kiểm thử lỗi của từng thành phần mà không cần giả định mọi lỗi AI đều là lỗi Backend.

**SYS-QLT-RAM-007 — Giám sát vận hành**

Nhóm vận hành phải có khả năng nhận biết các lỗi hệ thống và tình trạng khả dụng của những thành phần bắt buộc, trong phạm vi giám sát cơ bản của MVP.

**SYS-QLT-RAM-008 — Tính minh bạch của bản demo**

Chức năng mô phỏng, chưa hoàn thiện hoặc không kết nối tới dịch vụ thật phải được nhận diện rõ trong đánh giá và tài liệu demo; không được trình bày như một chức năng đã triển khai hoàn chỉnh.

## **4.3. Security & Safety**

**SYS-QLT-SEC-001 — Bảo vệ dữ liệu truyền tải**

Các kênh truyền dữ liệu có thông tin tài khoản, cấu hình hoặc nội dung người dùng phải sử dụng cơ chế bảo vệ phù hợp với rủi ro và nền tảng triển khai. Giao thức và cấu hình cụ thể được xác định trong ADD.

**SYS-QLT-SEC-002 — Bảo vệ thông tin xác thực**

API Key và thông tin xác thực phải được bảo vệ khỏi truy cập trái phép; không được xuất hiện trong log thông thường hoặc bị trả về cho tài khoản khác.

**SYS-QLT-SEC-003 — Cô lập dữ liệu**

Hệ thống phải thực thi kiểm soát truy cập để dữ liệu tài khoản và lịch sử hội thoại chỉ được truy cập trong phạm vi quyền tương ứng.

**SYS-QLT-SEC-004 — Bảo vệ ngữ cảnh nhạy cảm**

Hệ thống phải áp dụng chế độ bảo vệ trước khi truyền dữ liệu thuộc phạm vi nhạy cảm ra ngoài. Khi không thể xác định độ nhạy cảm và chế độ bảo vệ đang bật, mặc định không truyền dữ liệu.

**SYS-QLT-SEC-005 — Không tự động vượt qua chính sách**

Hệ thống không được tự động tắt chế độ bảo vệ, bỏ qua cảnh báo hoặc gửi lại yêu cầu bị chặn để khắc phục lỗi vận hành.

**SYS-QLT-SEC-006 — Cảnh báo khi giảm bảo vệ**

Khi người dùng chủ động tắt chế độ bảo vệ, hệ thống phải trình bày cảnh báo rủi ro và yêu cầu xác nhận trước khi áp dụng thay đổi.

**SYS-QLT-SEC-007 — Duy trì quyền kiểm soát của người dùng**

Người dùng phải có khả năng xem xét, chỉnh sửa hoặc xóa nội dung yêu cầu bị chặn trước khi chủ động thử gửi lại theo chính sách áp dụng.

**SYS-QLT-SEC-008 — Cô lập dữ liệu phiên**

Khi hết hạn phiên hoặc chuyển tài khoản, hệ thống phải ngăn dữ liệu chưa gửi và ngữ cảnh đang hoạt động của tài khoản trước bị truy cập trái phép.

**SYS-QLT-SEC-009 — Giới hạn dữ liệu trong log**

Log vận hành chỉ được ghi nhận thông tin cần thiết cho mục đích giám sát và xử lý lỗi. API Key và nội dung nhạy cảm thuộc phạm vi cấm ghi log không được đưa vào log thông thường.

**SYS-QLT-SEC-010 — Giới hạn tuyên bố bảo vệ**

Hệ thống chỉ được cam kết các biện pháp bảo vệ trong phạm vi sản phẩm kiểm soát. Hệ thống phải phân biệt các biện pháp đó với chính sách lưu trữ, xử lý hoặc sử dụng dữ liệu của nhà cung cấp AI bên ngoài.

**SYS-QLT-SEC-011 — Khả năng truy vết thao tác quan trọng**

Hệ thống phải cung cấp đủ thông tin vận hành để kiểm tra các tình huống lỗi và đánh giá việc thực thi chính sách bảo vệ trong phạm vi phù hợp, mà không làm lộ dữ liệu bị cấm ghi nhận.

**SYS-QLT-SEC-012 — Phạm vi tuân thủ**

Bản SyRS này chưa tuyên bố sản phẩm tuân thủ một quy định pháp luật hoặc chứng nhận bảo mật cụ thể nào. Nghĩa vụ pháp lý áp dụng và các yêu cầu tuân thủ phải được xác định riêng khi phạm vi triển khai và xử lý dữ liệu được chốt.

---

# **5\. System Design Constraints & Standards Compliance**

## **5.1. Constraints**

**SYS-CON-001 — Nền tảng MVP**

MVP phải ưu tiên Chrome Extension chạy trên máy tính để bàn/laptop. Hỗ trợ thiết bị di động thuộc phạm vi phát triển tương lai, chưa phải cam kết của MVP.

**SYS-CON-002 — Phân chia thành phần**

Hệ thống phải được thiết kế theo các trách nhiệm logic đã xác định: Extension, Backend và AI Service. Cấu trúc triển khai vật lý, giao thức và công nghệ cụ thể thuộc ADD.

**SYS-CON-003 — Mô hình triển khai**

Backend và các dịch vụ dùng chung do nhóm phát triển triển khai, giám sát và bảo trì. Người dùng cuối cài Extension, tạo tài khoản/đăng nhập và cung cấp API Key theo nhu cầu sử dụng.

**SYS-CON-004 — Tích hợp nhà cung cấp AI**

Hệ thống phải sử dụng các giao diện API được nhà cung cấp AI hỗ trợ và xử lý trường hợp nhà cung cấp không khả dụng hoặc không cung cấp một số thông tin sử dụng.

**SYS-CON-005 — Giới hạn trích xuất nội dung**

Hệ thống phải phản ánh giới hạn quyền truy cập và trích xuất của trình duyệt. Không được cam kết hỗ trợ toàn bộ trang web, định dạng hoặc tài liệu.

**SYS-CON-006 — Ràng buộc bảo vệ dữ liệu**

Mọi thiết kế luồng truyền dữ liệu ra ngoài phải đáp ứng các yêu cầu bảo vệ đã quy định trong SyRS. Không được coi việc sử dụng nhà cung cấp AI khác là lý do để bỏ qua chế độ bảo vệ.

**SYS-CON-007 — Giới hạn quyết định kiến trúc**

SyRS không cố định công nghệ lưu trữ, cơ chế mã hóa cụ thể, giao thức API, mô hình AI hoặc thuật toán trích xuất. Các quyết định đó phải được ghi nhận và giải trình trong ADD/SRS.

**SYS-CON-008 — Giới hạn triển khai demo**

Nhóm có thể mô phỏng một số chức năng hoặc nhánh lỗi để phục vụ trình diễn. Trạng thái triển khai thực tế và giới hạn của từng chức năng phải được ghi nhận để tránh tuyên bố sai về năng lực hệ thống.

## **5.2. Standards and compliance**

**SYS-STD-001 — Kỹ thuật yêu cầu**

Tài liệu SyRS sử dụng ISO/IEC/IEEE 29148:2018 làm chuẩn tham chiếu về kỹ thuật yêu cầu và tổ chức tài liệu theo cấu trúc đã thống nhất cho dự án.

**SYS-STD-002 — Truy vết yêu cầu**

Mỗi yêu cầu hệ thống phải có mã định danh duy nhất và có thể truy vết tới yêu cầu nguồn hoặc quyết định vận hành liên quan khi thông tin nguồn đã được gắn mã.

**SYS-STD-003 — Kiểm chứng yêu cầu**

Mỗi yêu cầu phải có phương án kiểm chứng phù hợp, chẳng hạn kiểm thử, kiểm tra, phân tích hoặc trình diễn. Những yêu cầu chưa thể kiểm chứng do thiếu ngưỡng hoặc thiếu quyết định phải được ghi nhận là điểm cần làm rõ.

**SYS-STD-004 — Tuân thủ bảo mật**

Các tiêu chuẩn bảo mật hoặc quy định pháp luật áp dụng phải được xác định theo phạm vi triển khai và dữ liệu thực tế. Tài liệu này không tự suy diễn việc tuân thủ khi chưa có đánh giá tương ứng.

## **5.3. Open decisions and unresolved constraints**

Các mục sau cần được chốt trong ADD, kế hoạch kiểm thử hoặc phiên bản SyRS tiếp theo:

1. Công nghệ, phiên bản và cấu hình cụ thể của từng thành phần.  
2. Cơ chế lưu trữ, truyền tải và bảo vệ API Key.  
3. Nhà cung cấp AI được hỗ trợ trong MVP và giới hạn API tương ứng.  
4. Ngưỡng hiệu năng định lượng và quy mô tải mục tiêu.  
5. Tần suất sao lưu, RTO và RPO.  
6. Chính sách lưu trữ, xóa dữ liệu và thời gian tồn tại của bản sao lưu.  
7. Chính sách bảo vệ chi tiết cho trường hợp phát hiện một phần nội dung nhạy cảm trong yêu cầu.  
8. Quy định pháp luật hoặc tiêu chuẩn bảo mật cụ thể cần tuân thủ.  
9. Danh sách chức năng thực sự triển khai, chức năng mô phỏng và chức năng chưa có trong bản demo.

---

# **6\. System Verification and Validation Provisions**

## **6.1. Verification approach**

Việc kiểm chứng SyRS phải đánh giá xem hệ thống có đáp ứng các yêu cầu đã đặc tả hay không. Các phương pháp được sử dụng gồm:

* **Inspection:** kiểm tra tài liệu, cấu hình, thông báo, log và trạng thái hệ thống.  
* **Test:** thực hiện kịch bản với đầu vào và kết quả mong đợi xác định.  
* **Analysis:** phân tích hành vi, luồng dữ liệu, kết quả đo hoặc bằng chứng kỹ thuật.  
* **Demonstration:** trình diễn hành vi hệ thống trong môi trường mục tiêu, phù hợp với các chức năng có thể quan sát trực tiếp.

Đối với chức năng được mô phỏng, kết quả trình diễn chỉ được coi là bằng chứng cho hành vi mô phỏng, không phải bằng chứng rằng chức năng sản phẩm hoàn chỉnh đã được triển khai.

## **6.2. Verification criteria**

| Mã | Phạm vi kiểm chứng | Phương pháp | Tiêu chí chấp nhận |
| ----- | ----- | ----- | ----- |
| SYS-VV-001 | Tài khoản và phiên | Test, Inspection | Đăng ký/đăng nhập theo luồng được triển khai; phiên hết hạn yêu cầu đăng nhập lại; dữ liệu chưa gửi được giữ theo chính sách và không bị chuyển sang tài khoản khác. |
| SYS-VV-002 | API Key và nhà cung cấp | Test | Key hợp lệ được xác nhận theo kết quả kiểm tra; Key lỗi được báo trạng thái; lỗi của một nhà cung cấp không làm vô hiệu cấu hình hợp lệ của nhà cung cấp khác. |
| SYS-VV-003 | Trích xuất nội dung | Test, Demonstration | Nội dung đọc được được cung cấp cho chức năng AI; khi trích xuất thất bại, hệ thống thông báo và không giả định đã đọc nội dung. |
| SYS-VV-004 | Hỗ trợ đọc bằng AI | Test, User Evaluation | Các chức năng AI được triển khai có thể thực hiện kịch bản tương ứng; kết quả được đánh giá về mức độ liên quan, bám sát nguồn và khả năng biểu đạt sự không chắc chắn. |
| SYS-VV-005 | Chế độ bảo vệ | Test, Inspection | Khi chế độ bảo vệ bật và dữ liệu nhạy cảm hoặc không xác định được độ nhạy cảm, dữ liệu bị chặn không được truyền ra ngoài; hệ thống thông báo và không tự động gửi lại. |
| SYS-VV-006 | Lịch sử và đồng bộ | Test | Lịch sử được lưu theo chính sách; lỗi đồng bộ không làm mất dữ liệu đã lưu cục bộ; xung đột được thông báo và không bị ghi đè im lặng. |
| SYS-VV-007 | Ngoại tuyến và lỗi dịch vụ | Test | Người dùng nhận được thông báo phù hợp; trang gốc vẫn có thể được sử dụng khi bản thân trang còn khả dụng; yêu cầu đang chờ không tự động gửi khi kết nối phục hồi. |
| SYS-VV-008 | Cô lập tài khoản | Test, Analysis | Tài khoản không thể truy cập lịch sử hoặc dữ liệu phiên của tài khoản khác; dữ liệu chưa gửi không bị lộ khi chuyển tài khoản. |
| SYS-VV-009 | Hiệu năng | Test, Analysis | Các thao tác đại diện được đo trong môi trường thử nghiệm; kết quả được ghi nhận làm cơ sở thiết lập ngưỡng định lượng. Không đặt tiêu chí đạt/trượt định lượng khi ngưỡng chưa được phê duyệt. |
| SYS-VV-010 | Khả năng bảo trì và vận hành | Inspection, Test | Có thể nhận biết lỗi và tình trạng khả dụng cơ bản; cơ chế sao lưu và phương án phục hồi được ghi nhận. |
| SYS-VV-011 | Đánh giá người dùng | User Evaluation | Một nhóm nhỏ người dùng thử các kịch bản đọc thực tế và cung cấp phản hồi về tính hữu ích, khả năng sử dụng, thông báo lỗi và mức độ tin cậy cảm nhận. |
| SYS-VV-012 | Tính minh bạch của demo | Inspection, Demonstration | Chức năng thật, chức năng mô phỏng và chức năng chưa triển khai được phân biệt rõ; không tuyên bố hoàn thành các hành vi chưa được chứng minh. |

## **6.3. Validation scenarios**

Các kịch bản xác nhận dưới đây kiểm tra xem hệ thống có đáp ứng nhu cầu sử dụng thực tế của người dùng hay không.

### **VAL-01 — Đọc và hỏi về nội dung trang**

Người dùng mở trang có nội dung đọc được, chọn một đoạn văn và yêu cầu giải thích hoặc dịch.

**Kết quả mong đợi:** Hệ thống sử dụng đoạn văn được chọn làm ngữ cảnh, trả kết quả phù hợp và không làm gián đoạn việc đọc trang gốc.

### **VAL-02 — Hội thoại theo ngữ cảnh**

Người dùng hỏi tiếp một câu có liên quan đến nội dung đã trao đổi trong cùng tab.

**Kết quả mong đợi:** Hệ thống duy trì ngữ cảnh hội thoại phù hợp; không trộn nội dung từ tab khác.

### **VAL-03 — Nội dung không thể trích xuất**

Người dùng mở một PDF hoặc trang mà Extension không thể đọc nội dung cần thiết.

**Kết quả mong đợi:** Hệ thống thông báo giới hạn, không giả định AI đã đọc tài liệu và cho phép người dùng cung cấp nội dung thủ công nếu chức năng đó có sẵn.

### **VAL-04 — Phát hiện nội dung nhạy cảm**

Người dùng yêu cầu AI xử lý nội dung có dấu hiệu nhạy cảm trong khi chế độ bảo vệ đang bật.

**Kết quả mong đợi:** Hệ thống chặn việc truyền dữ liệu thuộc phạm vi bảo vệ, giải thích lý do và cho phép người dùng chỉnh sửa nội dung hoặc lựa chọn xử lý theo chính sách đã quy định. Không có thao tác gửi tự động.

### **VAL-05 — Lỗi API Key**

Người dùng cấu hình Key không hợp lệ hoặc nhà cung cấp trả về lỗi xác thực/hạn mức.

**Kết quả mong đợi:** Hệ thống thông báo lỗi có thể xác định, không sử dụng cấu hình lỗi và cho phép người dùng chủ động thử lại sau khi sửa.

### **VAL-06 — Ngoại tuyến và phục hồi**

Người dùng mất kết nối trong khi sử dụng chức năng cần Backend hoặc AI Service.

**Kết quả mong đợi:** Hệ thống thông báo tình trạng, bảo toàn dữ liệu đã lưu, không tự động gửi yêu cầu AI đang chờ khi kết nối phục hồi và thử đồng bộ lại dữ liệu đã lưu theo chính sách.

### **VAL-07 — Chuyển tài khoản**

Người dùng đăng xuất tài khoản A và đăng nhập tài khoản B trên cùng trình duyệt.

**Kết quả mong đợi:** Dữ liệu phiên chưa gửi của A không còn truy cập được từ vùng làm việc của B; lịch sử của A vẫn được lưu theo chính sách và có thể tải lại khi A đăng nhập trở lại.

### **VAL-08 — Đánh giá tính hữu ích**

Người dùng thuộc các nhóm mục tiêu thử các tác vụ đọc, hỏi đáp, tóm tắt hoặc giải thích.

**Kết quả mong đợi:** Nhóm phát triển thu thập được phản hồi về khả năng hiểu nội dung, tính hữu ích, mức độ phù hợp và những giới hạn cần cải thiện. Kết quả không được khái quát thành bảo đảm hiệu quả cho mọi nhóm người dùng khi chưa có đủ bằng chứng.

## **6.4. AI evaluation provisions**

Việc đánh giá chất lượng AI phải phân biệt chất lượng mô hình với chất lượng của toàn bộ hệ thống.

Các chiều đánh giá dự kiến gồm:

* **Relevance:** mức độ liên quan của câu trả lời với câu hỏi.  
* **Groundedness/Faithfulness:** mức độ câu trả lời được hỗ trợ bởi ngữ cảnh nguồn được cung cấp.  
* **Uncertainty handling:** khả năng thể hiện giới hạn khi bằng chứng không đủ.  
* **User-perceived usefulness:** mức độ hữu ích theo phản hồi người dùng.  
* **Privacy-policy adherence:** khả năng tuân thủ chế độ bảo vệ trước khi truyền dữ liệu ra ngoài.

Phương pháp đo, bộ dữ liệu, ngưỡng chấp nhận và công cụ đánh giá cụ thể sẽ được xác định trong kế hoạch đánh giá AI. Không được dùng một chỉ số tổng hợp duy nhất để thay thế việc kiểm tra các hành vi an ninh và quyền riêng tư bắt buộc.

## **6.5. Completion and acceptance provisions**

MVP được xem là đủ điều kiện trình diễn khi:

1. Các luồng chính trong phạm vi triển khai có thể chạy qua những kịch bản đại diện.  
2. Các nhánh lỗi quan trọng có hành vi và thông báo phù hợp.  
3. Các yêu cầu bảo vệ dữ liệu, cô lập tài khoản, xử lý Key lỗi, ngoại tuyến và xung đột đồng bộ đã được kiểm thử hoặc có bằng chứng kiểm chứng tương ứng.  
4. Những giới hạn trích xuất nội dung và giới hạn của nhà cung cấp AI được thể hiện rõ.  
5. Chức năng mô phỏng được phân biệt với chức năng thực sự hoạt động.  
6. Các yêu cầu chưa có ngưỡng định lượng hoặc chưa có quyết định kiến trúc được ghi nhận thành vấn đề cần giải quyết, không được mặc định coi là đã đạt.

Việc chấp nhận từng yêu cầu phải dựa trên bằng chứng kiểm chứng và trạng thái triển khai thực tế. Một yêu cầu được mô phỏng trong demo không được đánh dấu là đã triển khai đầy đủ chỉ dựa trên việc trình diễn thành công.

---

**Kết thúc SyRS Draft 0.1**

