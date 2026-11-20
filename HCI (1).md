# **OPERATIONAL CONCEPT DESCRIPTION (OpsCon)**

## **Intelligent Reading Assistant**

**Trạng thái:** Bản nháp — phục vụ rà soát và thống nhất trước khi baseline  
**Loại hệ thống:** Trợ lý đọc thông minh tích hợp trên trình duyệt  
**Môi trường ưu tiên:** Google Chrome trên máy tính để bàn và máy tính xách tay  
**Mô hình triển khai:** Dịch vụ tập trung do nhóm phát triển vận hành  
**Cơ sở xây dựng:** Business Requirements Specification (BRS) của Intelligent Reading Assistant và kết quả phỏng vấn vận hành.

---

# **1\. Introduction**

## **1.1. Purpose**

Tài liệu Operational Concept Description (OpsCon) mô tả cách hệ thống Intelligent Reading Assistant được triển khai, tiếp cận và sử dụng trong môi trường thực tế dự kiến; các nhóm người tham gia vận hành; các chế độ và trạng thái hoạt động; những kịch bản vận hành đầu cuối; và tác động của hệ thống đối với người dùng, nhóm phát triển, hạ tầng và quy trình làm việc.

Tài liệu này nhằm:

* Thống nhất cách hình dung hệ thống hoạt động sau khi được triển khai, từ góc nhìn người sử dụng và nhóm vận hành.  
* Làm rõ mối quan hệ giữa Chrome Extension, dịch vụ backend, nhà cung cấp AI bên ngoài và website hướng dẫn.  
* Xác định cách hệ thống xử lý những tình huống thông thường và ngoại lệ, bao gồm lỗi AI, mất kết nối, không trích xuất được nội dung, xung đột đồng bộ và nguy cơ truyền dữ liệu nhạy cảm.  
* Cung cấp cơ sở cho việc phát triển các tài liệu yêu cầu và thiết kế hệ thống ở các giai đoạn tiếp theo.  
* Định hướng kiểm thử các luồng vận hành của bản demo, bao gồm cả luồng thành công và những nhánh lỗi quan trọng.

OpsCon tập trung vào **cách hệ thống được vận hành và sử dụng**, không thay thế BRS, tài liệu yêu cầu hệ thống hoặc tài liệu thiết kế kiến trúc.

## **1.2. Scope**

### **1.2.1. In Scope**

Phạm vi vận hành bao gồm:

1. Cài đặt và sử dụng Chrome Extension trên máy tính.  
2. Đăng ký, đăng nhập, đăng xuất và khôi phục phiên làm việc.  
3. Cấu hình, kiểm tra, thay thế và sử dụng API Key của các nhà cung cấp AI được hỗ trợ.  
4. Hỗ trợ đọc nội dung trang web, văn bản hiển thị và các nội dung nhúng như PDF trong phạm vi có thể trích xuất.  
5. Tương tác với AI thông qua lựa chọn đoạn văn bản, hỏi đáp theo trang hiện tại, hội thoại nhiều lượt, giải thích, dịch, tóm tắt và trích xuất ý chính.  
6. Gợi ý hỗ trợ đọc chủ động, câu hỏi kiểm tra mức độ hiểu và các thiết lập cá nhân hóa.  
7. Lưu trữ, xem lại, tiếp tục và xóa lịch sử hội thoại.  
8. Quản lý ngữ cảnh theo từng tab trình duyệt.  
9. Đồng bộ dữ liệu đã lưu theo tài khoản giữa các thiết bị.  
10. Vận hành chế độ bảo vệ dữ liệu, ngăn truyền nội dung ra dịch vụ AI bên ngoài khi chính sách bảo vệ yêu cầu.  
11. Xử lý mất mạng, lỗi backend, lỗi API Key, lỗi nhà cung cấp AI, lỗi trích xuất nội dung, lỗi phiên và xung đột đồng bộ.  
12. Các hoạt động vận hành của nhóm phát triển: triển khai, giám sát cơ bản, sao lưu, khôi phục và xử lý sự cố.  
13. Website tĩnh hỗ trợ hướng dẫn sử dụng, onboarding, khắc phục sự cố và cung cấp thông tin hỗ trợ.

### **1.2.2. Out of Scope**

Các nội dung sau không được xem là cam kết của phạm vi vận hành MVP:

* Hỗ trợ đầy đủ extension trên điện thoại hoặc mọi loại thiết bị.  
* Bảo đảm đọc được tất cả trang web, PDF hoặc định dạng nhúng.  
* Tự động thực hiện hành động trên website thay người dùng mà không có sự đồng ý.  
* Bảo đảm mọi câu trả lời AI đều chính xác hoặc luôn có đủ bằng chứng.  
* Kiểm soát toàn bộ cách nhà cung cấp AI xử lý dữ liệu sau khi nhận dữ liệu.  
* Cam kết có mô hình AI chạy cục bộ hoặc chế độ AI ngoại tuyến hoàn chỉnh.  
* Hệ thống quản trị vận hành chuyên sâu, bảng trạng thái dịch vụ công khai hoặc quy trình hỗ trợ theo SLA.  
* Cam kết RTO/RPO, mức độ sẵn sàng dịch vụ hoặc thời gian xử lý sự cố cụ thể.  
* Cơ chế phát hiện nội dung nhạy cảm chính xác tuyệt đối.  
* Các cơ chế bảo mật nâng cao như tự động khóa vùng dữ liệu sau thời gian không hoạt động, ngoài xác thực tài khoản hiện có.

Các giới hạn này không loại trừ khả năng mở rộng ở giai đoạn sau.

## **1.3. Operational Vision**

Intelligent Reading Assistant được vận hành như một dịch vụ hỗ trợ đọc tích hợp trực tiếp trong trình duyệt. Người dùng cài đặt extension, đăng ký tài khoản, cấu hình API Key và thực hiện các hoạt động đọc, hỏi đáp, xem lại hội thoại trong cùng môi trường Chrome.

Hệ thống hướng đến trải nghiệm đọc liên tục, hạn chế việc chuyển đổi giữa nhiều ứng dụng. Người dùng có thể yêu cầu hỗ trợ theo đoạn văn bản được chọn hoặc theo nội dung của trang đang đọc; đồng thời nhận gợi ý chủ động ở mức độ không gây gián đoạn.

Mô hình vận hành được định hướng bởi các nguyên tắc:

