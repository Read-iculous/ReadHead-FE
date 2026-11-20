Mình sẽ tiếp tục bằng văn bản nhé. Mình sẽ lập RTM bản sơ bộ cho Intelligent Reading Assistant, đồng thời rà soát các điểm cần làm rõ trước khi dùng RTM để phát triển SRS và ADD.

Lưu ý: hiện mình chưa có toàn văn cả bốn tài liệu trong ngữ cảnh truy xuất được. Vì vậy, bản dưới đây là truy vết theo nội dung và quyết định đã thống nhất, chưa phải RTM đã xác nhận chính xác từng mã yêu cầu nguồn. Mình sẽ không tự tạo mã BRS, StRS hay OpsCon rồi coi chúng là mã gốc.

# **1\. Cấu trúc truy vết đề xuất**

Luồng truy vết nên đi theo hai nhánh từ SyRS, phục vụ hai mục đích khác nhau:

BRS — Business Requirements

Mục tiêu, giá trị và kết quả kinh doanh mong muốn

StRS — Stakeholder Requirements

Nhu cầu của người đọc và các bên liên quan

OpsCon — Operational Concept

Bối cảnh vận hành, vai trò, kịch bản và xử lý ngoại lệ

SyRS — System Requirements

Hệ thống phải làm gì và những điều kiện hệ thống phải đáp ứng

SRS

Yêu cầu phần mềm chi tiết, giao diện, dữ liệu, hành vi và kiểm thử

ADD

Thành phần kiến trúc, trách nhiệm, giao tiếp, lưu trữ và quyết định thiết kế

OpsCon không nhất thiết phải là một tầng yêu cầu nằm giữa StRS và SyRS theo nghĩa nghiêm ngặt. Nó chủ yếu cung cấp bối cảnh vận hành và kịch bản để xác minh các yêu cầu hệ thống có đầy đủ hay không. Một yêu cầu SyRS có thể truy vết tới nhiều nguồn, và một yêu cầu nguồn có thể được hiện thực hóa bằng nhiều yêu cầu SyRS.

# **2\. RTM sơ bộ**

Quy ước mã tạm trong bảng:

* `BRS-*`, `StRS-*`, `OpsCon-*`: chưa điền mã gốc vì cần đối chiếu tài liệu.  
* `SY-*`: mã nhóm truy vết tạm do mình đặt để tổ chức bảng, không khẳng định đây là mã có trong SyRS Draft 0.1.  
* SRS và ADD sẽ được điền mã chính thức sau khi phân rã yêu cầu và thiết kế.

## **2.1. Danh mục truy vết chức năng**

