# IELTSFlow – User Stories & Acceptance Criteria
**Dự án:** IELTSFlow – Nền tảng Luyện thi & Thi thử IELTS tích hợp AI
**Tác giả:** Nguyễn Thị Thanh Minh
---

# MÔ ĐUN 1: REGISTER (ĐĂNG KÝ TÀI KHOẢN)

---

# US-REG-001 - Đăng kí tài khoản mới bằng Email & Mật khẩu

## User story:
Là một Guest, tôi muốn đăng kí một tài khoản mới bằng email và mật khẩu để trở thành một Candidate và sử dụng những tính năng học và thi thử được website IELTSFlow cung cấp.

## BDD Format:

### Feature: Đăng kí tài khoản người dùng thông qua trình duyệt web.

### Background:
Given Người dùng truy cập trang đăng kí "/auth?tab=register"
And Hệ thống hiển thị form điền thông tin đăng kí gồm các trường: fullName, email, password, confirmPassword và ô checkbox điều khoản "regTerms"

/*Happy path*/
@smoke @regression
### Scenario: Đăng kí tài khoản thành công, tất cả thông tin đều hợp lệ
Given địa chỉ email 'Candidate@gmail.com' chưa từng được dùng để đăng kí trên hệ thống
When người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống gọi hàm "userService.registerUser" để lưu thông tin vào DB
And hệ thống gửi email chứa đường link xác thực tài khoản "/verify-email" qua dịch vụ mail
And Hệ thống chuyển tab hiển thị về "login"
And Hệ thống hiển thị thông báo thành công: "Đăng ký thành công! Vui lòng kiểm tra hộp thư email (kể cả mục Spam) để kích hoạt tài khoản."

/*Negative path*/
@regression
### Scenario: Đăng kí tài khoản thất bại do thiếu Họ và Tên
Given Người dùng không nhập trường "fullName"
When Người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please fill out this field" tại trường "fullName"

@regression
### Scenario: Đăng kí tài khoản thất bại do thiếu Email
Given Người dùng không nhập trường "email"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please fill out this field" tại trường "email"

@regression
### Scenario: Đăng kí tài khoản thất bại do sai cú pháp Email
Given Người dùng nhập trường "email" không hợp lệ là "candidate.gmail.com"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng kí tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please include an '@' in the email address. 'candidate.gmail.com' is missing an '@'." tại trường "email"

@regression
### Scenario: Đăng kí tài khoản thất bại do Email đã được đăng kí
Given Người dùng nhập trường "email" đã được đăng kí là "Candidate@gmail.com"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng kí tài khoản
And Hệ thống không lưu thông tin đăng kí
And Hệ thống chuyển về trang đăng nhập "/auth"
And Hệ thống hiển thị thông báo lỗi là "Email đã được sử dụng!"

@regression
### Scenario: Đăng kí tài khoản thất bại do thiếu Mật khẩu
Given Người dùng không nhập trường "password"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And Người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "confirmPassword" là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please fill out this field" tại trường "password"

@regression
### Scenario: Đăng kí tài khoản thất bại do sai cú pháp Mật khẩu
Given Người dùng nhập trường "password" không hợp lệ là "a1"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And Người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "confirmPassword" là "a1"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống không lưu thông tin đăng kí
And Hệ thống chuyển về trang đăng nhập "/auth"
And Hệ thống hiển thị thông báo lỗi là "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ cái và số"

@regression
### Scenario: Đăng kí tài khoản thất bại do không nhập lại mật khẩu để xác nhận
Given Người dùng không nhập trường "confirmPassword"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please fill out this field" tại trường "confirmPassword"

@regression
### Scenario: Đăng kí tài khoản thất bại do mật khẩu xác nhận không khớp với mật khẩu đã điền
Given Người dùng nhập trường "confirmPassword" là "abcd1111"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
When Người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And Người dùng chọn vào ô checkbox điều khoản "regTerms" để đồng ý các điều khoản
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống không lưu thông tin đăng kí
And Hệ thống chuyển về trang đăng nhập "/auth"
And Hệ thống hiển thị thông báo lỗi là "Mật khẩu xác nhận không khớp"

@regression
### Scenario: Đăng kí tài khoản thất bại do không chọn vào ô checkbox điều khoản
Given Người dùng không chọn vào ô checkbox điều khoản "regTerms"
When Người dùng nhập trường "fullName" là "Nguyễn Văn Can"
And người dùng nhập trường "email" là "Candidate@gmail.com"
And người dùng nhập trường "password" có ít nhất 8 kí tự là "Password123"
And người dùng nhập trường "confirmPassword" là "Password123"
And người dùng nhấn nút "Tạo tài khoản"
Then Hệ thống từ chối đăng ký tài khoản
And Hệ thống giữ nguyên hiển thị ở tab "register"
And Hệ thống hiển thị thông báo lỗi là "Please check this box if you want to proceed" tại ô checkbox điều khoản "regTerms"

## Rule-Based Format

### RULE-REG-001: Bắt buộc nhập liệu đầy đủ
Tất cả các trường `fullName`, `email`, `password`, `confirmPassword` bắt buộc phải nhập dữ liệu. Trình duyệt chặn gửi form nếu để trống bất kỳ trường nào.