* **User-controlled interaction:** Người dùng quyết định khi nào gửi yêu cầu, thay đổi thiết lập hoặc tiếp tục yêu cầu bị lỗi.  
* **Context isolation:** Mỗi tab có ngữ cảnh đọc riêng; nội dung từ các tab không tự động trộn lẫn.  
* **Data protection by default:** Chế độ bảo vệ dữ liệu mặc định được bật; khi không thể xác định an toàn, hệ thống ưu tiên chặn truyền nội dung ra AI bên ngoài.  
* **Graceful degradation:** Khi AI hoặc backend gặp lỗi, hệ thống thông báo phù hợp và duy trì khả năng đọc trang web nếu bản thân trang vẫn hoạt động.  
* **Transparent limitations:** Khi không đọc được nội dung hoặc không đủ bằng chứng, hệ thống phải thông báo giới hạn thay vì giả vờ đã xử lý thành công.  
* **Recoverable work:** Dữ liệu đã lưu được bảo toàn; nội dung chưa gửi được quản lý riêng và không tự động gửi lại khi có thể phát sinh rủi ro.  
* **Demo-wide flow coverage:** Bản demo cố gắng triển khai và trình diễn tất cả nhóm luồng đã xác định; các nhánh lỗi quan trọng phải có cách xử lý hoặc mô phỏng được công bố rõ ràng.

---

# **2\. Referenced Documents**

| Tài liệu | Vai trò trong OpsCon |
| ----- | ----- |
| Business Requirements Specification (BRS) — Intelligent Reading Assistant | Cơ sở xác định mục tiêu, nhóm người dùng, phạm vi và nhu cầu nghiệp vụ |
| Operational Concept Description (OpsCon) — Intelligent Reading Assistant | Tài liệu hiện tại, mô tả mô hình và kịch bản vận hành |
| System Requirements Specification (SyRS), nếu được xây dựng | Chuyển các nhu cầu hệ thống thành yêu cầu ở cấp hệ thống |
| Software Requirements Specification (SRS), nếu được xây dựng | Chi tiết hóa các yêu cầu phần mềm |
| Architecture Design Description (ADD), nếu được xây dựng | Mô tả kiến trúc và cách các thành phần đáp ứng yêu cầu |
| Tài liệu hướng dẫn sử dụng và khắc phục sự cố | Hỗ trợ người dùng trong quá trình onboarding và tự xử lý các vấn đề phổ biến |
| Chính sách bảo vệ dữ liệu và điều khoản sử dụng, khi được xác lập | Quy định cụ thể về xử lý dữ liệu, trách nhiệm và giới hạn dịch vụ |

**Ghi chú:** Danh sách này là danh mục tài liệu tham chiếu dự kiến. Tên phiên bản, mã tài liệu, trạng thái phê duyệt và quan hệ truy xuất cần được bổ sung khi các tài liệu tương ứng được baseline.

---

# **3\. Operational Policies and Constraints**

## **3.1. Operational Constraints**

### **3.1.1. Ràng buộc về triển khai và môi trường**

1. Hệ thống sử dụng mô hình backend tập trung, được triển khai và vận hành bởi nhóm phát triển.  
2. Người dùng cài đặt Chrome Extension, đăng ký hoặc đăng nhập tài khoản và cấu hình API Key để sử dụng các chức năng AI.  
3. Website hướng dẫn là website tĩnh phục vụ onboarding, hướng dẫn và khắc phục sự cố; không phải nơi thực hiện các nghiệp vụ tài khoản hoặc cấu hình chính.  
4. Các hoạt động nghiệp vụ chính được thực hiện trong extension.  
5. MVP ưu tiên máy tính để bàn và máy tính xách tay chạy Chrome. Hỗ trợ điện thoại chưa thuộc cam kết vận hành.  
6. Extension chỉ hỗ trợ nội dung mà hệ thống có thể truy cập và trích xuất. Không được mặc định mọi trang web hoặc định dạng nhúng đều đọc được.

### **3.1.2. Ràng buộc về tài khoản và dữ liệu**

1. Người dùng phải đăng ký và đăng nhập để sử dụng các chức năng phụ thuộc tài khoản.  
2. Lịch sử hội thoại đã lưu được quản lý theo tài khoản và đồng bộ giữa các thiết bị.  
3. Ngữ cảnh đọc hiện tại gắn với tab; không tự động đồng bộ hoặc khôi phục ngữ cảnh trang đang đọc trên thiết bị khác.  
4. Dữ liệu chưa gửi được lưu tạm trên thiết bị hiện tại, không đồng bộ sang thiết bị khác.  
5. Khi đổi tài khoản, dữ liệu phiên của tài khoản cũ phải được loại khỏi vùng làm việc hiện tại để tránh truy cập nhầm.  
6. Người dùng có thể xóa một cuộc hội thoại hoặc toàn bộ lịch sử theo các chức năng được cung cấp. Việc xóa không mặc nhiên đồng nghĩa với xóa tài khoản.  
7. Thời hạn lưu dữ liệu, thời gian giữ bản sao lưu và thời gian lưu dữ liệu tạm phải được xác định ở tài liệu hoặc chính sách triển khai tương ứng.

### **3.1.3. Ràng buộc về AI và API Key**

1. Mỗi nhà cung cấp được cấu hình một API Key đang hoạt động cho mỗi tài khoản tại một thời điểm.  
2. Người dùng có thể thay thế API Key và cấu hình riêng cho các nhà cung cấp khác nhau.  
3. Hệ thống phải kiểm tra API Key mới trước khi xác nhận cấu hình thành công.  
4. Nếu API Key không hợp lệ, hết hạn hoặc hết hạn mức sử dụng, nhà cung cấp tương ứng được đánh dấu lỗi và không tiếp tục được sử dụng cho đến khi được sửa.  
5. Lỗi ở một nhà cung cấp không mặc nhiên làm mất khả năng sử dụng các nhà cung cấp khác đã cấu hình hợp lệ.  
6. Người dùng có thể chọn nhà cung cấp mặc định và chuyển nhà cung cấp cho từng yêu cầu.  
7. Yêu cầu AI thất bại không được tự động gửi lại sau khi người dùng sửa API Key hoặc khôi phục kết nối; người dùng phải chủ động thử lại.  
8. Nhà cung cấp AI, mô hình, giới hạn sử dụng và chi phí chưa được cố định trong OpsCon này.

### **3.1.4. Ràng buộc về tính liên tục của hoạt động**

1. Lỗi AI hoặc backend không được khiến extension cố tình giả lập rằng yêu cầu đã hoàn thành.  
2. Nếu trang web nguồn vẫn hoạt động, người dùng phải có thể tiếp tục đọc trang khi dịch vụ hỗ trợ đọc gặp lỗi.  
3. Khi không có mạng, extension có thể cho phép xem lịch sử đã lưu cục bộ và soạn yêu cầu mới, nhưng không tự động gửi yêu cầu khi mạng trở lại.  
4. Khi đóng Chrome, hệ thống giữ dữ liệu đã lưu và nội dung chưa gửi theo quy tắc tương ứng. Khi mở lại extension, hệ thống khôi phục cuộc hội thoại gần nhất nhưng không tự động khôi phục trang web đang đọc.  
5. Khi người dùng mở lại hội thoại cũ, hệ thống không tự động thu thập lại nội dung trang ban đầu. Người dùng có thể chủ động cung cấp hoặc khôi phục ngữ cảnh khi cần.

