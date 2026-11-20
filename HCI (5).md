# **Software Requirements Specification (SRS)**

## **Intelligent Reading Assistant — Draft 0.1**

**Trạng thái:** Draft — Chờ rà soát và xác nhận  
**Sản phẩm:** Intelligent Reading Assistant  
**Nền tảng mục tiêu:** Chrome Extension trên máy tính  
**Ngôn ngữ giao diện mục tiêu:** Tiếng Việt và tiếng Anh

---

# **1\. Introduction**

## **1.1 Purpose**

Tài liệu Software Requirements Specification (SRS) xác định các yêu cầu phần mềm đối với hệ thống Intelligent Reading Assistant, bao gồm giao diện người dùng, chức năng hỗ trợ đọc, tích hợp dịch vụ AI, quản lý tài khoản và dữ liệu, bảo vệ dữ liệu, đồng bộ, xử lý lỗi và các thuộc tính chất lượng phần mềm.

Tài liệu được sử dụng làm cơ sở để:

* Thiết kế và triển khai phần mềm.  
* Xác định hợp đồng giao tiếp giữa Chrome Extension, Backend và dịch vụ AI.  
* Xây dựng kế hoạch kiểm thử và xác minh yêu cầu.  
* Truy vết yêu cầu từ các tài liệu cấp cao hơn.  
* Đánh giá mức độ hoàn thành của bản demo.  
* Hỗ trợ phát triển Architecture Design Document (ADD).

Đối tượng sử dụng tài liệu gồm nhóm phát triển frontend, backend, AI/ML, người kiểm thử và các bên liên quan tham gia đánh giá sản phẩm.

## **1.2 Scope**

Intelligent Reading Assistant là công cụ hỗ trợ người dùng đọc và hiểu nội dung trên trình duyệt bằng các chức năng AI tương tác theo ngữ cảnh.

Phạm vi phần mềm trong MVP bao gồm:

1. Chrome Extension cung cấp giao diện hỗ trợ đọc.  
2. Đăng ký, đăng nhập và quản lý phiên người dùng.  
3. Thiết lập và quản lý API Key của nhà cung cấp AI.  
4. Trích xuất nội dung từ trang web và thử hỗ trợ PDF hoặc nội dung nhúng trong giới hạn kỹ thuật.  
5. Giải thích, dịch, tóm tắt và hỏi đáp về nội dung đang đọc.  
6. Quản lý hội thoại, ngữ cảnh đọc và lịch sử.  
7. Hỗ trợ chủ động, cá nhân hóa và câu hỏi kiểm tra mức độ hiểu bài.  
8. Bảo vệ dữ liệu trước khi gửi nội dung đến nhà cung cấp AI.  
9. Đồng bộ dữ liệu theo tài khoản.  
10. Xử lý lỗi kết nối, lỗi xác thực nhà cung cấp và lỗi đồng bộ.

Các thành phần và giới hạn bên ngoài:

* **Backend dùng chung:** xử lý các chức năng tài khoản, dữ liệu, đồng bộ và các nghiệp vụ phía máy chủ.  
* **AI Service:** thực hiện điều phối tác vụ AI và giao tiếp với các nhà cung cấp mô hình.  
* **Nhà cung cấp AI bên ngoài:** cung cấp mô hình và API theo cấu hình của người dùng.  
* **Website hướng dẫn tĩnh:** nằm ngoài ranh giới hệ thống chính; cung cấp hướng dẫn cài đặt và tự khắc phục sự cố.

Các nội dung chưa được cam kết thuộc MVP gồm AI chạy hoàn toàn ngoại tuyến, khóa tài khoản tự động trên máy tính dùng chung, hỗ trợ mọi loại PDF/nội dung nhúng, xóa tức thời mọi bản sao lưu và các chỉ tiêu định lượng về hiệu năng hoặc chất lượng AI chưa được phê duyệt.

## **1.3 Product Overview**

### **1.3.1 Product perspective**

Hệ thống là một sản phẩm phần mềm gồm các thành phần logic phối hợp với nhau:

* **Chrome Extension:** giao diện tương tác và tích hợp với trang đang đọc.  
* **Backend:** quản lý tài khoản, dữ liệu hội thoại, cấu hình người dùng và đồng bộ.  
* **AI Service:** xử lý yêu cầu AI, xây dựng ngữ cảnh và tích hợp nhà cung cấp.  
* **External AI Providers:** cung cấp năng lực mô hình thông qua API.  
* **External browser and web content:** cung cấp nội dung người dùng muốn đọc.

ADD sẽ xác định cách phân chia thành phần, triển khai, lưu trữ và giao tiếp cụ thể. SRS chỉ quy định hành vi và các hợp đồng phần mềm cần đáp ứng.

### **1.3.2 User classes**

| Nhóm người dùng | Nhu cầu chính |
| ----- | ----- |
| Người đọc phổ thông | Hiểu nội dung, giải thích thuật ngữ, dịch, tóm tắt và hỏi đáp |
| Người đọc chuyên sâu | Đặt câu hỏi nhiều lượt, duy trì ngữ cảnh và kiểm tra mức độ hiểu |
| Người dùng quan tâm quyền riêng tư | Kiểm soát dữ liệu được gửi đến nhà cung cấp AI |
| Nhóm vận hành/phát triển | Theo dõi lỗi cơ bản, khôi phục dịch vụ và hỗ trợ người dùng |
| Nhà cung cấp AI | Cung cấp API theo hợp đồng và chính sách riêng của nhà cung cấp |

Người đọc là nhóm người dùng chính. Nhóm phát triển/vận hành và nhà cung cấp AI là các bên hỗ trợ hoạt động của hệ thống.

### **1.3.3 Assumptions and dependencies**

* Người dùng có trình duyệt Chrome tương thích với phiên bản extension được phát hành.  
* Các chức năng tài khoản, dữ liệu từ xa và đồng bộ cần kết nối mạng và dịch vụ Backend tương ứng.  
* Các yêu cầu AI phụ thuộc vào API Key hợp lệ, hạn mức sử dụng, khả năng truy cập và hợp đồng API của nhà cung cấp được hỗ trợ.  
* Khả năng trích xuất nội dung phụ thuộc cấu trúc trang web, quyền truy cập và giới hạn của trình duyệt.  
* Chất lượng câu trả lời phụ thuộc nội dung nguồn, mô hình, prompt và khả năng truy cập của nhà cung cấp.  
* Người dùng có trách nhiệm kiểm tra và xác nhận nội dung họ chủ động cho phép gửi trong các trường hợp hệ thống cảnh báo dữ liệu nhạy cảm.

---

# **2\. Referenced Documents**

| Mã tham chiếu | Tài liệu | Mục đích sử dụng |
| ----- | ----- | ----- |
| REF-01 | Business Requirements Specification (BRS) | Mục tiêu và phạm vi kinh doanh |
| REF-02 | Stakeholder Requirements Specification (StRS) | Nhu cầu và kỳ vọng của các bên liên quan |
| REF-03 | Operational Concept (OpsCon) | Kịch bản và điều kiện vận hành |
| REF-04 | System Requirements Specification (SyRS) | Yêu cầu ở cấp hệ thống |
| REF-05 | Requirements Traceability Matrix (RTM) | Truy vết nguồn, phần mềm và kiểm thử |
| REF-06 | Architecture Design Document (ADD) | Tài liệu kiến trúc được phát triển dựa trên các yêu cầu |
| REF-07 | ISO/IEC/IEEE 29148:2018 | Tham chiếu phương pháp kỹ thuật yêu cầu |