### RULE-REG-002 - Định dạng Email
Địa chỉ Email phải khớp biểu thức chính quy (Regex): `^[A-Za-z0-9+_.-]+@(.+)$`

### RULE-REG-003 — Tính duy nhất của Email
Địa chỉ Email đăng ký không được trùng lặp với bất kỳ tài khoản nào đã tồn tại trong cơ sở dữ liệu.

### RULE-REG-004 - Kiểm tra độ mạnh Mật khẩu
Mật khẩu bắt buộc phải vượt qua bộ lọc kiểm tra độ mạnh `PasswordUtil.isPasswordStrong(password)`: có độ dài tối thiểu 8 ký tự, chứa cả chữ cái và chữ số.

### RULE-REG-005 — Khớp mật khẩu xác nhận
Mật khẩu xác nhận (`confirmPassword`) bắt buộc phải trùng khớp tuyệt đối với mật khẩu (`password`).

### RULE-REG-006 — Trạng thái mặc định của tài khoản mới
Tài khoản mới tạo sau khi gửi thông tin đăng ký sẽ có trạng thái mặc định là `Inactive` trong cơ sở dữ liệu. Trạng thái chỉ chuyển sang `Active` sau khi người dùng click vào link kích hoạt trong email xác thực gửi về.

### RULE-REG-007 — Bắt buộc đồng ý Điều khoản & Bảo mật
Checkbox đồng ý điều khoản được thiết lập thuộc tính bắt buộc tại tầng giao diện. Trình duyệt sẽ chặn việc gửi biểu mẫu đăng ký nếu người dùng chưa bấm chọn, đồng thời hiển thị thông báo lỗi `Please check this box if you want to proceed.`.

---

# US-REG-002 - Đăng kí tài khoản mới bằng tài khoản Google

## User story:
Là một Guest, tôi muốn đăng kí một tài khoản mới bằng tài khoản Google để trở thành một Candidate và sử dụng những tính năng học và thi thử được website IELTSFlow cung cấp.

## BDD Format:

### Feature: Đăng kí tài khoản người dùng bằng tài khoản Google

### Background:
Given Người dùng truy cập trang đăng kí "/auth?tab=register"
And Nút "Đăng kí bằng Google" đang hiển thị khả dụng

/*Happy path*/
@smoke @regression
### Scenario: Đăng kí tài khoản thành công bằng tài khoản Googile
Given Tài khoản Google có địa chỉ email là "Candidate@gmail.com" chưa từng được dùng để đăng kí trên hệ thống
When người dùng click vào nút "Đăng kí bằng Google"
And Người dùng xác thực tài khoản Google thành công trên cửa sổ ủy quyền của Google bằng cách nhập đúng email và password của tài khoản Google muốn đăng kí
Then Hệ thống tạo tự động một tài khoản mới sử dụng email của tài khoản Google vừa xác thực
And Hệ thống gán tên hiển thị và ảnh đại diện từ tài khoản Google vào tài khoản vừa Tạo
And Hệ thống lưu User vào Database với vai trò mặc định là "CANDIDATE" (RoleId: 3) và trạng thái là "Active"
And Hệ thống cấp session đăng nhập và điều hướng người dùng đến trang "/candidate/dashboard"

@regression
### Scenario: Đăng nhập Google thất bại với email đã đăng ký thủ công từ trước
Given Địa chỉ email Google "google.user@gmail.com" đã đăng ký tài khoản bằng Mật khẩu (AuthProvider: "Local")
When Người dùng click vào nút "Tiếp tục với Google"
And Xác thực tài khoản Google thành công
Then Hệ thống từ chối đăng nhập bằng Google
And Hệ thống giữ nguyên hiển thị trang "/auth" và báo lỗi: "Tài khoản email này đã được đăng ký thủ công. Vui lòng đăng nhập bằng mật khẩu."

## Rule-Based Format

### RULE-REG-08 — Ánh xạ luồng đăng nhập Google OAuth2
Luồng đăng nhập Google được tiếp nhận bởi `GoogleAuthServlet.java` tại URL pattern `/auth/google` (và `/api/auth/google`).

### RULE-REG-09 — Tự động liên kết hoặc tạo mới tài khoản Google
Nếu Email từ Google API chưa có trong hệ thống, tự động tạo mới User với trạng thái `Active` và Role `CANDIDATE`. Nếu Email đã tồn tại và có `authProvider = 'Google'`, tự động cấp session và cho phép đăng nhập. Trường hợp `authProvider = 'Local'`, hệ thống chặn và yêu cầu đăng nhập bằng mật khẩu.

---

# MÔ ĐUN 2: LOGIN (ĐĂNG NHẬP)

---

# US-LOG-001 — Đăng nhập hệ thống bằng Email và Mật khẩu

## User Story
Là một Người dùng đã đăng ký (Học viên / Mentor / Admin), tôi muốn đăng nhập bằng thông tin tài khoản của mình, để tôi có thể truy cập vào không gian làm việc phù hợp với vai trò của mình.

## BDD Format

### Feature: Xác thực đăng nhập bằng Email và Mật khẩu

### Background
Given Người dùng truy cập trang Xác thực tại đường dẫn "/auth"