## **3.2. Regulatory and Safety Policies**

### **3.2.1. Chính sách bảo vệ dữ liệu**

1. Chế độ bảo vệ dữ liệu được bật mặc định cho tài khoản mới.  
2. Chế độ này áp dụng với nội dung trang hiện tại, đoạn văn bản được chọn, câu hỏi có thể chứa dữ liệu nhạy cảm và ngữ cảnh hội thoại/lịch sử có liên quan khi gửi đến AI bên ngoài.  
3. Nếu chế độ bảo vệ đang bật và hệ thống phát hiện nội dung nhạy cảm hoặc không thể xác định nội dung có an toàn để truyền hay không, hệ thống phải chặn truyền nội dung đó theo mặc định, thông báo cho người dùng và cho phép họ xem xét.  
4. Hệ thống không được âm thầm gửi lại yêu cầu đã bị chặn. Người dùng có thể chỉnh sửa yêu cầu, loại bỏ dữ liệu nhạy cảm hoặc chủ động thay đổi chế độ bảo vệ theo chính sách được cung cấp.  
5. Khi người dùng tắt chế độ bảo vệ, hệ thống phải cảnh báo nguy cơ dữ liệu được gửi đến nhà cung cấp AI bên ngoài và yêu cầu xác nhận trước khi tiếp tục.  
6. Tắt chế độ bảo vệ không đồng nghĩa với việc tự động gửi toàn bộ nội dung trước đó hoặc tự động gửi lại các yêu cầu đang chờ.  
7. Chế độ bảo vệ kiểm soát việc truyền dữ liệu qua các luồng do sản phẩm quản lý. Nó không phải là cam kết rằng mọi loại rò rỉ đều bị ngăn chặn, cũng không kiểm soát toàn bộ cách nhà cung cấp AI xử lý dữ liệu sau khi nhận.  
8. Việc lưu lịch sử trên backend của sản phẩm phải được phân biệt với việc truyền dữ liệu sang AI bên ngoài. Chế độ bảo vệ không mặc nhiên xóa hoặc vô hiệu hóa lịch sử đã lưu.

### **3.2.2. Chính sách về tính trung thực của AI**

1. Hệ thống ưu tiên nội dung nguồn đang được sử dụng làm ngữ cảnh.  
2. Khi nguồn không đủ bằng chứng, hệ thống phải nêu rõ giới hạn, yêu cầu thêm ngữ cảnh hoặc đề xuất người dùng kiểm tra lại nguồn.  
3. Thông tin được hỗ trợ trực tiếp bởi nguồn và kiến thức nền bổ sung của AI cần được phân biệt khi thích hợp.  
4. Khi không trích xuất được nội dung, hệ thống phải thông báo rõ. Người dùng vẫn có thể tự cung cấp văn bản hoặc ngữ cảnh để hỏi AI, nhưng hệ thống không được tuyên bố đã đọc nội dung không thể truy cập.  
5. Gợi ý của hệ thống không tự động thay thế quyết định của người dùng; các hành động chủ động chỉ được thực hiện khi phù hợp với tương tác đã được người dùng cho phép.

### **3.2.3. Chính sách về tài khoản và quyền truy cập**

1. Dữ liệu của các tài khoản phải được cách ly.  
2. Sau khi đăng xuất, dữ liệu phiên của tài khoản cũ không được để tài khoản mới truy cập.  
3. MVP không yêu cầu cơ chế khóa riêng khi người dùng rời máy; hệ thống dựa vào đăng xuất và cơ chế bảo mật tài khoản hiện có.  
4. Việc xóa hội thoại và việc xóa tài khoản là hai thao tác riêng biệt.  
5. Hệ thống phải thông báo phạm vi xóa và không cam kết rằng mọi bản sao lưu được loại bỏ ngay lập tức.

### **3.2.4. Chính sách vận hành và xử lý sự cố**

1. Nhóm phát triển chịu trách nhiệm triển khai, giám sát cơ bản, bảo trì và khắc phục sự cố của dịch vụ tập trung.  
2. Giám sát MVP tập trung vào tính sẵn sàng dịch vụ, lỗi hệ thống và khả năng kết nối đến các thành phần cần thiết.  
3. Nhóm phát triển thực hiện sao lưu định kỳ và duy trì phương án khôi phục cơ bản.  
4. Không đặt ra cam kết SLA, RTO/RPO hoặc thời gian xử lý sự cố trong tài liệu này.  
5. Các yêu cầu liên quan đến quyền riêng tư, lưu trữ, sử dụng dữ liệu và nhà cung cấp AI phải được đối chiếu với chính sách áp dụng trước khi triển khai thực tế.

---

# **4\. Current Operational Situation**

Hiện tại, nhu cầu đọc hiểu được giải quyết chủ yếu thông qua trình duyệt và các công cụ riêng lẻ do người dùng lựa chọn. Người đọc có thể phải tự tra cứu thuật ngữ, chuyển nội dung sang công cụ AI, yêu cầu giải thích hoặc tóm tắt, rồi quay lại tài liệu ban đầu để tiếp tục đọc.

Cách làm này tạo ra một số vấn đề vận hành:

* Việc chuyển đổi giữa trang đọc và công cụ hỗ trợ làm gián đoạn dòng đọc.  
* Người dùng phải tự cung cấp ngữ cảnh và có thể phải lặp lại thông tin qua nhiều lượt tương tác.  
* Nội dung và hội thoại được quản lý phân tán, gây khó khăn khi xem lại hoặc tiếp tục trên thiết bị khác.  
* Người dùng khó biết công cụ AI đã thực sự đọc được nội dung nguồn hay chưa.  
* Khi sử dụng nội dung nội bộ hoặc nhạy cảm, người dùng cần kiểm soát thông tin được gửi đến dịch vụ bên ngoài.  
* Lỗi kết nối, giới hạn API Key hoặc lỗi nhà cung cấp có thể làm gián đoạn hỗ trợ đọc.

Trong bối cảnh vận hành dự kiến, chưa có một hệ thống tập trung nào của dự án được xác định là đã triển khai và đang phục vụ người dùng thật. Do đó, chương này mô tả hiện trạng nghiệp vụ và cách sử dụng công cụ trước khi đưa hệ thống mới vào vận hành, không phải báo cáo đánh giá một hệ thống sản xuất hiện hữu.

Việc sử dụng Intelligent Reading Assistant được kỳ vọng sẽ đưa các chức năng hỗ trợ đọc vào cùng môi trường trình duyệt, duy trì hội thoại theo ngữ cảnh và cung cấp cơ chế kiểm soát truyền dữ liệu rõ ràng hơn.

---

# **5\. Description of Proposed System Operations**

## **5.1. Operational Environment**

### **5.1.1. Các thành phần trong môi trường vận hành**