**Lưu ý:** Cần cập nhật phiên bản, ngày phát hành và mã tài liệu chính thức của REF-01 đến REF-06 khi các tài liệu được phê duyệt. Danh mục này không thay thế việc xác nhận mã yêu cầu nguồn.

---

# **3\. Specific Requirements**

## **3.1 External Interface Requirements**

### **3.1.1 User Interfaces (UI/UX)**

#### **UI-01 — Giao diện extension**

Hệ thống SHALL cung cấp giao diện Chrome Extension cho phép người dùng truy cập các chức năng hỗ trợ đọc mà không cần rời khỏi trang đang đọc.

#### **UI-02 — Tương tác với nội dung được chọn**

Hệ thống SHALL cung cấp thao tác để người dùng chọn một đoạn văn bản và khởi tạo tác vụ hỗ trợ đọc trên đoạn đó.

#### **UI-03 — Tương tác với trang hiện tại**

Hệ thống SHALL cho phép người dùng đặt câu hỏi liên quan đến nội dung trang hiện tại khi nội dung có thể được trích xuất.

#### **UI-04 — Hiển thị trạng thái yêu cầu**

Giao diện SHALL phân biệt tối thiểu các trạng thái: đang xử lý, hoàn tất, cần người dùng hành động, bị chặn bởi chính sách bảo vệ và thất bại.

#### **UI-05 — Hiển thị lỗi có hướng xử lý**

Khi một yêu cầu thất bại, hệ thống SHALL hiển thị thông tin đủ để người dùng phân biệt lỗi cấu hình/API Key, lỗi nhà cung cấp, lỗi Backend, lỗi mạng hoặc lỗi trích xuất nội dung khi có thể xác định.

#### **UI-06 — Thiết lập và cá nhân hóa**

Giao diện SHALL cho phép người dùng quản lý các thiết lập được hỗ trợ, bao gồm nhà cung cấp AI, cấu hình API Key, hỗ trợ chủ động và chế độ bảo vệ dữ liệu.

#### **UI-07 — Ngôn ngữ giao diện**

Giao diện SHALL hỗ trợ tiếng Việt và tiếng Anh. Phạm vi bản dịch và hành vi khi nội dung nguồn sử dụng ngôn ngữ khác cần được xác nhận trong quá trình triển khai.

#### **UI-08 — Hướng dẫn lần đầu**

Trong lần thiết lập đầu tiên, hệ thống SHALL hướng dẫn người dùng đăng nhập và cấu hình API Key. Người dùng MAY bỏ qua các thiết lập cá nhân hóa không thiết yếu.

#### **UI-09 — Không làm gián đoạn việc đọc**

Khi người dùng không tương tác với extension, các tính năng hỗ trợ chủ động SHALL không gây gián đoạn không cần thiết. Người dùng SHALL có thể điều chỉnh hoặc tắt tính năng này.

#### **UI-10 — Hiển thị giới hạn hỗ trợ nội dung**

Khi không thể trích xuất nội dung từ trang hoặc tài liệu, hệ thống SHALL thông báo giới hạn đó và SHALL NOT trình bày kết quả như thể mô hình đã đọc được toàn bộ nội dung.

### **3.1.2 Hardware Interfaces**

#### **HW-01 — Thiết bị người dùng**

MVP SHALL hoạt động trên máy tính có trình duyệt Chrome tương thích. Không yêu cầu giao tiếp trực tiếp với thiết bị phần cứng chuyên dụng.

#### **HW-02 — Tài nguyên thiết bị**

Extension SHALL hoạt động trong giới hạn tài nguyên và cơ chế thực thi do trình duyệt cung cấp. Các thao tác xử lý nặng không được mặc định là có thể chạy vô hạn hoặc không giới hạn tài nguyên trên thiết bị người dùng.

#### **HW-03 — Thiết bị ngoại vi**

Không có yêu cầu bắt buộc về camera, microphone, cảm biến, thiết bị đọc sách hoặc phần cứng chuyên dụng trong MVP.

### **3.1.3 Software Interfaces (APIs/Databases)**

#### **SWI-01 — Chrome Extension APIs**

Extension SHALL sử dụng các API của Chrome phù hợp với quyền đã khai báo để tương tác với tab, nội dung trang và trạng thái extension.

Các quyền truy cập phải được giới hạn theo nhu cầu chức năng. ADD và tài liệu triển khai phải xác định quyền cụ thể và cơ chế xin quyền tương ứng.

#### **SWI-02 — Backend API**

Extension SHALL giao tiếp với Backend thông qua hợp đồng API được xác định rõ, phục vụ các chức năng tài khoản, cấu hình người dùng, dữ liệu hội thoại và đồng bộ.

Các API liên quan tài khoản SHALL xác thực người gọi và kiểm tra quyền truy cập đối với dữ liệu thuộc tài khoản.

#### **SWI-03 — AI Service Interface**

Thành phần gọi AI SHALL truyền yêu cầu, nội dung ngữ cảnh được phép gửi, thông tin lựa chọn tác vụ và các tham số cần thiết theo hợp đồng giao tiếp đã thống nhất.

Hợp đồng SHALL xác định cấu trúc phản hồi thành công, lỗi có thể xử lý và lỗi không xác định.

#### **SWI-04 — Provider API**

AI Service SHALL hỗ trợ giao tiếp với các nhà cung cấp AI được cấu hình và xác nhận hỗ trợ.

Mỗi tích hợp SHALL xử lý thông tin xác thực, tham số yêu cầu, phản hồi và lỗi theo hợp đồng tương ứng của nhà cung cấp.

Danh sách nhà cung cấp chính thức chưa được chốt trong bản SRS này.

#### **SWI-05 — Persistence interfaces**

Các chức năng lưu dữ liệu SHALL sử dụng giao diện lưu trữ phù hợp với phạm vi dữ liệu cục bộ và dữ liệu máy chủ.

Dữ liệu hội thoại được lưu theo tài khoản SHALL được phân tách theo quyền sở hữu. Yêu cầu về schema, công nghệ cơ sở dữ liệu và chiến lược lưu trữ thuộc ADD.

#### **SWI-06 — Interface versioning**

Các giao diện giữa Extension, Backend và AI Service SHALL có cách xác định phiên bản hoặc cơ chế tương thích để thay đổi hợp đồng không gây lỗi âm thầm cho các thành phần phụ thuộc.

#### **SWI-07 — External help website**

Hệ thống MAY dẫn người dùng đến website hướng dẫn tĩnh bên ngoài để hỗ trợ cài đặt và xử lý sự cố. Website này không phải một thành phần bắt buộc của ranh giới hệ thống chính.

### **3.1.4 Communication Interfaces (Protocols)**

#### **COM-01 — Giao tiếp qua mạng**

Giao tiếp từ xa có dữ liệu tài khoản, hội thoại hoặc thông tin xác thực SHALL sử dụng giao thức bảo mật phù hợp, như HTTPS/TLS theo cấu hình triển khai được phê duyệt.

#### **COM-02 — Định dạng dữ liệu**

Các giao diện HTTP API SHALL sử dụng định dạng dữ liệu được xác định trong hợp đồng API. JSON là định dạng đề xuất cho các API ứng dụng, trừ khi một giao diện có yêu cầu khác.

#### **COM-03 — Xử lý mất kết nối**

Khi mất kết nối, hệ thống SHALL thông báo trạng thái phù hợp, bảo toàn nội dung chưa gửi trong phạm vi đã quy định và SHALL NOT tự động gửi nội dung đang chờ chỉ vì kết nối đã khôi phục.