@smoke @regression
### Scenario: Đăng nhập thành công với tài khoản Candidate
Given Tài khoản "candidate@gmail.com" có trạng thái "Active" và Role ID là "3" (Candidate)
When Người dùng nhập Email "candidate@gmail.com" và Mật khẩu chính xác
And Người dùng nhấn nút "Đăng nhập"
Then Hệ thống xác thực thông tin đăng nhập khớp
And Hệ thống lưu các thuộc tính "userId", "userEmail", "fullName", và "roleId" vào HttpSession
And Hệ thống chuyển hướng người dùng đến trang "/candidate/dashboard"

@smoke @regression
### Scenario: Đăng nhập thành công với tài khoản Admin
Given Tài khoản "admin@ieltsflow.com" có trạng thái "Active" và Role ID là "1" (Admin)
When Người dùng nhập đúng thông tin tài khoản và nhấn nút "Đăng nhập"
Then Hệ thống chuyển hướng người dùng đến trang quản trị "/admin/dashboard"

@smoke @regression
### Scenario: Đăng nhập thành công với tài khoản Mentor
Given Tài khoản "mentor@ieltsflow.com" có trạng thái "Active" và Role ID là "2" (Mentor)
When Người dùng nhập đúng thông tin tài khoản và nhấn nút "Đăng nhập"
Then Hệ thống chuyển hướng người dùng đến trang quản trị của giảng viên "/mentor/dashboard"

@regression
### Scenario: Đăng nhập thất bại do để trống trường Email
Given Người dùng để trống trường "email" là ""
When Người dùng nhập trường "password" là "Password123"
And Người dùng nhấn nút "Đăng nhập"
Then Trình duyệt chặn gửi form và hiển thị tooltip cảnh báo tại trường "email": "Please fill out this field."

@regression
### Scenario: Đăng nhập thất bại do Email không tồn tại
Given Email "unknown@gmail.com" không có trong cơ sở dữ liệu
When Người dùng nhập trường "email" là "unknown@gmail.com"
And Người dùng nhập trường "password" là "Password123"
And Người dùng nhấn nút "Đăng nhập"
Then Hệ thống từ chối đăng nhập và báo lỗi: "Tài khoản không tồn tại"

@regression
### Scenario: Đăng nhập thất bại do nhập sai mật khẩu
Given Tài khoản "candidate@gmail.com" tồn tại trong hệ thống
When Người dùng nhập trường "email" là "candidate@gmail.com"
And Người dùng nhập sai trường "password" là "WrongPass123"
And Người dùng nhấn nút "Đăng nhập"
Then Hệ thống từ chối đăng nhập và báo lỗi: "Sai mật khẩu"

@regression
### Scenario: Đăng nhập thất bại do tài khoản đã bị khóa (Banned)
Given Tài khoản "banned.user@gmail.com" có trạng thái là "Banned" trong cơ sở dữ liệu
When Người dùng nhập trường "email" là "banned.user@gmail.com" và mật khẩu hợp lệ
And Người dùng nhấn nút "Đăng nhập"
Then Hệ thống từ chối đăng nhập và báo lỗi: "Tài khoản của bạn đã bị khóa"

@regression
### Scenario: Đăng nhập thất bại do tài khoản chưa kích hoạt email (Inactive)
Given Tài khoản "inactive.user@gmail.com" có trạng thái là "Inactive" (chưa xác nhận qua email)
When Người dùng nhập trường "email" là "inactive.user@gmail.com" và mật khẩu hợp lệ
And Người dùng nhấn nút "Đăng nhập"
Then Hệ thống từ chối đăng nhập và báo lỗi: "Tài khoản đang bị khóa hoặc chưa được xác thực."

## Rule-Based Format

### RULE-LOG-01 — Bắt buộc nhập liệu trường Đăng nhập
Các trường `email` và `password` bắt buộc phải nhập dữ liệu (`required`). Trình duyệt sẽ chặn gửi form nếu để trống, hoặc phía máy chủ (`LoginServlet.java`) sẽ trả về thông báo lỗi `"Vui lòng nhập đầy đủ email và mật khẩu"`.

### RULE-LOG-02 — Kiểm tra chính xác thông tin đăng nhập
Hệ thống kiểm tra email bằng `userDAO.findByEmail(email)`. Nếu không tồn tại, trả về lỗi `"Tài khoản không tồn tại"`. Nếu tồn tại, kiểm tra mật khẩu bằng `PasswordUtil.checkPassword(password, user.getPasswordHash())`. Nếu sai mật khẩu, trả về lỗi `"Sai mật khẩu"`.

### RULE-LOG-03 — Kiểm tra ràng buộc trạng thái tài khoản khi đăng nhập
Tài khoản có trạng thái `"Banned"` khi đăng nhập sẽ lập tức bị hệ thống từ chối và báo lỗi `"Tài khoản của bạn đã bị khóa"`. Tài khoản có trạng thái `"Inactive"` (chưa kích hoạt email) sẽ báo lỗi `"Tài khoản đang bị khóa hoặc chưa được xác thực."`.

### RULE-LOG-04 — Ánh xạ chuyển hướng theo phân quyền (Role ID)
Sau khi xác thực thành công và lưu session, hệ thống tự động điều hướng người dùng dựa vào `roleId`:
- `roleId == 1` (Admin): Điều hướng đến `/admin/dashboard`
- `roleId == 2` (Mentor): Điều hướng đến `/mentor/dashboard`
- `roleId == 3` (Candidate): Điều hướng đến `/candidate/dashboard`