| Thành phần | Vai trò vận hành |
| ----- | ----- |
| Chrome Extension | Giao diện nghiệp vụ chính; đọc nội dung có thể truy cập, nhận yêu cầu, hiển thị kết quả, quản lý thiết lập và tương tác với người dùng |
| Dịch vụ backend tập trung | Xử lý các nghiệp vụ phía máy chủ như tài khoản, lưu trữ hội thoại, đồng bộ và các chức năng hỗ trợ khác theo thiết kế |
| Nhà cung cấp AI bên ngoài | Thực hiện các yêu cầu AI thông qua API Key của người dùng, theo cấu hình và chính sách bảo vệ dữ liệu |
| Website hướng dẫn tĩnh | Cung cấp hướng dẫn sử dụng, onboarding, thông tin khắc phục sự cố và hỗ trợ tự phục vụ |
| Thiết bị người dùng | Chạy Chrome Extension; giữ ngữ cảnh theo tab và dữ liệu chưa gửi tạm thời |
| Nhóm phát triển | Triển khai, giám sát, sao lưu, khôi phục và xử lý sự cố của các dịch vụ do dự án quản lý |

Kiến trúc triển khai chi tiết và cách phân chia trách nhiệm giữa extension, backend và dịch vụ AI sẽ được quy định trong tài liệu thiết kế hệ thống.

### **5.1.2. Luồng tương tác tổng quát**

1. Người dùng cài đặt extension và truy cập giao diện trong Chrome.  
2. Người dùng đăng ký hoặc đăng nhập.  
3. Extension hướng dẫn cấu hình API Key và các thiết lập cần thiết.  
4. Người dùng mở trang cần đọc và tương tác với extension.  
5. Extension xác định ngữ cảnh theo tab và kiểm tra các điều kiện liên quan, bao gồm trạng thái phiên, kết nối và chế độ bảo vệ dữ liệu.  
6. Nếu yêu cầu được phép thực hiện, hệ thống xử lý theo luồng phù hợp và hiển thị kết quả.  
7. Hội thoại được lưu và đồng bộ theo chính sách dữ liệu đã xác định.  
8. Khi phát sinh lỗi, hệ thống thông báo nguyên nhân trong phạm vi có thể xác định và hướng dẫn bước xử lý tiếp theo.

### **5.1.3. Yêu cầu về khả năng đọc nội dung**

Hệ thống hướng tới việc hỗ trợ nội dung văn bản hiển thị trên trang web và các nội dung nhúng như PDF trong phạm vi có thể truy cập và trích xuất.

Nếu không thể đọc nội dung:

* Thông báo rằng nội dung chưa được trích xuất thành công.  
* Không tạo câu trả lời theo cách khiến người dùng hiểu nhầm rằng hệ thống đã đọc toàn bộ nguồn.  
* Cho phép người dùng tự cung cấp nội dung hoặc ngữ cảnh đủ để tiếp tục hỏi AI.  
* Không tự động truyền nội dung chưa được kiểm tra theo chính sách bảo vệ dữ liệu.

## **5.2. User Classes & Operational Profiles**

### **5.2.1. Người dùng cuối**

| Nhóm | Mục tiêu sử dụng | Hoạt động vận hành chính |
| ----- | ----- | ----- |
| Học sinh, sinh viên và người học | Hiểu bài đọc, thuật ngữ và khái niệm | Yêu cầu giải thích, hỏi đáp, tóm tắt, trả lời câu hỏi kiểm tra mức độ hiểu |
| Người đọc tài liệu chuyên môn, nhà nghiên cứu và người đi làm | Tiếp cận nội dung chuyên ngành hoặc phức tạp | Hỏi đáp theo nguồn, trích xuất ý chính, làm rõ khái niệm và xem lại hội thoại |
| Người đọc ngoại ngữ | Hỗ trợ hiểu nội dung bằng ngôn ngữ khác | Dịch, giải thích từ vựng và hỏi đáp theo ngữ cảnh |
| Người cần hỗ trợ khả năng đọc hoặc tiếp cận | Giảm trở ngại trong việc tiếp nhận văn bản | Điều chỉnh cách trình bày, mức độ chi tiết và nhận hỗ trợ giải thích |
| Người dùng phổ thông | Tăng hiệu quả đọc hằng ngày | Hỏi đáp, tóm tắt và nhận gợi ý hỗ trợ đọc |

Các nhóm trên mô tả nhu cầu sử dụng, không hàm ý rằng hệ thống đã có khả năng phát hiện hoặc chẩn đoán tình trạng đọc cụ thể của từng người.

### **5.2.2. Nhóm phát triển và vận hành**

Nhóm phát triển chịu trách nhiệm:

* Triển khai và duy trì dịch vụ tập trung.  
* Giám sát các chỉ số vận hành cơ bản.  
* Xử lý lỗi hệ thống và sự cố kết nối thuộc phạm vi quản lý.  
* Thực hiện sao lưu và khôi phục khi cần.  
* Duy trì website hướng dẫn và tài liệu khắc phục sự cố.  
* Xác định rõ chức năng đã triển khai, chức năng mô phỏng và giới hạn của bản demo.

### **5.2.3. Trách nhiệm của người dùng**

Người dùng chịu trách nhiệm:

* Quản lý tài khoản và API Key của mình.  
* Cấu hình nhà cung cấp AI và thiết lập cá nhân.  
* Chủ động quyết định khi nào gửi yêu cầu, thử lại yêu cầu thất bại hoặc thay đổi chế độ bảo vệ dữ liệu.  
* Kiểm tra lại kết quả AI khi cần, đặc biệt đối với nội dung quan trọng hoặc nhạy cảm.  
* Tự xử lý các vấn đề thông thường theo hướng dẫn; báo cáo các lỗi hệ thống khi cần thiết.

## **5.3. Operational Modes and States**

### **5.3.1. Các chế độ hoạt động**

| Chế độ | Mô tả |
| ----- | ----- |
| Onboarding | Người dùng đăng ký/đăng nhập, cấu hình API Key và các thiết lập cần thiết |
| Normal Reading | Đọc nội dung và sử dụng các chức năng hỗ trợ đọc bình thường |
| AI Interaction | Gửi yêu cầu, nhận kết quả và tiếp tục hội thoại nhiều lượt |
| Proactive Assistance | Hệ thống đưa ra gợi ý hỗ trợ đọc ở mức độ không gây gián đoạn |
| Data Protection | Kiểm soát nội dung được phép truyền ra AI bên ngoài; mặc định bật |
| Offline/Degraded | Mất kết nối hoặc dịch vụ không khả dụng; duy trì các chức năng còn có thể sử dụng |
| Session Expired/Logged Out | Các chức năng phụ thuộc tài khoản bị dừng cho đến khi đăng nhập lại |
| Recovery | Khôi phục kết nối, tiếp tục đồng bộ hoặc xử lý dữ liệu xung đột |

### **5.3.2. Trạng thái vận hành quan trọng**