#### **COM-04 — Xác thực giao tiếp**

Các giao diện yêu cầu xác thực SHALL sử dụng cơ chế xác thực được phê duyệt. Thông tin xác thực SHALL NOT được ghi trực tiếp vào log ứng dụng ở dạng có thể sử dụng lại.

#### **COM-05 — Timeout và lỗi giao tiếp**

Các yêu cầu giao tiếp từ xa SHALL có cơ chế xử lý timeout và lỗi. Giá trị timeout cụ thể và chính sách retry SHALL được xác định trong thiết kế hoặc cấu hình đã phê duyệt.

#### **COM-06 — Không tự động gửi lại yêu cầu nhạy cảm**

Hệ thống SHALL NOT tự động gửi lại một yêu cầu AI đã bị chặn hoặc đang chờ người dùng xác nhận. Người dùng phải chủ động thực hiện thao tác gửi lại.

---

## **3.2 Software System Features & Functional Requirements**

### **3.2.1 Feature A — Account and Session Management**

**Mục đích:** Cho phép người dùng truy cập hệ thống và quản lý dữ liệu theo tài khoản.

**Inputs:** Thông tin đăng ký, thông tin đăng nhập, yêu cầu đăng xuất hoặc trạng thái phiên.

**Processing:** Xác thực thông tin, kiểm tra phiên, xác định tài khoản hiện hành và quyền truy cập dữ liệu.

**Outputs:** Kết quả xác thực, trạng thái phiên và thông báo lỗi phù hợp.

* FR-ACC-01: Hệ thống SHALL cho phép người dùng đăng ký và đăng nhập bằng phương thức được hỗ trợ.  
* FR-ACC-02: Hệ thống SHALL yêu cầu đăng nhập đối với các chức năng phụ thuộc tài khoản.  
* FR-ACC-03: Khi phiên hết hạn hoặc người dùng đăng xuất, hệ thống SHALL dừng các thao tác cần xác thực cho đến khi người dùng đăng nhập lại.  
* FR-ACC-04: Khi chuyển tài khoản trên cùng trình duyệt, hệ thống SHALL xóa dữ liệu phiên làm việc đang hoạt động của tài khoản trước khỏi vùng làm việc hiện tại và SHALL NOT để tài khoản mới truy cập dữ liệu đang chờ của tài khoản trước.  
* FR-ACC-05: Sau khi đăng nhập lại, hệ thống SHALL cho phép người dùng tiếp tục thao tác theo trạng thái dữ liệu được hỗ trợ, nhưng SHALL NOT tự động gửi các yêu cầu AI còn chờ.  
* FR-ACC-06: Khi phiên hết hạn, hệ thống SHALL bảo toàn trạng thái đọc và nội dung chưa gửi ở phạm vi cục bộ đã quy định để người dùng có thể tiếp tục sau khi đăng nhập, nếu môi trường vẫn cho phép.

### **3.2.2 Feature B — Onboarding and User Configuration**

**Mục đích:** Hỗ trợ người dùng thiết lập hệ thống lần đầu và quản lý tùy chọn.

**Inputs:** Lựa chọn nhà cung cấp, API Key, thiết lập cá nhân hóa và trạng thái các chế độ hỗ trợ.

**Processing:** Kiểm tra dữ liệu cấu hình, lưu thiết lập và áp dụng chúng vào phiên sử dụng.

**Outputs:** Trạng thái cấu hình và thông báo thành công hoặc lỗi.

* FR-CFG-01: Hệ thống SHALL hướng dẫn người dùng thiết lập API Key trước khi sử dụng các chức năng AI cần khóa đó.  
* FR-CFG-02: Hệ thống SHALL cho phép người dùng thiết lập một nhà cung cấp AI mặc định.  
* FR-CFG-03: Hệ thống SHALL cho phép người dùng thay đổi nhà cung cấp cho từng yêu cầu nếu nhà cung cấp đó đã được cấu hình hợp lệ.  
* FR-CFG-04: Hệ thống SHALL cho phép người dùng bật hoặc tắt hỗ trợ chủ động.  
* FR-CFG-05: Hệ thống SHALL cho phép người dùng bật hoặc tắt chế độ bảo vệ dữ liệu theo chính sách được xác định.  
* FR-CFG-06: Hệ thống SHALL lưu các tùy chọn tài khoản được hỗ trợ và đồng bộ chúng giữa các thiết bị theo quy tắc dữ liệu tài khoản.  
* FR-CFG-07: Hệ thống SHALL phân biệt thiết lập thiết yếu với thiết lập cá nhân hóa không thiết yếu để cho phép bỏ qua phần thiết lập không bắt buộc.

### **3.2.3 Feature C — API Key Management**

**Mục đích:** Quản lý thông tin xác thực cần thiết để sử dụng nhà cung cấp AI.

**Inputs:** API Key mới, yêu cầu thay thế key, yêu cầu kiểm tra hoặc thông tin lỗi từ nhà cung cấp.

**Processing:** Kiểm tra key qua cơ chế phù hợp của nhà cung cấp, lưu hoặc thay thế cấu hình khi kiểm tra đạt yêu cầu.

**Outputs:** Trạng thái key và thông báo lỗi cụ thể.

* FR-KEY-01: Hệ thống SHALL cho phép cấu hình tối đa một API Key đang hoạt động cho mỗi nhà cung cấp trên mỗi tài khoản, trong phạm vi thiết kế được phê duyệt.  
* FR-KEY-02: Hệ thống SHALL cho phép thay thế API Key hiện có.  
* FR-KEY-03: Hệ thống SHALL kiểm tra API Key mới trước khi xác nhận thay thế key đang hoạt động.  
* FR-KEY-04: Khi key không hợp lệ hoặc hết hạn, hệ thống SHALL thông báo lỗi phù hợp và đánh dấu nhà cung cấp tương ứng là chưa sẵn sàng cho đến khi cấu hình được sửa.  
* FR-KEY-05: Khi nhà cung cấp không khả dụng do lỗi key hoặc hạn mức, hệ thống SHALL không làm mất khả năng sử dụng các nhà cung cấp khác đã cấu hình hợp lệ.  
* FR-KEY-06: Hệ thống SHALL NOT tự động gửi lại yêu cầu thất bại chỉ vì người dùng đã thay thế hoặc sửa API Key.  
* FR-KEY-07: Hệ thống SHALL bảo vệ API Key khỏi việc truy cập trái phép và SHALL NOT hiển thị key đầy đủ trong log hoặc thông báo lỗi thông thường.

### **3.2.4 Feature D — Content Extraction and Reading Context**

**Mục đích:** Thu thập nội dung phù hợp để hỗ trợ đọc và trả lời câu hỏi.

**Inputs:** Trang web hiện tại, đoạn văn bản được chọn hoặc tài liệu thuộc loại có hỗ trợ.

**Processing:** Đọc nội dung được phép truy cập, trích xuất văn bản, xác định phạm vi nội dung và xây dựng ngữ cảnh.

**Outputs:** Nội dung trích xuất được, thông tin trạng thái hoặc lỗi trích xuất.