---

# US-LOG-002 — Khôi phục mật khẩu khi quên (Forgot Password)

## User Story
Là một Người dùng đã đăng ký, tôi muốn yêu cầu nhận email đặt lại mật khẩu khi quên, để tôi có thể khôi phục quyền truy cập vào tài khoản một cách an toàn.

## BDD Format

### Feature: Khôi phục mật khẩu tài khoản qua mã xác thực OTP

### Background
Given Người dùng đang ở trang Quên mật khẩu "/forgot-password"

@smoke @regression
### Scenario: Yêu cầu gửi mã OTP khôi phục thành công
Given Email "candidate@gmail.com" tồn tại hoạt động trong hệ thống
When Người dùng nhập Email "candidate@gmail.com"
And Người dùng chọn hành động gửi mã OTP ("action=sendOtp")
Then Hệ thống sinh mã OTP ngẫu nhiên, lưu tạm vào session và gửi về hòm thư người dùng
And Hệ thống đặt thuộc tính "step" là "verifyOtp" và chuyển hướng đến trang nhập OTP
And Hệ thống hiển thị thông báo: "Mã xác thực đã được gửi tới email của bạn."

@regression
### Scenario: Xác thực mã OTP thành công và chuyển sang bước tạo mật khẩu mới
Given Hệ thống đang ở bước nhập OTP và lưu email khôi phục trong session
When Người dùng nhập đúng mã OTP nhận được trong email
And Người dùng chọn hành động xác thực ("action=verifyOtp")
Then Hệ thống xác thực mã OTP khớp và tạo ra một mã "resetToken" lưu vào session
And Hệ thống đặt thuộc tính "step" là "resetPassword" và hiển thị form nhập mật khẩu mới

@regression
### Scenario: Đặt lại mật khẩu thành công
Given Hệ thống đang ở bước tạo mật khẩu mới và có "resetToken" trong session
When Người dùng nhập mật khẩu mới dài trên 8 ký tự là "NewPassword123"
And Nhập lại mật khẩu xác nhận khớp là "NewPassword123"
And Chọn hành động lưu mật khẩu ("action=resetPassword")
Then Hệ thống cập nhật mật khẩu mới đã mã hóa vào cơ sở dữ liệu của User
And Hệ thống xóa các thuộc tính nhạy cảm "resetEmail" và "resetToken" khỏi session
And Hệ thống chuyển hướng người dùng về trang Xác thực "/auth"
And Hệ thống hiển thị thông báo thành công: "Đổi mật khẩu thành công! Bạn có thể đăng nhập ngay bây giờ."

@regression
### Scenario: Yêu cầu gửi mã OTP thất bại do Email không tồn tại
Given Email "notfound@gmail.com" không có trong hệ thống
When Người dùng nhập Email "notfound@gmail.com" và chọn "Gửi mã OTP"
Then Hệ thống từ chối gửi mã và báo lỗi: "Email không tồn tại trong hệ thống!"

@regression
### Scenario: Xác thực mã OTP thất bại do nhập sai mã hoặc đã hết hạn
Given Hệ thống đang ở bước nhập OTP cho email khôi phục
When Người dùng nhập mã OTP sai "000000" và chọn "Xác thực OTP"
Then Hệ thống giữ nguyên ở bước nhập OTP và báo lỗi: "Mã OTP không chính xác hoặc đã hết hạn!"

@regression
### Scenario: Đặt lại mật khẩu thất bại do mật khẩu xác nhận không khớp
Given Hệ thống đang ở bước tạo mật khẩu mới và có "resetToken" hợp lệ
When Người dùng nhập mật khẩu mới là "NewPassword123" nhưng mật khẩu xác nhận là "WrongConfirm123"
And Chọn hành động lưu mật khẩu
Then Hệ thống từ chối cập nhật mật khẩu và hiển thị lỗi: "Mật khẩu xác nhận không khớp."

## Rule-Based Format

### RULE-LOG-05 — Thời hạn hiệu lực của mã OTP
Mã OTP khôi phục mật khẩu sinh ra được lưu trong HTTP Session và có thời hạn hiệu lực cố định (5 phút). Quá thời gian này, mã OTP tự động hết hạn và người dùng phải yêu cầu gửi lại.

### RULE-LOG-06 — Bảo mật quy trình đặt lại mật khẩu (Token Verification)
Bước đặt mật khẩu mới bắt buộc phải có `resetToken` hợp lệ trong session (được cấp sau khi xác thực OTP thành công). Mọi truy cập trực tiếp vào bước đổi mật khẩu mà không có token hợp lệ đều bị từ chối.

### RULE-LOG-07 — Kiểm tra độ mạnh và khớp mật khẩu mới
Trong bước tạo mật khẩu mới, mật khẩu phải thỏa mãn độ dài tối thiểu 8 ký tự, chứa cả chữ cái và chữ số. Trường hợp mật khẩu xác nhận không khớp, hệ thống ném ngoại lệ `"Mật khẩu xác nhận không khớp."`.

---