* **Authenticated:** Người dùng đã đăng nhập và có thể sử dụng chức năng phụ thuộc tài khoản.  
* **Unauthenticated:** Chưa đăng nhập hoặc phiên hết hạn.  
* **AI Configured:** Có ít nhất một nhà cung cấp AI với cấu hình hợp lệ.  
* **AI Configuration Error:** Cấu hình của một nhà cung cấp gặp lỗi.  
* **Connected:** Các kết nối cần thiết hiện khả dụng.  
* **Offline/Unavailable:** Mạng, backend hoặc nhà cung cấp cần thiết không khả dụng.  
* **Protected:** Chế độ bảo vệ dữ liệu đang bật.  
* **Request Blocked:** Yêu cầu bị chặn do chính sách bảo vệ hoặc không đủ cơ sở để xác định an toàn.  
* **Request Pending Locally:** Yêu cầu chưa được gửi và đang được giữ tạm trên thiết bị.  
* **Sync Conflict:** Phát hiện khác biệt giữa dữ liệu cục bộ và dữ liệu máy chủ.  
* **Content Extraction Failed:** Không thể trích xuất nội dung nguồn cần thiết.

Các trạng thái trên mô tả hành vi vận hành mong muốn. Tên trạng thái kỹ thuật và máy trạng thái chi tiết sẽ được xác định trong tài liệu thiết kế.

---

# **6\. Operational Scenarios**

## **6.1. Scenario 1 — Onboarding và bắt đầu sử dụng**

**Mục tiêu:** Đưa người dùng từ lần đầu mở extension đến trạng thái có thể sử dụng chức năng AI.

**Tác nhân:** Người dùng cuối.

**Điều kiện trước:** Extension đã được cài đặt trên Chrome.

**Luồng chính:**

1. Người dùng mở extension.  
2. Hệ thống yêu cầu đăng ký hoặc đăng nhập.  
3. Sau khi xác thực thành công, extension hướng dẫn cấu hình API Key.  
4. Hệ thống kiểm tra API Key được cung cấp.  
5. Nếu hợp lệ, cấu hình được xác nhận.  
6. Người dùng có thể điều chỉnh các thiết lập cá nhân hóa không bắt buộc hoặc bỏ qua.  
7. Người dùng bắt đầu đọc và sử dụng các chức năng AI.

**Ngoại lệ:**

* Nếu API Key không hợp lệ, hệ thống thông báo nguyên nhân có thể xác định và hướng dẫn cấu hình lại.  
* Nếu backend không khả dụng, hệ thống thông báo lỗi và không giả vờ rằng quá trình thiết lập đã hoàn tất.  
* Việc website hướng dẫn không khả dụng không được làm mất quyền đọc trang web bình thường.

**Kết quả:** Người dùng hoàn thành các bước bắt buộc hoặc được thông báo rõ về bước chưa hoàn thành.

## **6.2. Scenario 2 — Hỗ trợ đọc và hỏi đáp theo trang hiện tại**

**Mục tiêu:** Cho phép người dùng nhận hỗ trợ AI dựa trên nội dung đang đọc.

**Tác nhân:** Người dùng cuối.

**Luồng chính:**

1. Người dùng mở trang web hoặc nội dung được hỗ trợ.  
2. Người dùng chọn một đoạn văn bản hoặc yêu cầu hỗ trợ theo trang hiện tại.  
3. Extension xác định ngữ cảnh thuộc tab đang hoạt động.  
4. Hệ thống kiểm tra trạng thái đăng nhập, cấu hình AI và chế độ bảo vệ dữ liệu.  
5. Nếu yêu cầu được phép gửi, hệ thống xử lý và hiển thị kết quả.  
6. Người dùng có thể tiếp tục hỏi trong cùng hội thoại.  
7. Hội thoại được lưu theo chính sách lịch sử.

**Ngoại lệ:**

* Nếu không đọc được nội dung nguồn, hệ thống thông báo và cho phép người dùng tự cung cấp nội dung.  
* Nếu không đủ bằng chứng, AI nêu giới hạn và yêu cầu thêm ngữ cảnh.  
* Nếu chế độ bảo vệ chặn nội dung, hệ thống không gửi yêu cầu bị chặn.  
* Nếu AI hoặc backend gặp lỗi, hệ thống thông báo và cho phép người dùng chủ động thử lại.

**Kết quả:** Người dùng nhận hỗ trợ theo ngữ cảnh chính xác hoặc được hướng dẫn rõ ràng khi không thể thực hiện.

## **6.3. Scenario 3 — Gợi ý chủ động và câu hỏi kiểm tra mức độ hiểu**

**Mục tiêu:** Hỗ trợ người dùng hiểu sâu hơn mà không làm gián đoạn việc đọc.

**Luồng chính:**

1. Người dùng đọc nội dung.  
2. Hệ thống có thể đưa ra gợi ý hỗ trợ hoặc đề xuất câu hỏi kiểm tra mức độ hiểu.  
3. Gợi ý được trình bày theo cách không che khuất hoặc gây cản trở nội dung chính.  
4. Người dùng chọn trả lời, bỏ qua hoặc điều chỉnh cách hỗ trợ.  
5. Nếu người dùng tương tác, hệ thống tiếp tục theo ngữ cảnh của tab hiện tại.

**Ngoại lệ và giới hạn:**

* Người dùng có thể điều chỉnh hoặc tắt hoàn toàn gợi ý chủ động.  
* Hệ thống không tự động thực hiện hành động thay người dùng.  
* Nếu nội dung không đủ cơ sở, hệ thống không được khẳng định chắc chắn rằng người dùng đã hiểu hoặc chưa hiểu nội dung.

**Kết quả:** Gợi ý hỗ trợ đọc vẫn nằm dưới quyền kiểm soát của người dùng.

## **6.4. Scenario 4 — Bảo vệ dữ liệu nhạy cảm**

**Mục tiêu:** Hạn chế việc truyền nội dung nhạy cảm ra nhà cung cấp AI bên ngoài.

**Điều kiện trước:** Chế độ bảo vệ dữ liệu đang bật.

**Luồng chính:**

1. Người dùng đọc trang, chọn đoạn văn bản hoặc nhập câu hỏi.  
2. Extension xác định dữ liệu cần sử dụng cho yêu cầu.  
3. Hệ thống đánh giá dữ liệu theo cơ chế bảo vệ được triển khai.  
4. Nếu nội dung được phép truyền, yêu cầu tiếp tục theo quy trình AI.  
5. Nếu nội dung nhạy cảm hoặc không thể xác định an toàn, hệ thống chặn truyền nội dung.  
6. Hệ thống thông báo lý do và cho phép người dùng xem xét yêu cầu.  
7. Người dùng có thể sửa câu hỏi, loại bỏ nội dung nhạy cảm hoặc chủ động thay đổi chế độ bảo vệ theo chính sách.  
8. Yêu cầu chỉ được gửi sau khi có hành động tiếp tục hợp lệ của người dùng.

