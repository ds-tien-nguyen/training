# Review Code

## Quan điểm chung

- **Code phải hoạt động**
- Code phải dễ đọc hiểu (Sách tham khảo : The art of readable code)

    _Bad code_
    ```php
    function getUser() {}
    function getUser2() {}
    function getUser2() {}
    ```
    _Good code_
    ```php
    function getUser() {}
    function getUserById() {}
    function getAdminUsers() {}
    ```
- Phải tuân thủ theo coding convention (e.g: [PSR-12](https://www.php-fig.org/psr/psr-12/))
- Đặt tên phải ngắn gọn dễ hiểu
- Đặt tên phải đúng chính tả
- Không được phép sử dụng [magic numbers](http://c2.com/cgi/wiki?MagicNumber)
  
    _Bad code_
    ```php
    $total = 1.08 * $price;
    ```
    _Good code_
    ```php
    const TAX_RATE = 1.08;
	$total = TAX_RATE * $price;
    ```
- Tất cả các biến phải được sử dụng ở phạm vi nhỏ nhất có thể
- Không được có các phần code bị comment out
- Không có các đoạn code không chạy, các đoạn code không sử dụng đến
- Không  viết các đoạn code mà đã có sẵn trong các lib
  
    _Bad code_
    ```php
    $startDate = strtotime('2022-10-14');
    $endDate = strtotime('2022-10-20');
    
    $dateDiff = $endDate - $startDate;
    echo round($dateDiff / (60 * 60 * 24));
    ```
    _Good code_
    ```php
    $startDate = Carbon::parse('2022-10-14');
    $endDate = Carbon::parse('2022-10-20');
    
    echo $endDate->diffInDays($startDate);
    ```
- Code không được lặp lại
- Không sử dụng các expression dài dòng và khó hiểu
  
    _Bad code_
    ```php
    $a =  (true == true ? 'A' : 'B') ? 'C' : 'D'; 
    ```
- Không đặt tên biến có chứa các từ ngữ phủ định gây khó hiểu như no, not...
  
    _Bad code_
    ```php
    $hasNoValues = true;
    
    if ($hasNoValue) {
        doSomething()
    }
    ```
    _Good code_
    ```php
    $hasValues = false;
    
    if (!$hasValues) {
        doSomething()
    }
    ```
- Không có các block code rỗng
- Sử dụng các cấu trúc chuẩn của language (e.g: Array, List...)
- Sử dụng catch để bắt các exception cụ thể
- Không sử dụng == và === lẫn với nhau trong cùng 1 đoạn code xử lý
- Các vòng lặp phải hữu hạn và có điều kiện kết thúc rõ ràng
- Các vòng lặp lồng nhau phải viết cụ thể điều kiện kết thúc của từng vòng lặp
- Blocks code bên trong vòng lặp phải đơn giản nhất có thể
- Performance luôn phải được ưu tiên

## Architecture
- Phải sử dụng chính xác theo design pattern của framework (e.g: Service Container, Service Provider, Dependency Injection trong Laravel)
- Một class chỉ nên phục vụ 1 mục đích duy nhất (Single responsibility principle) 
- Classes, modules, functions, etc. cần mở cho sự mở rộng và đóng cho sự thay đổi (Open–closed principle)
- Các object trong chương trình có thể được thay thế bằng các đối tượng con của nó nhưng không làm thay đổi hoạt động của chương trình (Liskov substitution principle)
- Nhiều các interface riêng biệt sẽ tốt hơn sử dụng chung 1 interface (Interface segregation principle)
- Phụ thuộc vào các abtract class chứ không phụ thuộc vào một đối tượng cụ thể (Dependency inversion principle)

## API
- APIs phải check giá trị đầu vào
- API phải có xác thực và phân quyền rõ ràng
- Các API thay đổi phải được phản ánh ngay lập tức vào tài liệu API
- APIs phải trả về status chính xác trong response

## Logging
- Log phải dễ đọc và debug
- Trong các trường hợp lỗi bắt buộc phải ghi log
- Không sử dụng print_r, var_dump hoặc các function tương tự trong code
- Không được in ra các stack traces

## Documentation
- Comments phải thể hiện được mục đích của đoạn code
- Tất cả các method nên có comment rõ ràng
- Tất cả các methods/interfaces/contracts public cần được comment đúng format
- Tất cả các case rẽ nhánh đều nên có comment
- Tất cả các phần xử lý bất thường đều nên có comment

## Security
- Tất cả các input đầu vào đều cần được check security (e.g XSS)
- Cần xử lý các tham số ngoại lệ để tránh code phát sinh lỗi
- Không có thông tin nhạy cảm nào được ghi lại hoặc hiển thị trong stacktrace