* FR-CNT-01: Hệ thống SHALL cho phép người dùng khởi tạo hỗ trợ đọc từ một đoạn văn bản được chọn.  
* FR-CNT-02: Hệ thống SHALL cố gắng trích xuất nội dung trang hiện tại khi người dùng yêu cầu hỗ trợ dựa trên trang.  
* FR-CNT-03: Hệ thống SHALL thử hỗ trợ PDF hoặc nội dung nhúng khi cơ chế trích xuất và quyền truy cập cho phép.  
* FR-CNT-04: Nếu không trích xuất được nội dung cần thiết, hệ thống SHALL thông báo giới hạn và SHALL NOT giả định nội dung chưa đọc được đã được cung cấp cho mô hình.  
* FR-CNT-05: Hệ thống SHALL duy trì ngữ cảnh hoạt động riêng cho từng tab.  
* FR-CNT-06: Khi người dùng chuyển tab, hệ thống SHALL giữ ngữ cảnh tương ứng với từng tab trong phạm vi phiên còn hiệu lực và SHALL NOT tự động trộn nội dung giữa các tab.  
* FR-CNT-07: Hệ thống SHALL NOT tự động khôi phục nội dung trang cũ chỉ vì người dùng mở lại một hội thoại đã lưu.  
* FR-CNT-08: Khi mở lại extension, hệ thống SHALL khôi phục hội thoại gần nhất theo hành vi đã xác định nhưng SHALL NOT mặc định khôi phục trạng thái trang đọc, ngữ cảnh cũ hoặc cấu hình hỗ trợ đang hoạt động.

### **3.2.5 Feature E — AI Reading Assistance**

**Mục đích:** Cung cấp các tác vụ hỗ trợ hiểu nội dung thông qua AI.

**Inputs:** Nội dung nguồn được phép sử dụng, câu hỏi của người dùng, loại tác vụ và ngữ cảnh hội thoại phù hợp.

**Processing:** Xác định tác vụ, chuẩn bị ngữ cảnh, gửi yêu cầu đến AI Service và xử lý phản hồi.

**Outputs:** Câu trả lời, kết quả tác vụ hoặc thông báo lỗi.

* FR-AI-01: Hệ thống SHALL hỗ trợ giải thích nội dung.  
* FR-AI-02: Hệ thống SHALL hỗ trợ dịch nội dung.  
* FR-AI-03: Hệ thống SHALL hỗ trợ tóm tắt nội dung.  
* FR-AI-04: Hệ thống SHALL hỗ trợ hỏi đáp dựa trên nội dung đọc được và câu hỏi của người dùng.  
* FR-AI-05: Hệ thống SHALL cho phép nhiều lượt hỏi đáp trong cùng hội thoại và sử dụng ngữ cảnh hội thoại phù hợp.  
* FR-AI-06: Hệ thống SHALL ưu tiên nội dung nguồn hiện tại khi tác vụ yêu cầu giải thích hoặc hỏi đáp về nội dung đó.  
* FR-AI-07: Khi bằng chứng từ nguồn không đủ để hỗ trợ kết luận, hệ thống SHALL thể hiện sự không chắc chắn hoặc yêu cầu người dùng bổ sung ngữ cảnh.  
* FR-AI-08: Khi câu trả lời có sử dụng kiến thức nền ngoài nội dung nguồn, hệ thống SHALL phân biệt phần được nguồn hỗ trợ với phần bổ sung khi có thể xác định đáng tin cậy.  
* FR-AI-09: Khi AI Service hoặc nhà cung cấp thất bại, hệ thống SHALL hiển thị trạng thái thất bại và bảo toàn yêu cầu chưa gửi hoặc chưa hoàn tất theo quy tắc lưu tạm.  
* FR-AI-10: Hệ thống SHALL NOT tự động gửi lại yêu cầu sau khi người dùng sửa cấu hình hoặc khi kết nối được khôi phục.  
* FR-AI-11: Hệ thống SHALL không khẳng định đã phân tích toàn bộ tài liệu nếu chỉ một phần nội dung được trích xuất thành công.  
* FR-AI-12: Hệ thống SHALL cho phép người dùng đổi nhà cung cấp trước khi chủ động gửi yêu cầu khác.

### **3.2.6 Feature F — Comprehension Questions**

**Mục đích:** Hỗ trợ người dùng tự kiểm tra mức độ hiểu nội dung.

**Inputs:** Nội dung đã đọc, cấu hình hỗ trợ và lựa chọn của người dùng.

**Processing:** Xác định điều kiện gợi ý, tạo câu hỏi dựa trên nội dung có thể truy cập và nhận câu trả lời của người dùng.

**Outputs:** Câu hỏi, trạng thái trả lời hoặc thông báo không đủ nội dung.

* FR-CMP-01: Hệ thống SHALL hỗ trợ tạo câu hỏi kiểm tra mức độ hiểu bài dựa trên nội dung có thể truy cập.  
* FR-CMP-02: Khi tính năng được kích hoạt theo điều kiện cấu hình, hệ thống SHALL có thể đề xuất câu hỏi sau khi người dùng đọc một lượng nội dung phù hợp.  
* FR-CMP-03: Hệ thống SHALL cho phép người dùng trả lời, bỏ qua hoặc điều chỉnh việc hỗ trợ theo khả năng giao diện.  
* FR-CMP-04: Khi nội dung nguồn không đủ để tạo câu hỏi phù hợp, hệ thống SHALL thông báo giới hạn thay vì tạo câu hỏi và khẳng định chúng dựa trên toàn bộ nguồn.  
* FR-CMP-05: Hệ thống SHALL tuân thủ chính sách bảo vệ dữ liệu khi gửi nội dung phục vụ tạo câu hỏi.

### **3.2.7 Feature G — Proactive Reading Support and Personalization**

**Mục đích:** Đưa ra hỗ trợ phù hợp với quá trình đọc mà không làm mất quyền kiểm soát của người dùng.

**Inputs:** Ngữ cảnh đọc hiện tại, tùy chọn người dùng và điều kiện kích hoạt hỗ trợ.

**Processing:** Đánh giá điều kiện gợi ý, xác định loại hỗ trợ có thể đề xuất và áp dụng cấu hình cá nhân hóa.

**Outputs:** Gợi ý hỗ trợ, trạng thái tính năng và thiết lập tương ứng.

* FR-PRV-01: Hệ thống SHALL bật hỗ trợ chủ động theo cấu hình mặc định đã thống nhất cho MVP.  
* FR-PRV-02: Hệ thống SHALL cho phép người dùng điều chỉnh hoặc tắt hỗ trợ chủ động.  
* FR-PRV-03: Hệ thống SHALL không tự thực hiện hành động thay mặt người dùng nếu hành động đó cần sự chấp thuận của người dùng.  
* FR-PRV-04: Hệ thống SHALL áp dụng các tùy chọn cá nhân hóa đã lưu khi chúng có liên quan đến tác vụ hiện tại.  
* FR-PRV-05: Hệ thống SHALL không làm gián đoạn nội dung đọc chính chỉ để hiển thị một gợi ý không thiết yếu.  
* FR-PRV-06: Hệ thống SHALL không coi việc phát hiện nội dung hoặc trạng thái đọc là chính xác tuyệt đối khi cơ chế phát hiện có giới hạn.

### **3.2.8 Feature H — Privacy and Data Protection**

**Mục đích:** Giảm nguy cơ gửi dữ liệu nhạy cảm đến nhà cung cấp AI mà người dùng không có chủ đích cho phép.

**Inputs:** Nội dung trang, đoạn văn bản được chọn, câu hỏi, ngữ cảnh hội thoại và trạng thái chế độ bảo vệ.

**Processing:** Kiểm tra chính sách bảo vệ, phát hiện dấu hiệu nhạy cảm khi có thể, chặn hoặc yêu cầu xác nhận trước khi truyền dữ liệu.