# MÔ ĐUN 3: BOOK MOCK TEST (ĐĂNG KÝ THI THỬ)

---

# US-MCK-001 — Xem danh sách đề thi thử IELTS có sẵn trên hệ thống

## User Story
Là một Học viên, tôi muốn xem danh sách các đề thi thử IELTS có sẵn, để tôi có thể duyệt và tìm kiếm đề thi phù hợp với nhu cầu luyện tập của mình.

## BDD Format

### Feature: Xem danh sách đề thi thử IELTS trên hệ thống

### Background
Given Học viên đã đăng nhập thành công vào hệ thống và có gói cước Candidate Pro đang hoạt động
And Học viên đang ở trang Danh sách thi thử "/candidate/mock-test"

@smoke @regression
### Scenario: Hiển thị danh sách đề thi thử thành công
When Trang web tải xong giao diện chọn đề thi thử (phương thức GET)
Then Hệ thống gọi `mockTestService.getAllMockTests()` hoặc `getAllPracticeTests()` (tuỳ theo `mode`) để lấy danh sách đề thi từ cơ sở dữ liệu
And Các thông tin hiển thị bao gồm: Tên đề thi (`exam.title`), Thời gian (`exam.duration`), Loại đề (`exam.type`), và Kỹ năng tập trung (`exam.skillFocus`)

@regression
### Scenario: Truy cập trang chọn đề thi thử thất bại khi chưa nâng cấp gói Candidate Pro
Given Học viên đã đăng nhập nhưng chưa đăng ký gói thành viên Pro (hoặc gói đã hết hạn)
When Học viên truy cập trang Danh sách thi thử "/candidate/mock-test"
Then Hệ thống từ chối truy cập và tự động chuyển hướng đến trang "/subscription?error=premium_required_mocktest"

## Rule-Based Format

### RULE-MCK-01 — Ánh xạ đường dẫn và Ràng buộc quyền truy cập (Pro Subscription Barrier)
Trang chọn đề thi được quản lý bởi `MockTestServlet.java` tại URL pattern `/candidate/mock-test`. Trước khi tải dữ liệu, hệ thống kiểm tra `subService.getActiveSubscriptionByUserId(userId) != null`. Nếu học viên chưa có gói Pro, lập tức chuyển hướng về trang yêu cầu nâng cấp (`/subscription?error=premium_required_mocktest`).

### RULE-MCK-02 — Truy xuất danh sách đề thi
Trang GET `/candidate/mock-test` gọi `mockTestService.getAllMockTests()` hoặc `mockTestService.getAllPracticeTests()` để hiển thị toàn bộ các đề thi hiện có cho học viên lựa chọn. Hàm `getRandomMockTest()` chỉ được kích hoạt khi học viên nhấn bắt đầu thi (`POST /candidate/mock-test?action=start`) mà không chọn mã đề thi cụ thể.

---

# US-MCK-002 — Bắt đầu làm bài thi thử và Tải đề thi vào phòng thi

## User Story
Là một Học viên, tôi muốn bắt đầu đề thi thử đã chọn, để tôi có thể vào phòng thi trực tuyến và hiển thị đầy đủ các phần thi cũng như câu hỏi.

## BDD Format

### Feature: Bắt đầu ca thi thử và truy cập phòng thi trực tuyến

### Background
Given Học viên đã đăng nhập, có gói cước Pro hoạt động và đang ở trang chọn đề thi "/candidate/mock-test"

@smoke @regression
### Scenario: Bắt đầu ca thi thử thành công (Happy Path)
Given Hệ thống có sẵn đề thi hợp lệ trong cơ sở dữ liệu
When Học viên bấm nút "Bắt đầu thi ngay" (gửi yêu cầu POST "/candidate/mock-test" kèm tham số hidden "action=start")
Then Hệ thống gọi `mockTestService.createSubmission(userId, examId)` để tạo một bản ghi bài làm mới trong bảng `TestSubmission`
And Hệ thống tải danh sách câu hỏi ("questions") và các phần thi ("sections") của đề thi vào session
And Hệ thống lưu thời gian bắt đầu làm bài "mt_examStartTime" dưới dạng timestamp mili-giây vào session
And Hệ thống điều hướng học viên đến giao diện phòng thi "/candidate/mock-test?action=take"

@regression
### Scenario: Bắt đầu ca thi thử thất bại khi chưa có đề thi nào khả dụng
Given Ngân hàng đề thi thử hiện chưa có đề thi nào khả dụng (`getRandomMockTest() == null`)
When Học viên bấm nút "Bắt đầu thi ngay" (POST "/candidate/mock-test?action=start")
Then Hệ thống từ chối khởi tạo phòng thi
And Hệ thống giữ nguyên học viên ở trang "/candidate/mock-test"
And Hệ thống hiển thị thông báo lỗi: "Hiện tại chưa có đề thi nào. Vui lòng thử lại sau."

## Rule-Based Format

### RULE-MCK-03 — Kiểm tra sự tồn tại của đề thi trước khi bắt đầu
Khi học viên gửi yêu cầu `POST /candidate/mock-test?action=start`, hệ thống kiểm tra đề thi hiện hành thông qua `mockTestService.getRandomMockTest()`. Nếu trả về `null`, hệ thống từ chối khởi tạo phòng thi và hiển thị lỗi `"Hiện tại chưa có đề thi nào. Vui lòng thử lại sau."`.