**Ngoại lệ:**

* Không được âm thầm thử lại yêu cầu đã bị chặn.  
* Nếu không xác định được mức độ nhạy cảm, mặc định không truyền nội dung.  
* Việc tắt chế độ bảo vệ phải có cảnh báo nguy cơ và xác nhận.  
* Hệ thống không được mô tả cơ chế phát hiện là hoàn hảo hoặc bảo đảm ngăn mọi hình thức rò rỉ.

**Kết quả:** Quyết định truyền dữ liệu được thực hiện theo chế độ bảo vệ và hành động rõ ràng của người dùng.

## **6.5. Scenario 5 — Lỗi API Key hoặc nhà cung cấp AI**

**Mục tiêu:** Khôi phục khả năng sử dụng AI mà không làm mất dữ liệu đang có.

**Luồng chính:**

1. Người dùng gửi yêu cầu AI.  
2. Hệ thống nhận lỗi liên quan đến API Key, hạn mức hoặc nhà cung cấp.  
3. Hệ thống phân biệt nguyên nhân nếu có đủ thông tin.  
4. Nhà cung cấp gặp lỗi được đánh dấu tương ứng.  
5. Người dùng được hướng dẫn mở phần cấu hình API Key.  
6. Người dùng thay thế hoặc sửa API Key.  
7. Hệ thống kiểm tra cấu hình mới trước khi xác nhận.  
8. Người dùng chủ động gửi lại yêu cầu đã thất bại nếu muốn.

**Ngoại lệ:**

* Không tiếp tục sử dụng nhà cung cấp đang có cấu hình lỗi.  
* Các nhà cung cấp khác có cấu hình hợp lệ vẫn có thể được sử dụng.  
* Không tự động gửi lại yêu cầu thất bại sau khi sửa cấu hình.

**Kết quả:** Lỗi được thông báo rõ và việc thử lại nằm dưới quyền kiểm soát của người dùng.

## **6.6. Scenario 6 — Mất kết nối Internet hoặc backend không khả dụng**

**Mục tiêu:** Giảm gián đoạn và bảo toàn công việc của người dùng.

**Luồng chính:**

1. Hệ thống phát hiện kết nối không khả dụng hoặc yêu cầu thất bại.  
2. Extension thông báo tình trạng và các chức năng bị ảnh hưởng.  
3. Người dùng vẫn có thể đọc trang nếu trang đã tải được và trình duyệt còn truy cập nội dung.  
4. Người dùng có thể xem lịch sử đã lưu cục bộ và soạn yêu cầu để gửi sau.  
5. Yêu cầu chưa gửi được lưu tạm trên thiết bị hiện tại.  
6. Khi kết nối trở lại, người dùng chủ động thử gửi yêu cầu.  
7. Các yêu cầu đã lưu và đồng bộ thành công được giữ theo chính sách thông thường.

**Ngoại lệ:**

* Không tự động gửi yêu cầu chờ khi mạng trở lại.  
* Nếu backend không khả dụng, các chức năng phụ thuộc backend như tài khoản, lịch sử máy chủ hoặc đồng bộ có thể bị ảnh hưởng.  
* Không giả định mọi chức năng AI đều phụ thuộc cùng một thành phần; hành vi cụ thể phụ thuộc kiến trúc triển khai.

**Kết quả:** Người dùng biết trạng thái hệ thống và có thể chủ động tiếp tục công việc.

## **6.7. Scenario 7 — Không thể trích xuất nội dung trang hoặc PDF**

**Mục tiêu:** Bảo đảm người dùng hiểu đúng giới hạn của hệ thống.

**Luồng chính:**

1. Người dùng yêu cầu hỗ trợ đọc một trang hoặc PDF.  
2. Extension thử truy cập và trích xuất nội dung trong phạm vi hỗ trợ.  
3. Nếu trích xuất thất bại, hệ thống thông báo rõ.  
4. Người dùng có thể tự nhập hoặc cung cấp nội dung cần thiết trong câu hỏi/hội thoại.  
5. Hệ thống xử lý phần thông tin người dùng cung cấp nếu yêu cầu đáp ứng điều kiện vận hành và bảo vệ dữ liệu.

**Ngoại lệ:**

* Không được khẳng định AI đã đọc nội dung nguồn khi thực tế chưa trích xuất được.  
* Không được tự động gửi nội dung nguồn chưa được kiểm tra theo chính sách bảo vệ dữ liệu.

**Kết quả:** Người dùng có phương án tiếp tục nhưng không bị gây hiểu nhầm về nguồn dữ liệu AI đã sử dụng.

## **6.8. Scenario 8 — Đóng Chrome và tiếp tục phiên làm việc**

**Mục tiêu:** Bảo toàn dữ liệu đã lưu và giúp người dùng tiếp tục hội thoại.

**Luồng chính:**

1. Người dùng đóng Chrome hoặc tắt extension.  
2. Dữ liệu đã lưu thành công được giữ nguyên.  
3. Nội dung chưa gửi được giữ tạm trên thiết bị theo quy tắc tương ứng.  
4. Khi mở lại extension, hệ thống khôi phục cuộc hội thoại gần nhất.  
5. Hệ thống không tự động mở lại trang web trước đó hoặc khôi phục ngữ cảnh đọc đã mất.  
6. Người dùng tự mở lại trang và chủ động cung cấp hoặc khôi phục ngữ cảnh khi cần.

**Kết quả:** Cuộc hội thoại được tiếp tục mà không tự ý khôi phục trang hoặc gửi yêu cầu ngoài ý muốn.

## **6.9. Scenario 9 — Nhiều tab và nhiều hội thoại**

**Mục tiêu:** Tránh trộn ngữ cảnh đọc giữa các trang.

**Luồng chính:**

1. Người dùng mở nhiều tab Chrome.  
2. Mỗi tab có ngữ cảnh đọc riêng.  
3. Yêu cầu được tạo trong tab nào thì sử dụng ngữ cảnh của tab đó.  
4. Khi người dùng chuyển tab, hội thoại và ngữ cảnh của tab cũ được giữ nguyên.  
5. Tab mới có phiên hỗ trợ đọc riêng.  
6. Người dùng quay lại tab cũ để tiếp tục hội thoại trước đó.

**Ngoại lệ:**

* Không tự động đưa nội dung của tab khác vào yêu cầu.  
* Việc mở lại hội thoại cũ không tự động thu thập lại trang nguồn; người dùng chủ động khôi phục hoặc cung cấp ngữ cảnh khi cần.

**Kết quả:** Mỗi hội thoại duy trì ranh giới ngữ cảnh rõ ràng theo tab.

## **6.10. Scenario 10 — Đồng bộ dữ liệu và xử lý xung đột**

**Mục tiêu:** Duy trì tính nhất quán của dữ liệu giữa thiết bị và máy chủ.