| ID tạm | Yêu cầu / năng lực hệ thống | Nguồn cần đối chiếu | Đích SRS dự kiến | Đích ADD dự kiến |
| ----- | ----- | ----- | ----- | ----- |
| SY-01 | Đăng ký, đăng nhập và quản lý phiên người dùng | BRS, StRS, OpsCon | Xác thực, phiên, lỗi đăng nhập, hết hạn phiên | Identity/Auth, Session Management |
| SY-02 | Hướng dẫn thiết lập lần đầu và cấu hình API Key | BRS, StRS, OpsCon | Luồng onboarding, nhập và xác thực key | Extension UI, Backend API, Secret Handling |
| SY-03 | Quản lý API Key theo nhà cung cấp | StRS, OpsCon | Thêm, thay thế, kiểm tra và xử lý key lỗi | Credential Store, Provider Adapter |
| SY-04 | Chọn nhà cung cấp AI mặc định và đổi theo yêu cầu | StRS, OpsCon | Chọn provider, lỗi provider, chuyển provider | AI Provider Abstraction, Routing |
| SY-05 | Trích xuất nội dung trang web và thử hỗ trợ PDF/nội dung nhúng | StRS, OpsCon | Loại nội dung hỗ trợ, lỗi trích xuất, thông báo | Content Extraction, Browser Integration |
| SY-06 | Hỏi đáp trên đoạn văn bản được chọn | BRS, StRS, OpsCon | Đầu vào, đầu ra, ngữ cảnh và lỗi yêu cầu | Context Builder, AI Request Service |
| SY-07 | Hỏi đáp dựa trên trang hiện tại qua nhiều lượt | StRS, OpsCon | Quản lý ngữ cảnh hội thoại và lượt hỏi | Conversation Context Manager |
| SY-08 | Cách ly ngữ cảnh giữa các tab | OpsCon, SyRS | Quy tắc phân biệt tab và phiên đọc | Per-tab Context Store |
| SY-09 | Giải thích, dịch, tóm tắt và hỏi đáp nội dung | BRS, StRS, OpsCon | Hành vi từng tác vụ, giới hạn đầu vào và đầu ra | AI Orchestration, Prompt/Task Handlers |
| SY-10 | Tạo câu hỏi kiểm tra mức độ hiểu bài | BRS, StRS, OpsCon | Điều kiện kích hoạt, tạo câu hỏi, trả lời/bỏ qua | Comprehension Support |
| SY-11 | Hỗ trợ chủ động và gợi ý không gây gián đoạn | StRS, OpsCon | Điều kiện gợi ý, bật/tắt, điều chỉnh | Proactive Support Policy |
| SY-12 | Cá nhân hóa hỗ trợ đọc | BRS, StRS, OpsCon | Cấu hình cá nhân, giá trị mặc định, lưu và đồng bộ | User Preferences |
| SY-13 | Ưu tiên nguồn hiện tại và thể hiện sự không chắc chắn | BRS, StRS, OpsCon | Yêu cầu về căn cứ, thiếu bằng chứng và câu trả lời | Grounding/Prompt Policy, Response Validation |
| SY-14 | Lưu và tiếp tục hội thoại | BRS, StRS, OpsCon | Lưu, tải, tiếp tục và khôi phục hội thoại | Conversation Persistence |
| SY-15 | Đồng bộ dữ liệu theo tài khoản | StRS, OpsCon | Đồng bộ, lỗi mạng, dữ liệu cục bộ | Sync Service, Backend Persistence |
| SY-16 | Xử lý xung đột đồng bộ mà không ghi đè âm thầm | OpsCon, SyRS | Phát hiện xung đột, thông báo, lựa chọn người dùng | Conflict Detection/Resolution |
| SY-17 | Xóa một hội thoại hoặc toàn bộ lịch sử | StRS, OpsCon | Phạm vi xóa, xác nhận và thông báo giới hạn | History Management, Data Lifecycle |
| SY-18 | Khôi phục trạng thái khi mở lại extension | OpsCon, SyRS | Quy tắc khôi phục hội thoại và không khôi phục ngữ cảnh không hỗ trợ | Extension Session State |
| SY-19 | Cách ly dữ liệu khi chuyển tài khoản | OpsCon, SyRS | Xóa trạng thái làm việc của tài khoản trước | Account Session Isolation |
| SY-20 | Giữ yêu cầu chưa gửi để người dùng tự xử lý lại | OpsCon, SyRS | Lưu tạm, sửa, xóa, thử lại thủ công | Local Pending Request Store |
| SY-21 | Xử lý ngoại tuyến và dịch vụ không khả dụng | OpsCon, SyRS | Thông báo lỗi, giữ yêu cầu, điều kiện thử lại | Connectivity/Error Handling |
| SY-22 | Chế độ bảo vệ dữ liệu | BRS, StRS, OpsCon | Trạng thái mặc định, phạm vi áp dụng, quy tắc chặn | Protection Policy Engine |
| SY-23 | Chặn gửi nội dung nhạy cảm cho đến khi người dùng quyết định | StRS, OpsCon | Cảnh báo, xem/sửa nội dung, xác nhận, thử lại | Outbound Data Gate |
| SY-24 | Áp dụng chính sách bảo vệ với mọi nhà cung cấp | OpsCon, SyRS | Xác minh hành vi nhất quán khi đổi provider | Provider-independent Policy Layer |
| SY-25 | Phân biệt lỗi API Key với lỗi backend/nhà cung cấp | OpsCon, SyRS | Phân loại lỗi và hướng dẫn khắc phục | Error Classification, Provider Adapter |
| SY-26 | Không tự gửi lại yêu cầu sau lỗi hoặc khi có mạng trở lại | OpsCon, SyRS | Điều kiện thử lại và yêu cầu xác nhận thủ công | Retry Policy, Pending Request Handling |
| SY-27 | Theo dõi tình trạng dịch vụ và lỗi cơ bản | OpsCon, SyRS | Những sự kiện cần ghi nhận và cảnh báo | Logging, Basic Monitoring |
| SY-28 | Sao lưu và phục hồi dữ liệu | OpsCon, SyRS | Phạm vi sao lưu, khôi phục, thông báo sự cố | Backup/Recovery Design |
| SY-29 | Cung cấp hướng dẫn tự phục vụ và xử lý lỗi | OpsCon | Nội dung trợ giúp và trách nhiệm xử lý lỗi | Help Content, Operational Procedures |
| SY-30 | Thông báo rõ năng lực demo và hành vi mô phỏng | BRS, OpsCon | Trạng thái tính năng, lỗi và giới hạn demo | Feature Configuration, Demo/Test Support |

## **2.2. Danh mục truy vết phi chức năng và ràng buộc**