**Outputs:** Cho phép tiếp tục, cảnh báo, trạng thái bị chặn hoặc lựa chọn xử lý tiếp theo.

* FR-SEC-01: Chế độ bảo vệ dữ liệu SHALL được bật theo mặc định.  
* FR-SEC-02: Hệ thống SHALL cho phép người dùng thay đổi trạng thái chế độ bảo vệ theo tài khoản và đồng bộ thiết lập đó theo quy tắc đã xác định.  
* FR-SEC-03: Hệ thống SHALL áp dụng chính sách bảo vệ cho nội dung trang, đoạn văn bản được chọn, câu hỏi và ngữ cảnh hội thoại liên quan được dự kiến gửi đến AI.  
* FR-SEC-04: Nếu phát hiện dấu hiệu nội dung nhạy cảm, hệ thống SHALL cảnh báo và mặc định chặn truyền nội dung đó ra ngoài cho đến khi người dùng chủ động xử lý.  
* FR-SEC-05: Khi không chắc chắn về mức độ nhạy cảm, hệ thống SHALL ưu tiên chặn theo chính sách mặc định và cho phép người dùng xem xét trước khi quyết định.  
* FR-SEC-06: Hệ thống SHALL giải thích lý do chặn và cung cấp lựa chọn sửa hoặc loại bỏ nội dung nhạy cảm trước khi gửi lại.  
* FR-SEC-07: Hệ thống SHALL chỉ cho phép bỏ qua cơ chế chặn sau khi người dùng chủ động tắt chế độ bảo vệ và xác nhận theo giao diện được xác định.  
* FR-SEC-08: Hệ thống SHALL NOT âm thầm gửi lại yêu cầu đã bị chặn.  
* FR-SEC-09: Chính sách bảo vệ SHALL áp dụng nhất quán bất kể nhà cung cấp AI đang được chọn.  
* FR-SEC-10: Khi yêu cầu bị chặn, hệ thống SHALL vẫn cho phép các thao tác không cần gửi nội dung bị chặn, chẳng hạn đọc trang, quản lý thiết lập và chỉnh sửa yêu cầu.  
* FR-SEC-11: Hệ thống SHALL phân biệt phạm vi dữ liệu mà ứng dụng có thể kiểm soát với hoạt động xử lý nội bộ của nhà cung cấp bên ngoài; hệ thống SHALL NOT tuyên bố kiểm soát những hoạt động ngoài khả năng của mình.  
* FR-SEC-12: Việc loại trừ telemetry hoặc dữ liệu vận hành khỏi chính sách chặn nội dung nhạy cảm SHALL NOT được hiểu là miễn trừ các nghĩa vụ bảo vệ dữ liệu và kiểm soát truy cập áp dụng cho những dữ liệu đó.

### **3.2.9 Feature I — Conversation History and Data Lifecycle**

**Mục đích:** Lưu và quản lý dữ liệu đọc theo tài khoản để hỗ trợ tiếp tục sử dụng.

**Inputs:** Yêu cầu lưu, tải, xóa hội thoại và dữ liệu liên quan.

**Processing:** Xác định quyền sở hữu, đọc hoặc cập nhật dữ liệu, áp dụng phạm vi xóa.

**Outputs:** Hội thoại, lịch sử hoặc kết quả thao tác dữ liệu.

* FR-HIS-01: Hệ thống SHALL lưu hội thoại và lịch sử đọc theo cấu hình mặc định đã thống nhất.  
* FR-HIS-02: Hệ thống SHALL cho phép người dùng truy cập hội thoại đã lưu thuộc tài khoản của mình.  
* FR-HIS-03: Hệ thống SHALL lưu ngữ cảnh hội thoại cần thiết cho việc tiếp tục tương tác trong phạm vi dữ liệu đã được quy định.  
* FR-HIS-04: Hệ thống SHALL cho phép xóa một hội thoại cụ thể.  
* FR-HIS-05: Hệ thống SHALL cho phép xóa toàn bộ lịch sử thuộc phạm vi được xác định.  
* FR-HIS-06: Trước thao tác xóa có phạm vi rộng, hệ thống SHALL cho người dùng biết phạm vi dữ liệu sẽ bị ảnh hưởng.  
* FR-HIS-07: Hệ thống SHALL phân biệt việc xóa lịch sử với việc xóa tài khoản.  
* FR-HIS-08: Hệ thống SHALL không cho người dùng truy cập dữ liệu hội thoại thuộc tài khoản khác.  
* FR-HIS-09: Chế độ bảo vệ dữ liệu SHALL NOT được diễn giải là chế độ không lưu lịch sử.  
* FR-HIS-10: Hệ thống SHALL công bố hoặc thể hiện phù hợp giới hạn liên quan đến việc xóa dữ liệu khỏi bản sao lưu nếu dữ liệu đó chưa được loại bỏ ngay lập tức.

### **3.2.10 Feature J — Synchronization and Conflict Handling**

**Mục đích:** Duy trì dữ liệu tài khoản giữa các thiết bị.

**Inputs:** Dữ liệu đã lưu, thay đổi hội thoại, thiết lập và trạng thái kết nối.

**Processing:** Đồng bộ thay đổi, phát hiện lỗi hoặc xung đột và giữ an toàn dữ liệu.

**Outputs:** Dữ liệu được đồng bộ, thông báo lỗi hoặc yêu cầu người dùng quyết định khi có xung đột.

* FR-SYN-01: Hệ thống SHALL đồng bộ hội thoại và dữ liệu tài khoản được hỗ trợ giữa các thiết bị.  
* FR-SYN-02: Khi đồng bộ thất bại, hệ thống SHALL giữ dữ liệu đã lưu cục bộ trong phạm vi được hỗ trợ.  
* FR-SYN-03: Hệ thống SHALL thử đồng bộ lại dữ liệu đã lưu sau khi kết nối được khôi phục theo chính sách đồng bộ.  
* FR-SYN-04: Việc khôi phục kết nối SHALL NOT tự động gửi yêu cầu AI đang chờ người dùng xác nhận.  
* FR-SYN-05: Khi phát hiện xung đột dữ liệu, hệ thống SHALL thông báo và SHALL NOT âm thầm ghi đè dữ liệu đang được bảo toàn.  
* FR-SYN-06: Hệ thống SHALL cung cấp cách xử lý xung đột phù hợp để người dùng quyết định khi cần.  
* FR-SYN-07: Hệ thống SHALL không đồng bộ yêu cầu chưa xử lý đang được giữ tạm trên thiết bị theo quy tắc đã thống nhất.  
* FR-SYN-08: Hệ thống SHALL không mặc định khôi phục ngữ cảnh đọc đang hoạt động từ thiết bị khác chỉ vì hội thoại đã được đồng bộ.

### **3.2.11 Feature K — Error Handling and Recovery**

**Mục đích:** Hỗ trợ người dùng tiếp tục làm việc khi các thành phần hoặc dịch vụ bên ngoài gặp lỗi.

**Inputs:** Lỗi xác thực, lỗi API, mất mạng, lỗi Backend, lỗi đồng bộ hoặc lỗi trích xuất.

**Processing:** Phân loại lỗi, xác định thao tác có thể tiếp tục và giữ dữ liệu cần thiết.

**Outputs:** Thông báo, dữ liệu được bảo toàn và lựa chọn khắc phục.