**Luồng chính:**

1. Người dùng thay đổi dữ liệu khi có kết nối.  
2. Hệ thống lưu dữ liệu và thực hiện đồng bộ theo cơ chế thông thường.  
3. Nếu đồng bộ thất bại, hệ thống thông báo và giữ dữ liệu đã lưu trên thiết bị.  
4. Khi kết nối trở lại, hệ thống tự động thử đồng bộ lại.  
5. Nếu phát hiện xung đột giữa dữ liệu cục bộ và dữ liệu máy chủ, hệ thống thông báo và bảo toàn dữ liệu hiện có.  
6. Người dùng quyết định cách xử lý khi cần.

**Ngoại lệ:**

* Không tự động ghi đè dữ liệu khi đã phát hiện xung đột cần giải quyết.  
* MVP chưa yêu cầu giao diện hợp nhất dữ liệu phức tạp.  
* Dữ liệu chưa gửi không đồng bộ sang thiết bị khác.

**Kết quả:** Giảm nguy cơ mất dữ liệu và tránh xung đột bị xử lý âm thầm.

## **6.11. Scenario 11 — Đăng xuất, hết phiên và đổi tài khoản**

**Mục tiêu:** Ngăn dữ liệu cá nhân bị truy cập nhầm giữa các tài khoản.

**Luồng chính:**

1. Người dùng đăng xuất hoặc phiên hết hạn.  
2. Các chức năng phụ thuộc tài khoản dừng và yêu cầu đăng nhập lại.  
3. Nội dung người dùng đang nhập và trạng thái đọc hiện tại được giữ cục bộ theo quy tắc đã xác định.  
4. Khi người dùng đổi sang tài khoản khác, dữ liệu phiên của tài khoản cũ được loại khỏi vùng làm việc hiện tại.  
5. Tài khoản mới không được truy cập dữ liệu cục bộ riêng của tài khoản cũ.  
6. Khi đăng nhập lại tài khoản cũ, hệ thống tải dữ liệu đã lưu theo cơ chế thông thường.

**Ngoại lệ:**

* Không gửi tự động nội dung đang nhập hoặc yêu cầu chưa gửi sau khi đăng nhập lại.  
* MVP không yêu cầu tự động khóa extension sau một khoảng thời gian không hoạt động.

**Kết quả:** Phiên tài khoản được cách ly, dữ liệu đã lưu của tài khoản không bị xóa chỉ vì đăng xuất.

## **6.12. Scenario 12 — Xóa lịch sử hội thoại**

**Mục tiêu:** Cho phép người dùng kiểm soát lịch sử đã lưu.

**Luồng chính:**

1. Người dùng mở lịch sử.  
2. Người dùng chọn xóa một hội thoại hoặc xóa toàn bộ lịch sử.  
3. Hệ thống thực hiện thao tác theo phạm vi được chọn.  
4. Hệ thống thông báo phạm vi dữ liệu bị xóa và các giới hạn liên quan.  
5. Dữ liệu thuộc bản sao lưu có thể còn tồn tại trong một khoảng thời gian theo chính sách lưu trữ.

**Ngoại lệ:**

* Xóa lịch sử không tự động xóa tài khoản.  
* Thao tác xóa lịch sử không mặc nhiên đồng nghĩa với việc mọi bản sao lưu được loại bỏ tức thời.  
* Quy tắc đối với dữ liệu tạm chưa gửi cần được áp dụng riêng theo vòng đời dữ liệu đó.

**Kết quả:** Người dùng kiểm soát được lịch sử hội thoại theo chức năng được cung cấp.

## **6.13. Scenario 13 — Nhóm phát triển vận hành và xử lý sự cố**

**Mục tiêu:** Duy trì hoạt động cơ bản của dịch vụ tập trung.

**Luồng chính:**

1. Nhóm phát triển triển khai phiên bản hệ thống.  
2. Hệ thống được giám sát ở mức cơ bản về tính sẵn sàng, lỗi và kết nối thành phần.  
3. Khi phát hiện sự cố, nhóm phát triển xác định phạm vi ảnh hưởng.  
4. Nhóm xử lý lỗi hoặc khôi phục dịch vụ.  
5. Các dữ liệu được sao lưu định kỳ được sử dụng trong hoạt động khôi phục khi cần.  
6. Người dùng nhận thông báo lỗi trong extension khi chức năng bị ảnh hưởng.

**Ngoại lệ:**

* MVP không bắt buộc có bảng trạng thái dịch vụ công khai hoặc thông báo bảo trì chủ động.  
* Không có SLA hoặc mục tiêu thời gian khôi phục cụ thể được cam kết trong tài liệu này.  
* Quy trình xử lý sự cố chính thức, phân công trực vận hành và mức độ nghiêm trọng cần được xác định nếu triển khai production.

**Kết quả:** Nhóm phát triển có cơ chế cơ bản để phát hiện và xử lý các lỗi thuộc phạm vi quản lý.

---

# **7\. Summary of Impacts**

## **7.1. Tác động đến người dùng**

### **Tác động tích cực**

* Hỗ trợ đọc được tích hợp trực tiếp trong trình duyệt.  
* Giảm chuyển đổi giữa công cụ đọc và công cụ AI.  
* Có thể tiếp tục hội thoại nhiều lượt theo ngữ cảnh của từng tab.  
* Có lịch sử hội thoại theo tài khoản và khả năng tiếp tục trên thiết bị khác.  
* Có quyền điều chỉnh cách hỗ trợ, tắt gợi ý chủ động và kiểm soát chế độ bảo vệ dữ liệu.  
* Có hướng dẫn rõ ràng khi hệ thống không đọc được nội dung hoặc gặp lỗi.

### **Thay đổi về trách nhiệm**

* Người dùng cần quản lý tài khoản và API Key.  
* Người dùng cần chủ động quyết định việc gửi lại yêu cầu thất bại hoặc thay đổi chế độ bảo vệ.  
* Người dùng cần kiểm tra kết quả AI khi nội dung có mức độ quan trọng cao.  
* Người dùng phải tự mở lại trang nguồn khi khởi động lại phiên trình duyệt.

## **7.2. Tác động đến nhóm phát triển**

Nhóm phát triển cần đảm nhiệm các hoạt động vận hành mới:

* Triển khai và duy trì backend tập trung.  
* Xử lý lỗi tài khoản, lưu trữ, đồng bộ và kết nối thành phần.  
* Giám sát cơ bản và thực hiện sao lưu, khôi phục.  
* Duy trì website hướng dẫn và tài liệu khắc phục sự cố.  
* Kiểm thử các luồng bảo vệ dữ liệu, đổi tài khoản, đồng bộ xung đột và lỗi AI.  
* Phân biệt rõ tính năng triển khai thật với chức năng mô phỏng trong demo.