| ID tạm | Chủ đề | Nguồn cần đối chiếu | Đích SRS | Đích ADD |
| ----- | ----- | ----- | ----- | ----- |
| Q-01 | Bảo mật tài khoản, phiên và API Key | BRS, StRS, SyRS | Quy tắc bảo vệ thông tin xác thực | Authentication, Secret Storage, Access Control |
| Q-02 | Quyền riêng tư và giới hạn dữ liệu gửi ra ngoài | BRS, StRS, OpsCon | Quy tắc xử lý và gửi dữ liệu | Data Flow, Protection Boundary |
| Q-03 | Tính nhất quán của dữ liệu giữa thiết bị | StRS, OpsCon | Hành vi đồng bộ và lỗi đồng bộ | Sync Architecture |
| Q-04 | Khả năng sử dụng và khả năng tiếp cận | BRS, StRS | Yêu cầu giao diện, phản hồi, tùy chỉnh | Extension UI Architecture |
| Q-05 | Hiệu năng và độ trễ | BRS, StRS, SyRS | Chỉ tiêu có thể đo và điều kiện đo | Caching, Async Processing, Provider Calls |
| Q-06 | Độ tin cậy và khả năng phục hồi | OpsCon, SyRS | Hành vi khi lỗi, lưu dữ liệu, khôi phục | Failure Handling, Backup/Recovery |
| Q-07 | Khả năng mở rộng nhà cung cấp AI | BRS, SyRS | Hợp đồng giao tiếp và tương thích provider | Provider Adapter Interface |
| Q-08 | Khả năng kiểm thử và xác minh | StRS, SyRS | Tiêu chí chấp nhận và kịch bản kiểm thử | Testability, Observability |
| Q-09 | Ràng buộc nền tảng Chrome Extension | BRS, StRS, SyRS | Hành vi và giới hạn môi trường | Extension/Backend Interface |
| Q-10 | Giới hạn nguồn lực và phạm vi demo | BRS, OpsCon | Phân biệt bắt buộc, mô phỏng, hoãn lại | Deployment/Configuration Boundaries |

Các hàng trên là danh mục kiểm tra truy vết, không phải 40 yêu cầu đã được xác nhận nguyên văn. Khi hoàn thiện RTM, mỗi hàng cần có mã nguồn thật, trích dẫn hoặc tham chiếu mục nguồn, cùng trạng thái truy vết.

# **3\. Những điểm cần giải quyết trước khi khóa RTM**

1\. Chính sách bảo vệ dữ liệu

Cần đặc tả rõ trường hợp một yêu cầu có cả nội dung nhạy cảm và không nhạy cảm: mặc định chặn toàn bộ hay cho phép gửi phần đã được người dùng chủ động loại bỏ nội dung nhạy cảm. Cũng cần làm rõ nội dung nào được xem trước khi gửi và những dữ liệu nào bị loại khỏi chính sách này.

2\. Lưu trữ API Key

Cần chốt key được lưu ở đâu, có mã hóa khi lưu và truyền hay không, ai có thể truy cập, và cách xử lý khi đăng xuất hoặc xóa tài khoản. Đây là quyết định cần phản ánh trong ADD và tạo ra yêu cầu bảo mật có thể kiểm thử trong SRS.

3\. Lịch sử, đồng bộ và xóa dữ liệu

Chưa nên cam kết xóa khỏi bản sao lưu ngay lập tức nếu chính sách lưu trữ chưa được quyết định. Cần phân biệt xóa trong giao diện, xóa dữ liệu chính, dữ liệu đồng bộ và dữ liệu trong bản sao lưu.

4\. Chỉ tiêu định lượng

Chưa có ngưỡng được xác nhận cho độ trễ, tính sẵn sàng, thời gian khôi phục hay chất lượng AI. Không nên tự đặt các con số trong SRS; hãy để trạng thái “chưa xác định” và đưa thành quyết định cần phê duyệt.

5\. Phạm vi MVP và mô phỏng

Mỗi chức năng cần trạng thái riêng: `Required`, `Optional`, `Simulated`, `Deferred` hoặc `Out of Scope`. Điều này giúp ADD không mô tả một kiến trúc triển khai đầy đủ cho tính năng chỉ cần mô phỏng trong demo.

# **4\. Cách sử dụng RTM để tạo SRS và ADD**

* SRS: mỗi yêu cầu phần mềm có mã duy nhất, mô tả một hành vi hoặc ràng buộc kiểm chứng được, liên kết ngược về SyRS và các nguồn gốc, đồng thời liên kết tới test case.  
* ADD: mỗi quyết định kiến trúc cần có lý do, phương án được chọn, phương án thay thế nếu cần, hệ quả và các yêu cầu mà quyết định đó hỗ trợ.  
* RTM: bổ sung truy vết hai chiều để phát hiện yêu cầu nguồn chưa được triển khai, yêu cầu đích không có nguồn, và quyết định thiết kế không phục vụ yêu cầu nào.

Ví dụ, yêu cầu về chặn nội dung nhạy cảm sẽ có chuỗi:

`Nhu cầu bảo vệ dữ liệu (StRS) → Kịch bản chặn yêu cầu (OpsCon) → Yêu cầu chính sách bảo vệ (SyRS) → Quy tắc kiểm tra trước khi gửi (SRS) → Outbound Data Gate (ADD) → Test case về chặn, sửa và xác nhận gửi`.

# **5\. Bước tiếp theo để biến bản sơ bộ thành RTM chính thức**

Bản hiện tại giúp rà soát phạm vi, nhưng để thực sự rà soát lại bốn tài liệu, mình cần toàn văn BRS, StRS, OpsCon và SyRS. Bạn hãy tải lên các bản hiện tại; mình sẽ đối chiếu từng yêu cầu, giữ nguyên mã nguồn, đánh dấu truy vết thiếu/thừa và thay các mã nhóm tạm bằng mã yêu cầu chính thức. Sau đó mới có thể lập RTM đầy đủ để dùng trực tiếp cho SRS và ADD.