* FR-ERR-01: Hệ thống SHALL phân biệt lỗi cấu hình/API Key với lỗi dịch vụ chung khi có đủ thông tin.  
* FR-ERR-02: Hệ thống SHALL giữ yêu cầu chưa được gửi thành công trong vùng lưu tạm cục bộ theo phạm vi đã xác định.  
* FR-ERR-03: Hệ thống SHALL cho phép người dùng sửa hoặc xóa yêu cầu chưa gửi.  
* FR-ERR-04: Hệ thống SHALL yêu cầu người dùng chủ động gửi lại yêu cầu sau khi khắc phục lỗi.  
* FR-ERR-05: Khi ngoại tuyến, hệ thống SHALL thông báo giới hạn chức năng phụ thuộc mạng.  
* FR-ERR-06: Khi dịch vụ Backend không khả dụng, hệ thống SHALL thông báo các chức năng bị ảnh hưởng nhưng không mặc định coi mọi chức năng đọc cục bộ là không thể sử dụng.  
* FR-ERR-07: Khi dịch vụ AI không khả dụng, hệ thống SHALL không hiển thị phản hồi giả như một kết quả AI thành công.  
* FR-ERR-08: Khi lỗi đồng bộ xảy ra, hệ thống SHALL bảo toàn dữ liệu đã lưu và thông báo cho người dùng khi cần.  
* FR-ERR-09: Hệ thống SHALL không tự động gửi nội dung đang chờ khi người dùng đăng nhập lại hoặc khi mạng được khôi phục.  
* FR-ERR-10: Hệ thống SHALL cho phép người dùng tiếp tục đọc trang web gốc nếu trang đó vẫn hoạt động độc lập với extension.

### **3.2.12 Feature L — Monitoring and Operational Support**

**Mục đích:** Hỗ trợ nhóm phát triển và vận hành phát hiện lỗi cơ bản.

**Inputs:** Sự kiện trạng thái dịch vụ, lỗi ứng dụng và kết quả kết nối tới các thành phần cần thiết.

**Processing:** Ghi nhận sự kiện được phép, tổng hợp trạng thái cơ bản và hỗ trợ điều tra sự cố.

**Outputs:** Log kỹ thuật, trạng thái dịch vụ và thông tin phục vụ khắc phục lỗi.

* FR-OPS-01: Hệ thống SHALL hỗ trợ theo dõi tính sẵn sàng dịch vụ, lỗi hệ thống và kết nối tới các thành phần thiết yếu.  
* FR-OPS-02: Hệ thống SHALL ghi nhận đủ thông tin kỹ thuật để phân loại lỗi thông thường trong phạm vi vận hành.  
* FR-OPS-03: Log SHALL NOT chứa API Key đầy đủ, thông tin xác thực có thể tái sử dụng hoặc nội dung nhạy cảm không cần thiết cho mục đích vận hành.  
* FR-OPS-04: Hệ thống SHALL cung cấp thông tin lỗi đủ hữu ích để nhóm phát triển xử lý sự cố mà không tiết lộ dữ liệu tài khoản trái phép.  
* FR-OPS-05: Quy trình sao lưu và phục hồi SHALL được quy định trong tài liệu vận hành và thiết kế; các chỉ tiêu RTO/RPO chỉ được đưa vào khi được phê duyệt.

---

## **3.3 Software Non-Functional Requirements**

### **3.3.1 Performance Requirements**

Các yêu cầu hiệu năng cần được kiểm chứng trong điều kiện tải và môi trường xác định. Chưa có ngưỡng định lượng được phê duyệt, vì vậy các yêu cầu dưới đây quy định hành vi cần có và đánh dấu các chỉ tiêu cần chốt.

* NFR-PER-01: Giao diện SHALL phản hồi trạng thái khi người dùng khởi tạo tác vụ, để người dùng phân biệt yêu cầu đang xử lý với giao diện không phản hồi.  
* NFR-PER-02: Hệ thống SHALL xử lý các yêu cầu AI theo cơ chế bất đồng bộ phù hợp để việc chờ phản hồi không khóa toàn bộ giao diện đọc.  
* NFR-PER-03: Hệ thống SHALL không gửi lại nhiều lần cùng một yêu cầu do lỗi giao diện hoặc thao tác lặp ngoài chủ đích mà không có chính sách xử lý rõ ràng.  
* NFR-PER-04: Thời gian phản hồi mục tiêu cho các tác vụ giao diện, Backend và AI SHALL được xác định bằng ngưỡng đo riêng theo loại tác vụ.  
* NFR-PER-05: Hệ thống SHALL được đánh giá với các trường hợp nội dung ngắn, nội dung dài và lỗi nhà cung cấp trước khi kết luận mức độ đáp ứng hiệu năng.  
* NFR-PER-06: Mức sử dụng tài nguyên trên trình duyệt và Backend SHALL được đánh giá trong phạm vi tải mục tiêu đã được phê duyệt.

**Các giá trị cần xác nhận:** thời gian phản hồi mục tiêu, kích thước nội dung tối đa, số người dùng đồng thời, tải yêu cầu AI và giới hạn tài nguyên. Không áp dụng số liệu giả định như tiêu chí nghiệm thu chính thức.

### **3.3.2 Security & Data Protection Requirements**

* NFR-SEC-01: Hệ thống SHALL xác thực người dùng trước khi truy cập chức năng và dữ liệu phụ thuộc tài khoản.  
* NFR-SEC-02: Hệ thống SHALL thực thi phân quyền ở phía máy chủ đối với dữ liệu thuộc tài khoản.  
* NFR-SEC-03: Hệ thống SHALL bảo vệ dữ liệu nhạy cảm và thông tin xác thực khi truyền qua mạng.  
* NFR-SEC-04: Hệ thống SHALL sử dụng phương thức lưu API Key an toàn, giới hạn quyền truy cập và tránh lưu hoặc ghi log key ở dạng văn bản rõ không được bảo vệ.  
* NFR-SEC-05: Hệ thống SHALL giới hạn quyền Chrome Extension ở mức cần thiết cho các chức năng được phê duyệt.  
* NFR-SEC-06: Hệ thống SHALL thực thi chính sách bảo vệ dữ liệu trước khi truyền nội dung AI ra ngoài, độc lập với nhà cung cấp được chọn.  
* NFR-SEC-07: Hệ thống SHALL bảo đảm dữ liệu hội thoại không bị truy cập trái phép khi người dùng đăng xuất hoặc chuyển tài khoản.  
* NFR-SEC-08: Hệ thống SHALL tránh ghi log nội dung câu hỏi, trang đang đọc hoặc ngữ cảnh hội thoại nếu không cần thiết cho mục đích kỹ thuật đã xác định.  
* NFR-SEC-09: Hệ thống SHALL phân biệt dữ liệu nghiệp vụ, dữ liệu vận hành và dữ liệu gửi cho nhà cung cấp AI để áp dụng chính sách bảo vệ phù hợp.  
* NFR-SEC-10: Hệ thống SHALL không tuyên bố dữ liệu đã được xóa hoàn toàn khỏi hệ thống bên ngoài nếu không có cơ chế xác nhận tương ứng.  
* NFR-SEC-11: Cơ chế phát hiện nội dung nhạy cảm SHALL được xem là cơ chế giảm rủi ro, không phải bảo đảm phát hiện tuyệt đối mọi dữ liệu nhạy cảm.  
* NFR-SEC-12: Các chính sách lưu trữ, thời hạn giữ dữ liệu, xóa dữ liệu và xử lý bản sao lưu SHALL được xác định trước khi phát hành chính thức.

### **3.3.3 Usability & Software Quality Attributes**