MVP không yêu cầu một bộ phận hỗ trợ người dùng chuyên trách hoặc SLA phản hồi chính thức.

## **7.3. Tác động đến hạ tầng và dịch vụ**

Hệ thống dự kiến cần:

* Môi trường triển khai dịch vụ backend.  
* Cơ chế lưu trữ tài khoản và lịch sử hội thoại.  
* Khả năng phục vụ đồng bộ dữ liệu giữa các thiết bị.  
* Kết nối đến các nhà cung cấp AI được hỗ trợ.  
* Cơ chế giám sát, sao lưu và khôi phục cơ bản.  
* Website tĩnh phục vụ onboarding và hướng dẫn.

Các công nghệ cụ thể, quy mô tài nguyên, giới hạn đồng thời, chi phí vận hành và mục tiêu hiệu năng cần được xác định trong tài liệu thiết kế và kế hoạch triển khai.

## **7.4. Tác động đến quy trình làm việc**

| Quy trình | Thay đổi dự kiến |
| ----- | ----- |
| Đọc và tra cứu | Hỗ trợ đọc được thực hiện ngay trong trình duyệt |
| Hỏi đáp theo nguồn | Người dùng tương tác với AI theo ngữ cảnh của tab hiện tại |
| Quản lý hội thoại | Lịch sử được lưu và đồng bộ theo tài khoản |
| Bảo vệ dữ liệu | Người dùng có chế độ bảo vệ mặc định bật và được thông báo khi yêu cầu bị chặn |
| Xử lý lỗi | Người dùng nhận hướng dẫn và chủ động thử lại khi phù hợp |
| Vận hành dịch vụ | Nhóm phát triển thực hiện giám sát, sao lưu và xử lý sự cố cơ bản |

## **7.5. Các điểm cần xác nhận trước khi baseline**

Các mục sau chưa có đủ thông tin để được xem là quyết định vận hành cuối cùng:

1. Chính sách và thời hạn lưu lịch sử hội thoại, dữ liệu tạm và bản sao lưu.  
2. Cách bảo vệ API Key khi lưu trữ, truyền và sử dụng.  
3. Danh sách nhà cung cấp AI được hỗ trợ và điều kiện sử dụng tương ứng.  
4. Chi tiết chính sách khi người dùng tắt chế độ bảo vệ dữ liệu, đặc biệt đối với nội dung nhạy cảm bị chặn trước đó.  
5. Phạm vi dữ liệu được giữ lại cục bộ khi phiên hết hạn hoặc khi đổi tài khoản.  
6. Cơ chế xử lý xung đột đồng bộ và thao tác xóa khi có thay đổi ngoại tuyến.  
7. Tiêu chí đánh giá chất lượng AI và cách đo lường hiệu quả của gợi ý hỗ trợ đọc.  
8. Các giới hạn vận hành của bản demo: chức năng thực tế, chức năng mô phỏng và những nội dung chưa triển khai.

Các mục này cần được xử lý trong tài liệu yêu cầu, thiết kế, chính sách dữ liệu hoặc kế hoạch kiểm thử tương ứng trước khi được coi là cam kết chính thức.

---

**Kết luận:** OpsCon xác lập mô hình vận hành dự kiến cho Intelligent Reading Assistant theo hướng dịch vụ tập trung, tương tác qua Chrome Extension, hỗ trợ đọc theo ngữ cảnh và kiểm soát truyền dữ liệu. Bản nháp ưu tiên bao phủ các luồng vận hành chính cùng những nhánh lỗi quan trọng, đồng thời không tự đặt ra các cam kết kỹ thuật hoặc mức dịch vụ chưa được thống nhất.

## **1\. Kết quả rà soát: các điểm còn thiếu và xung đột**

Mô hình vận hành tập trung, Chrome Extension, tài khoản người dùng, API Key do người dùng cung cấp, lịch sử hội thoại, ngữ cảnh theo tab và cơ chế bảo vệ dữ liệu nhìn chung nhất quán.

1\. Ranh giới bảo vệ dữ liệu cần diễn đạt chính xác

Chế độ bảo vệ mặc định bật nhằm ngăn sản phẩm gửi nội dung nhạy cảm đến nhà cung cấp AI. Tuy nhiên, hệ thống vẫn lưu lịch sử trên máy chủ của chính sản phẩm và đồng bộ theo tài khoản. Vì vậy, phải phân biệt rõ lưu trữ trong hệ thống với truyền dữ liệu ra nhà cung cấp AI bên ngoài. Không được hứa rằng hệ thống ngăn mọi hình thức rò rỉ hoặc kiểm soát cách nhà cung cấp AI xử lý dữ liệu sau khi nhận.

2\. Quy tắc tắt chế độ bảo vệ cần giới hạn theo chính sách

Người dùng có thể tắt chế độ sau khi được cảnh báo và xác nhận. Nhưng hành động này không đồng nghĩa với việc hệ thống tự gửi toàn bộ nội dung đang chặn. Mỗi yêu cầu vẫn cần được người dùng chủ động thực hiện, và việc gửi dữ liệu phải tuân thủ chính sách bảo vệ đã xác định.

3\. Quy tắc đồng bộ, xóa và dữ liệu tạm cần được giữ tách biệt

Lịch sử đã lưu được đồng bộ theo tài khoản; yêu cầu chưa gửi chỉ lưu tạm trên thiết bị; dữ liệu cục bộ phải được cách ly khi đổi tài khoản. Khi xóa lịch sử, hệ thống giải thích phạm vi xóa và khả năng bản sao lưu còn tồn tại trong một khoảng thời gian. Đây là các loại dữ liệu và vòng đời khác nhau, không nên mô tả như một cơ chế lưu duy nhất.

4\. Một số chi tiết chưa có quyết định cụ thể

Chưa chốt thời hạn lưu dữ liệu, thời hạn giữ bản sao lưu, ngưỡng hết phiên, thời gian giữ yêu cầu chưa gửi, chính sách lưu API Key, các nhà cung cấp AI cụ thể và tiêu chí định lượng cho chất lượng AI. Tôi sẽ ghi nhận đây là các điểm cần xác định ở giai đoạn thiết kế hoặc triển khai, không tự đặt ra con số.

5\. Mức độ vận hành của demo cần được phân biệt với production

Nhóm phát triển chịu trách nhiệm triển khai, giám sát cơ bản, sao lưu định kỳ và xử lý sự cố. Tuy nhiên, bản demo không mặc nhiên có SLA, RTO/RPO, bảng trạng thái dịch vụ hay quy trình hỗ trợ người dùng chính thức. Các chức năng chưa hoàn thiện phải được đánh dấu rõ thay vì giả định đã vận hành thực tế.

Các điểm trên không ngăn cản việc viết OpsCon. Tôi sẽ ghi chúng thành ràng buộc, giả định hoặc điểm cần xác nhận, đồng thời giữ nguyên phạm vi template 7 chương mà bạn đã chọn.