### RULE-MCK-04 — Duy trì phiên làm bài thi trong HTTP Session
Trong suốt thời gian ca thi diễn ra, các thông tin `mt_currentExam`, `mt_currentQuestions`, `mt_currentSections` và `mt_currentSubmissionId` phải được duy trì trong HTTP Session để hỗ trợ khôi phục bài thi nếu học viên vô tình load lại trang.

---

# US-MCK-003 — Làm bài, phát hiện vi phạm quy chế thi (Anti-Cheat) và nộp bài chấm điểm tự động bằng AI

## User Story
Là một Học viên, tôi muốn ghi câu trả lời, thu âm file nói và nộp bài thi thử, để hệ thống tự động chấm điểm Listening/Reading và kích hoạt AI đánh giá chi tiết Writing/Speaking cho tôi.

## BDD Format

### Feature: Nộp bài thi thử, giám sát gian lận và chấm điểm tự động bằng AI

### Background
Given Học viên đang ở giao diện phòng thi "/candidate/mock-test?action=take"
And Session đang lưu thông tin bài làm "mt_currentSubmissionId" hợp lệ

@regression
### Scenario: Ghi nhận vi phạm quy chế thi khi học viên chuyển tab
Given Hệ thống đang bật tính năng giám sát chống gian lận
When Học viên thực hiện chuyển tab trình duyệt hoặc thoát toàn màn hình (kích hoạt sự kiện blur/focus)
And Hệ thống gửi request AJAX thông báo vi phạm ("action=violation") lên server
Then Hệ thống tăng số lần vi phạm trong cơ sở dữ liệu cho ca thi
And Nếu số lần vi phạm vượt mức tối đa "maxViolations", hệ thống tự động nộp bài thi ngay lập tức

@smoke @regression
### Scenario: Nộp bài thi thành công và kích hoạt AI chấm điểm tự động
Given Học viên hoàn thành bài thi và nhấn nút "Nộp bài" (hoặc hết giờ làm bài hệ thống tự trigger "action=submit")
When Hệ thống tiến hành thu thập toàn bộ đáp án của học viên trong form
And Hệ thống tự động tính điểm Reading và Listening dựa trên đáp án đúng lưu trong Database
And Hệ thống gửi nội dung bài làm Writing lên Gemini API để đánh giá chi tiết
And Hệ thống gửi tệp ghi âm Speaking lên Azure Speech API và Gemini API để phân tích phát âm, ngữ pháp và tính điểm
Then Hệ thống lưu kết quả điểm tổng quát (Overall Band) và nhận xét chi tiết vào Database
And Hệ thống xóa sạch các thông tin bài làm tạm thời trong session
And Hệ thống điều hướng học viên sang trang kết quả "/candidate/mock-test?action=result&submissionId=X" để xem bảng điểm chi tiết

## Rule-Based Format

### RULE-MCK-05 — Giới hạn vi phạm quy chế thi (Anti-Cheat Threshold Policy)
Hệ thống giám sát sự kiện rời tab trình duyệt hoặc thoát toàn màn hình. Mỗi lần vi phạm sẽ được cộng dồn trong cơ sở dữ liệu. Nếu vượt ngưỡng cho phép (`violations >= 3`), hệ thống tự động đóng ca thi và thu bài chấm điểm ngay lập tức.

### RULE-MCK-06 — Quy định định dạng tệp âm thanh phần thi Speaking
Tệp ghi âm Speaking gửi lên từ trình duyệt phải có định dạng audio hợp lệ (`.wav`, `.webm`, `.mp3`) và dung lượng không vượt quá giới hạn 20MB, được lưu tạm tại thư mục upload cấu hình bởi `FileUploadServlet.java` trước khi gửi tới Azure Speech Service.

---

# MÔ ĐUN 4: SUBSCRIPTION (GÓI CƯỚC THÀNH VIÊN)

---

# US-SUB-001 — Xem danh sách các gói nâng cấp Candidate Pro

## User Story
Là một Học viên, tôi muốn xem và so sánh quyền lợi giữa các gói cước nâng cấp Candidate Pro, để tôi hiểu rõ giá trị nhận được trước khi quyết định mua.

## BDD Format

### Feature: Xem bảng giá và quyền lợi các gói cước Candidate Pro

### Background
Given Học viên đã đăng nhập và đang ở trang Dashboard "/candidate/dashboard"

@smoke @regression
### Scenario: Xem bảng giá các gói cước thành công
When Học viên click chọn mục "Nâng cấp Pro" (URL "/subscription")
Then Hệ thống truy vấn danh sách các gói cước đang hoạt động ("Active") từ bảng `SubscriptionPackage`
And Hệ thống hiển thị giao diện chứa danh sách các gói kèm thông tin: Tên gói, Giá tiền, Thời hạn sử dụng (Tháng) và mô tả quyền lợi nâng cấp.

## Rule-Based Format

### RULE-SUB-01 — Ánh xạ đường dẫn danh sách gói cước thành viên
Yêu cầu truy vấn và hiển thị danh sách gói cước được đảm nhiệm bởi `SubscriptionController.java` tại URL pattern `/subscription` thông qua hàm `getActivePackagesPaginated`.