#### **NFR-USA — Usability**

* NFR-USA-01: Các thao tác hỗ trợ đọc chính SHALL có thể được thực hiện từ giao diện extension mà không bắt buộc người dùng rời trang đang đọc.  
* NFR-USA-02: Thông báo lỗi SHALL nêu vấn đề theo cách người dùng có thể hiểu và cung cấp hướng xử lý khi xác định được.  
* NFR-USA-03: Cảnh báo bảo vệ dữ liệu SHALL giải thích lý do chặn và các lựa chọn tiếp theo.  
* NFR-USA-04: Các gợi ý chủ động SHALL có thể được điều chỉnh hoặc tắt.  
* NFR-USA-05: Hệ thống SHALL không trình bày nội dung mô phỏng trong demo như một chức năng AI thực sự đã hoàn tất.

#### **NFR-REL — Reliability and Recoverability**

* NFR-REL-01: Hệ thống SHALL bảo toàn dữ liệu đã lưu khi thao tác đồng bộ thất bại.  
* NFR-REL-02: Hệ thống SHALL không âm thầm ghi đè dữ liệu khi phát hiện xung đột đồng bộ.  
* NFR-REL-03: Hệ thống SHALL bảo toàn yêu cầu chưa gửi theo quy tắc lưu tạm và SHALL không tự động gửi yêu cầu khi mạng được khôi phục.  
* NFR-REL-04: Hệ thống SHALL phân biệt trạng thái thành công, thất bại và chưa xác định của yêu cầu AI.  
* NFR-REL-05: Quy trình sao lưu và phục hồi SHALL được kiểm tra theo phạm vi vận hành được phê duyệt.

#### **NFR-MNT — Maintainability and Testability**

* NFR-MNT-01: Các chức năng quản lý tài khoản, hỗ trợ đọc, bảo vệ dữ liệu và tích hợp nhà cung cấp SHALL có ranh giới trách nhiệm rõ ràng trong thiết kế phần mềm.  
* NFR-MNT-02: Các tích hợp nhà cung cấp AI SHALL có hợp đồng giao tiếp đủ rõ để kiểm thử và thay thế hoặc bổ sung nhà cung cấp.  
* NFR-MNT-03: Các lỗi quan trọng SHALL có thể được phân loại và điều tra thông qua thông tin kỹ thuật không làm lộ dữ liệu nhạy cảm.  
* NFR-MNT-04: Các yêu cầu SHALL có liên kết đến ít nhất một phương pháp xác minh phù hợp.  
* NFR-MNT-05: Các chức năng mô phỏng, chưa triển khai và bị hoãn SHALL được ghi nhận rõ trong phạm vi demo.

#### **NFR-CMP — Compatibility**

* NFR-CMP-01: Extension SHALL tương thích với phiên bản Chrome và cấu hình quyền được công bố trong phạm vi phát hành.  
* NFR-CMP-02: Giao diện giữa Extension, Backend và AI Service SHALL tuân thủ hợp đồng đã thống nhất.  
* NFR-CMP-03: Hệ thống SHALL xử lý trường hợp nhà cung cấp trả về lỗi hoặc cấu trúc phản hồi không như mong đợi mà không làm hỏng phiên làm việc.

#### **NFR-AI — AI Quality and Groundedness**

* NFR-AI-01: Các tác vụ được yêu cầu là giải thích hoặc hỏi đáp về nguồn SHALL sử dụng ngữ cảnh nguồn có thể truy cập khi phù hợp.  
* NFR-AI-02: Hệ thống SHALL có cách đánh giá chất lượng câu trả lời theo bộ dữ liệu và giao thức đánh giá được phê duyệt.  
* NFR-AI-03: Các kết quả đánh giá SHALL phân biệt chất lượng nội dung, mức độ phù hợp với nguồn và khả năng hoàn thành tác vụ.  
* NFR-AI-04: Ngưỡng chất lượng AI dùng để nghiệm thu SHALL được xác định sau khi thống nhất dữ liệu đánh giá, phương pháp đo và mục tiêu MVP.  
* NFR-AI-05: Hệ thống SHALL không tuyên bố câu trả lời luôn chính xác hoặc mọi nội dung đều được chứng minh bởi nguồn.

---

## **3.4 Software Design & Technology Constraints**

Phần này xác định các ràng buộc mà SRS cần phản ánh. Chi tiết cấu trúc thành phần, thư viện, schema và quyết định triển khai sẽ được ghi trong ADD.

* CON-01: Phần giao diện chính của người dùng SHALL được triển khai dưới dạng Chrome Extension.  
* CON-02: Công nghệ giao diện SHALL sử dụng React theo quyết định công nghệ của dự án.  
* CON-03: Backend SHALL sử dụng .NET theo quyết định công nghệ của dự án.  
* CON-04: AI Service SHALL được phát triển bằng Python và được tách thành thành phần logic riêng với Backend.  
* CON-05: Backend và AI Service SHALL giao tiếp qua giao diện được xác định rõ; việc tách logic không bắt buộc hai thành phần phải triển khai trên hai máy chủ vật lý khác nhau.  
* CON-06: Hệ thống SHALL hỗ trợ cách tích hợp nhà cung cấp AI cho phép thay đổi hoặc bổ sung provider mà không cần viết lại toàn bộ chức năng hỗ trợ đọc.  
* CON-07: Hệ thống SHALL áp dụng quy tắc bảo vệ dữ liệu thống nhất trước khi gửi yêu cầu đến bất kỳ provider nào được hỗ trợ.  
* CON-08: Hệ thống SHALL sử dụng cơ chế xác thực và phân quyền phù hợp với mô hình dịch vụ dùng chung.  
* CON-09: Công nghệ lưu trữ, mã hóa, triển khai, sao lưu, phục hồi và quản lý bí mật SHALL được ghi nhận trong ADD.  
* CON-10: Hệ thống SHALL không yêu cầu người dùng cuối tự triển khai Backend hoặc AI Service.  
* CON-11: Hệ thống SHALL không coi việc chạy AI hoàn toàn ngoại tuyến là yêu cầu MVP.  
* CON-12: Các tính năng mô phỏng trong demo SHALL được phân biệt với chức năng đã triển khai thực sự.  
* CON-13: Mọi công nghệ hoặc dịch vụ bên ngoài được sử dụng SHALL có hợp đồng giao tiếp và giới hạn phụ thuộc được ghi nhận.  
* CON-14: Các quyết định thiết kế liên quan đến API Key, dữ liệu nhạy cảm, lưu trữ hội thoại và đồng bộ SHALL được giải thích trong ADD và liên kết đến các yêu cầu tương ứng.

---

# **4\. Verification & Validation Matrix**

## **4.1 Verification methods**

Các phương pháp xác minh sử dụng trong ma trận:

* **T — Test:** Kiểm thử phần mềm hoặc kiểm thử tích hợp.  
* **I — Inspection:** Kiểm tra tài liệu, mã nguồn, cấu hình hoặc log.  
* **A — Analysis:** Phân tích dữ liệu, luồng xử lý hoặc kết quả đo.  
* **D — Demonstration:** Trình diễn kịch bản sử dụng.  
* **U — User Validation:** Người dùng mục tiêu thực hiện tác vụ và phản hồi.

Một yêu cầu có thể sử dụng nhiều phương pháp. Các mã kiểm thử dưới đây là mã đề xuất cho kế hoạch kiểm thử; chúng chưa đại diện cho test case đã được triển khai.

## **4.2 Requirements verification matrix**

