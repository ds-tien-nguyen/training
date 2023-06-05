
# Bài tập training PHP phần 1 - Tìm hiểu về Laravel 10

## Mô tả

Xây dựng trang web quản lý timesheet với các mô tả nghiệp vụ như sau:

1.1. Mỗi timesheet có các nội dung như sau:
- Ngày (ví dụ 2018-08-14)
- Các công việc đã làm trong ngày (multiple line). Mỗi line có dạng:
  - Task ID (nếu không có thì để là "N/A")
  - Nội dung task
  - Thời gian sử dụng
- Các khó khăn gặp phải (text)
- Các dự định sẽ làm trong ngày tiếp theo (text)

1.2. Mô tả use case của nhân viên
- Nhân viên có trang cá nhân, có thể sửa đổi các thông tin sau:
  - password, avatar, description
- Mỗi ngày nhân viên cần phải vào trang web để tạo timesheet
- Hệ thống sẽ tự động ghi nhận:
  - Số lần nhân viên đăng ký timesheet trong tháng (kể cả làm đúng giờ hay làm bổ sung)
  - Số lần mỗi nhân viên chậm làm timesheet theo tháng
- Sau khi tạo timesheet, nhân viên có quyền sửa nội dung timesheet
- Nhân viên có thể truy cập trang danh sách, liệt kê các timesheet của mình theo tuần / tháng.

## Yêu cầu

2.1. Đọc hiểu mô tả nghiệp vụ. Vẽ biểu đồ use case cơ bản.

2.2. Xây dựng cấu trúc database. Tạo file GoogleSheet theo mẫu.

https://docs.google.com/spreadsheets/d/1yo7DOcFnZ939w4OnVug2Pgaa1EVY7XquwHXn3pt_eZ8/edit?usp=sharing

2.3. Tạo code base cơ bản:
- PHP 8.1, MariaDB
- Laravel 10
- migrations
- layouts

2.4. Code chức năng nhân viên
- Màn hình nhân viên login
- Màn hình tạo timesheet
- Màn hình danh sách timesheet
- Màn hình view chi tiết timesheet
- Màn hình chỉnh sửa timesheet

2.5. Các chức năng nâng cao
- Chức năng phân quyền trong hệ thống (Yêu cầu tìm hiểu về Policy trong Laravel):
  - Tạo thêm field role trong table users, định nghĩa quyền cho các user trong hệ thống như sau 
    |  Roles | Function  |
    |---|---|
    | Admin  | Có thể duyệt timesheet của tất cả mọi người, có thể quản lý user, export timesheet  |
    | Manager  |  Chỉ duyệt được timesheet của những người thuộc quyền mình quản lý |
    | User  |  Chỉ tạo được timesheet và sửa được timesheet của bản thân mình |
  - Khi user truy cập vào các page mà mình ko có quyền truy cập thì xử lý lỗi 403
- Thêm màn hình hiển thị list time sheet bằng calendar  (Yêu cầu sử dụng 1 plugin js)
- Sử dụng bootstrap 5 để customize lại giao diện của hệ thống