### RULE-SUB-02 — Lọc trạng thái gói cước hiển thị
Hệ thống chỉ hiển thị các gói cước đang hoạt động (`status = 'Active'` và `deleted = false` hoặc `null`). Các gói bị ẩn hoặc đã xóa không được phép xuất hiện trên giao diện.

---

# US-SUB-002 — Chọn gói nâng cấp và khởi tạo giao dịch thanh toán

## User Story
Là một Học viên, tôi muốn chọn một gói cước nâng cấp và tạo đơn hàng, để tôi có thể chuyển sang trang thanh toán mã QR an toàn.

## BDD Format

### Feature: Chọn gói cước nâng cấp và khởi tạo đơn hàng

### Background
Given Học viên đang ở trang chọn gói cước "/subscription"

@smoke @regression
### Scenario: Khởi tạo giao dịch thành công (Happy Path)
Given Gói cước "Candidate Pro 6 Tháng" có ID là "2" và giá tiền "599000" VNĐ đang khả dụng
When Học viên chọn Gói cước này và bấm nút "Đăng ký mua"
Then Hệ thống gửi yêu cầu HTTP POST lên Servlet "/checkout" kèm tham số "packageId=2"
And Hệ thống tạo mới một đối tượng "Transaction" lưu vào Database với trạng thái ban đầu là "Pending" và phương thức thanh toán mặc định là "VietQR - SePay"
And Hệ thống chuyển hướng trình duyệt của học viên đến URL "/checkout?id=Transaction_ID" để thực hiện thanh toán

## Rule-Based Format

### RULE-SUB-03 — Kiểm tra tính hợp lệ của gói cước khi khởi tạo giao dịch
Giao dịch chỉ được khởi tạo thành công khi gói cước tương ứng đang hoạt động hợp lệ (`deleted = false` hoặc `null`). Nếu gói cước không tồn tại hoặc đã ngừng bán, hệ thống từ chối tạo đơn và điều hướng về trang `/subscription`.

### RULE-SUB-04 — Quy định khởi tạo giao dịch mới
Giao dịch mới khởi tạo luôn có trạng thái mặc định là `Pending` (Đang chờ thanh toán), đồng thời ghi nhận mốc thời gian tạo đơn (`createdAt`) chính xác theo máy chủ hệ thống.

---

# MÔ ĐUN 5: PAYMENT (THANH TOÁN QUA SEPAY)

---

# US-PAY-001 — Giao diện hiển thị mã QR thanh toán động qua SePay

## User Story
Là một Học viên, tôi muốn hệ thống hiển thị mã VietQR động trên trang thanh toán, để tôi quét mã và chuyển khoản ngân hàng nhanh chóng, chính xác qua ứng dụng ngân hàng di động.

## BDD Format

### Feature: Tạo và hiển thị mã QR thanh toán động qua cổng SePay

### Background
Given Học viên đang truy cập trang "/checkout" với tham số "id" giao dịch hợp lệ
And Giao dịch tương ứng đang có trạng thái là "Pending" trong cơ sở dữ liệu

@smoke @regression
### Scenario: Hiển thị giao diện Checkout kèm QR động thành công
When Trang checkout được tải hoàn tất
Then Hệ thống gọi lớp tiện ích "SePayUtil.generateQRUrl" để tạo đường link mã VietQR động
And Mã QR này tự động tích hợp thông tin: số tài khoản ngân hàng thụ hưởng, chủ tài khoản, số tiền giao dịch chính xác và nội dung chuyển khoản theo cú pháp
And Hệ thống render giao diện hiển thị ảnh mã QR, số tiền cần chuyển, cú pháp nội dung chuyển khoản chi tiết và đồng hồ đếm ngược chờ giao dịch.

## Rule-Based Format

### RULE-PAY-01 — Cấu hình thông số tài khoản ngân hàng thụ hưởng
Các thông số tài khoản ngân hàng thụ hưởng được cấu hình thông qua biến hệ thống: `SEPAY_BANK_ACC`, `SEPAY_BANK_NAME` và `SEPAY_BANK_ACCOUNT_NAME`.

### RULE-PAY-02 — Cấu trúc cú pháp nội dung chuyển khoản mã VietQR
Nội dung chuyển khoản nhúng trong mã VietQR bắt buộc tuân theo cú pháp `IF{Transaction_ID}` (ví dụ: `IF102`) để cổng thanh toán tự động đối soát đơn hàng khi nhận Webhook.

---

# US-PAY-002 — Webhook SePay xác nhận thanh toán thành công và tự động kích hoạt gói Pro

## User Story
Là một Học viên, tôi muốn hệ thống tự động nhận diện giao dịch chuyển khoản thành công và kích hoạt gói Pro cho tôi ngay lập tức, để tôi không cần gửi biên lai thủ công hoặc chờ người duyệt.

## BDD Format

### Feature: Xử lý Webhook thanh toán thành công và tự động kích hoạt gói Pro

### Background
Given Giao dịch ID "102" đang có trạng thái "Pending" với số tiền yêu cầu thanh toán là "599000" VNĐ
And Giao dịch tương ứng với tài khoản học viên có ID "12"