| Test ID | Requirement IDs | Kịch bản / tiêu chí xác minh | Phương pháp | Ưu tiên |
| ----- | ----- | ----- | ----- | ----- |
| VT-01 | FR-ACC-01–06 | Đăng ký, đăng nhập, hết hạn phiên, đăng xuất và chuyển tài khoản đúng quy tắc; dữ liệu phiên trước không lộ cho tài khoản sau | T, D | Critical |
| VT-02 | FR-CFG-01–07 | Thiết lập lần đầu, bỏ qua tùy chọn không thiết yếu và lưu cấu hình đúng | T, D | High |
| VT-03 | FR-KEY-01–07, NFR-SEC-04 | Thêm key hợp lệ, key lỗi, thay thế key và kiểm tra không lộ key trong log | T, I | Critical |
| VT-04 | SWI-01, FR-CNT-01–08 | Đọc đoạn được chọn, trang web và các trường hợp PDF/nội dung nhúng được hỗ trợ; thông báo chính xác khi trích xuất thất bại | T, D | High |
| VT-05 | FR-AI-01–12 | Kiểm thử giải thích, dịch, tóm tắt và hỏi đáp; kiểm tra xử lý thiếu nguồn và lỗi provider | T, D | Critical |
| VT-06 | FR-CMP-01–05 | Tạo câu hỏi hiểu bài, trả lời, bỏ qua và xử lý thiếu nội dung | T, D | Medium |
| VT-07 | FR-PRV-01–06 | Bật/tắt hỗ trợ chủ động, kiểm tra hành vi không gây gián đoạn và không tự thực hiện hành động cần đồng ý | T, U | Medium |
| VT-08 | FR-SEC-01–12, COM-06 | Nội dung nghi nhạy cảm bị chặn theo mặc định; người dùng có thể sửa nội dung hoặc xác nhận theo chính sách; không tự động gửi lại | T, I | Critical |
| VT-09 | FR-HIS-01–10 | Lưu, tải, xóa một hội thoại và xóa toàn bộ lịch sử; kiểm tra đúng phạm vi xóa và quyền sở hữu | T, I | Critical |
| VT-10 | FR-SYN-01–08 | Đồng bộ thành công, đồng bộ lỗi, khôi phục kết nối và xung đột; dữ liệu không bị ghi đè âm thầm | T, D | Critical |
| VT-11 | FR-ERR-01–10, COM-03 | Mất mạng, Backend lỗi, provider lỗi và API Key lỗi; thông báo phù hợp, giữ yêu cầu và không tự động gửi khi mạng trở lại | T, D | Critical |
| VT-12 | FR-OPS-01–05, NFR-SEC-08 | Kiểm tra log, giám sát cơ bản và không ghi thông tin xác thực hoặc nội dung nhạy cảm không cần thiết | T, I | High |
| VT-13 | NFR-PER-01–06 | Đo độ trễ, tài nguyên và khả năng xử lý theo ngưỡng hiệu năng được phê duyệt | T, A | High |
| VT-14 | NFR-USA-01–05 | Người dùng mục tiêu hoàn thành các tác vụ đọc chính, hiểu thông báo lỗi và thao tác bảo vệ dữ liệu | U, D | High |
| VT-15 | NFR-REL-01–05 | Mô phỏng lỗi đồng bộ, lỗi dịch vụ và kiểm tra phục hồi dữ liệu trong phạm vi hỗ trợ | T, A | High |
| VT-16 | NFR-MNT-01–05 | Kiểm tra phân chia trách nhiệm, khả năng kiểm thử, phân loại lỗi và trạng thái các chức năng demo | I, T | Medium |
| VT-17 | NFR-CMP-01–03 | Kiểm tra trên môi trường Chrome được hỗ trợ và với phản hồi provider lỗi hoặc sai định dạng | T, I | High |
| VT-18 | NFR-AI-01–05 | Đánh giá groundedness, mức phù hợp với nguồn và chất lượng hoàn thành tác vụ bằng bộ dữ liệu, giao thức và ngưỡng đã phê duyệt | T, A | High |
| VT-19 | CON-01–14 | Kiểm tra các ràng buộc công nghệ, hợp đồng giao tiếp và tài liệu kiến trúc tương ứng | I, A | High |
| VT-20 | COM-01–06, SWI-02–06 | Kiểm tra giao thức, xác thực, timeout, xử lý lỗi và tương thích giao diện giữa các thành phần | T, I | High |

## **4.3 Demo acceptance and validation**

Bản demo được đánh giá bằng hai nhóm hoạt động:

1. **Scenario-based testing:** nhóm phát triển chạy các kịch bản thành công, lỗi và ngoại lệ.  
2. **User validation:** một nhóm nhỏ người dùng mục tiêu thử các tác vụ đọc và cung cấp phản hồi.

Các luồng ưu tiên xác minh gồm:

* Đăng nhập, cấu hình API Key và thực hiện tác vụ AI đầu tiên.  
* Chọn đoạn văn bản để giải thích hoặc dịch.  
* Hỏi đáp nhiều lượt về trang đang đọc.  
* Xử lý nội dung không thể trích xuất.  
* Chặn nội dung nghi nhạy cảm và cho người dùng xử lý tiếp.  
* Khôi phục sau lỗi API Key hoặc lỗi nhà cung cấp.  
* Mất mạng, lưu tạm yêu cầu và gửi lại thủ công.  
* Đồng bộ dữ liệu và xử lý xung đột.  
* Xóa lịch sử và kiểm tra cách ly dữ liệu giữa các tài khoản.

Các nhánh quan trọng có thể được mô phỏng trong demo nếu chưa triển khai đầy đủ, nhưng trạng thái mô phỏng phải được ghi nhận rõ ràng và không được tính là đã xác minh triển khai thực tế.

## **4.4 Traceability and acceptance status**

Mỗi yêu cầu SRS SHALL được quản lý với tối thiểu các trường:

| Trường | Mục đích |
| ----- | ----- |
| Requirement ID | Định danh duy nhất |
| Requirement statement | Nội dung yêu cầu |
| Source reference | Mã yêu cầu nguồn trong BRS/StRS/OpsCon/SyRS |
| Related SRS requirements | Quan hệ phân rã hoặc phụ thuộc |
| ADD reference | Thành phần hoặc quyết định thiết kế liên quan |
| Verification method | Phương pháp xác minh |
| Test ID | Mã kịch bản hoặc test case |
| Priority | Mức ưu tiên |
| Implementation status | Implemented / Simulated / Deferred / Not implemented |
| Verification status | Not tested / Pass / Fail / Blocked |
| Remarks | Giới hạn, giả định và quyết định còn mở |

Không được coi một yêu cầu là đã hoàn tất chỉ vì có thiết kế hoặc có test case. Trạng thái hoàn thành phải phản ánh bằng chứng triển khai và kết quả xác minh thực tế.

---

**Kết thúc SRS Draft 0.1.**

**Các nội dung cần xác nhận trước khi phát hành bản chính thức:**

1. Đối chiếu mã nguồn và nội dung yêu cầu với BRS, StRS, OpsCon, SyRS.  
2. Chốt danh sách provider và hợp đồng API được hỗ trợ.  
3. Chốt chính sách lưu API Key, thời hạn lưu dữ liệu và xử lý bản sao lưu.  
4. Phê duyệt các ngưỡng hiệu năng và tiêu chí đánh giá AI.  
5. Chốt danh sách tính năng MVP, mô phỏng và hoãn lại.  
6. Bổ sung mã ADD và test case thực tế vào RTM.

