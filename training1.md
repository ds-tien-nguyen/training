
# Bài tập training PHP phần 1 - Tìm hiểu về Laravel 6

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

https://docs.google.com/spreadsheets/d/1lhyZvU-bJPku-5yVCI7dbVoVx4nq9F-TG71w7fsd4A8/edit?usp=sharing

2.3. Tạo code base cơ bản:
- PHP 7.4.x, MariaDB 10.1.x
- Laravel 6 LTS
- migrations
- layouts

2.4. Code chức năng nhân viên
- Màn hình nhân viên login
- Màn hình tạo timesheet
- Màn hình danh sách timesheet
- Màn hình view chi tiết timesheet
- Màn hình chỉnh sửa timesheet

2.5. Các chức năng nâng cao
- Chức năng phân quyền trong hệ thống:
 - Tạo thêm field role trong table users, định nghĩa quyền cho các user trong hệ thống như sau 
| Role      | Function |
| ----------- | ----------- |
| Admin      | Title       |
| Manager   | Text        |
| User   | Text        |
