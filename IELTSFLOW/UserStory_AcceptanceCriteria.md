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