@smoke @regression
### Scenario: Nhận Webhook chuyển khoản thành công và nâng cấp tài khoản học viên (Happy Path)
Given Học viên đã thực hiện chuyển tiền thành công qua mã QR với nội dung giao dịch chứa cú pháp "IF102"
When Cổng thanh toán SePay gửi một HTTP POST request đến API Webhook "/webhook/sepay" của hệ thống IELTSFlow
And Dữ liệu JSON gửi lên chứa thông tin: "transferType: in", "code: IF102", và số tiền nhận được "transferAmount: 599000"
And Hệ thống kiểm tra mã giao dịch cổng thanh toán "gatewayTxId" chưa từng được xử lý trước đó (chống trùng lặp giao dịch)
Then Hệ thống cập nhật trạng thái giao dịch "102" thành "Success"
And Hệ thống gọi "SubscriptionService.processSuccessfulTransaction" để nâng cấp quyền học viên
And Hệ thống tự động tạo mới hoặc gia hạn hạn dùng gói Pro ("UserSubscription") thêm thời hạn tương đương số tháng của gói cước vào Database
And Giao diện của học viên tự động cập nhật hiển thị trạng thái đã thanh toán thành công và mở khóa toàn bộ đặc quyền Pro

## Rule-Based Format

### RULE-PAY-03 — Kiểm tra chống xử lý lặp giao dịch Webhook (Idempotency Check)
Hệ thống bắt buộc phải kiểm tra thông qua hàm `transactionService.isGatewayTransactionProcessed(gatewayTxId)`. Nếu mã giao dịch từ cổng thanh toán đã được xử lý trước đó, hệ thống trả về HTTP 200 OK ngay lập tức để ngăn chặn gia hạn lặp lại.

### RULE-PAY-04 — Quy định cộng dồn thời hạn gói Pro khi nâng cấp thành công
Nếu học viên chưa có gói Pro hoặc gói cũ đã hết hạn, thời hạn gói mới bắt đầu tính từ thời điểm thanh toán thành công. Nếu học viên đang có gói Pro còn hiệu lực, thời hạn sử dụng mới sẽ được cộng dồn tiếp nối vào ngày hết hạn hiện tại.

---

# US-PAY-003 — Xử lý lỗi Webhook khi thanh toán thiếu tiền hoặc sai nội dung chuyển khoản

## User Story
Là một Quản trị viên hệ thống, tôi muốn hệ thống xử lý thông minh các giao dịch lỗi (chuyển thiếu tiền hoặc sai cú pháp), để phát hiện sai lệch tài chính và chặn các lượt nâng cấp tài khoản không hợp lệ.

## BDD Format

### Feature: Xử lý ngoại lệ khi thanh toán thiếu tiền hoặc sai cú pháp chuyển khoản

### Background
Given Giao dịch ID "102" đang có trạng thái "Pending" trong cơ sở dữ liệu với số tiền yêu cầu thanh toán là "599000" VNĐ

@regression
### Scenario: Học viên chuyển khoản thiếu tiền so với yêu cầu đơn hàng (Underpayment)
When Cổng thanh toán SePay gửi request Webhook xác nhận giao dịch "IF102" nhưng số tiền "transferAmount" thực tế chỉ là "500000" VNĐ
Then Hệ thống cập nhật trạng thái giao dịch "102" thành "Failed"
And Hệ thống KHÔNG nâng cấp tài khoản của học viên lên Pro
And Hệ thống gọi "systemLogDAO.createSystemLog" ghi lại nhật ký lỗi nghiệp vụ dạng "Invalid Transaction" với nội dung "Underpaid transaction 102. Expected: 599000, Got: 500000"

@regression
### Scenario: Chuyển khoản sai cú pháp nội dung dẫn tới không tìm thấy đơn hàng
When Cổng thanh toán SePay gửi request Webhook với nội dung chuyển khoản không chứa mã đơn dạng "IF" hợp lệ
Then Hệ thống không thể tìm ra giao dịch tương ứng trong cơ sở dữ liệu
And Hệ thống từ chối cập nhật trạng thái đơn hàng
And Hệ thống tạo một bản ghi log nghiệp vụ lỗi "Invalid Transaction" trong bảng "SystemLog" với thông tin chi tiết để Admin đối soát thủ công

## Rule-Based Format

### RULE-PAY-05 — Chính sách xử lý chuyển khoản thiếu tiền (Underpayment Policy)
Trường hợp số tiền chuyển khoản thực tế nhỏ hơn số tiền yêu cầu của đơn hàng (`transferAmount < t.getAmount()`), hệ thống chuyển trạng thái giao dịch sang `Failed`, từ chối kích hoạt gói Pro và ghi nhận nhật ký lỗi tài chính vào `SystemLog`.

### RULE-PAY-06 — Chính sách xử lý chuyển khoản dư tiền (Overpayment Policy)
Trường hợp số tiền chuyển khoản thực tế lớn hơn hoặc bằng số tiền yêu cầu (`transferAmount >= t.getAmount()`), hệ thống vẫn cập nhật giao dịch thành `Success` và kích hoạt gói Pro bình thường cho học viên. Đồng thời, ghi nhận một log thông tin `"Overpaid transaction"` để theo dõi chênh lệch số dư thừa.

