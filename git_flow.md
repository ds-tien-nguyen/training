# Git flow

## 1, Tổng quan

Tài liệu này mô tả các vấn đề:

- Mô hình branch
- Trình tự thực hiện một task
- Các lệnh git thường gặp

## 2, Mô hình branch

![gitflow](https://gist.githubusercontent.com/tuanle/26e8e3c7e714b45112c1bfeb797a62cb/raw/e8ef2f15271296f25f70aacdd7fcfb2b7498426d/03.gitflow.jpg)

- Một branch được coi như một "phiên bản" của code base, nhằm phục vụ cho một mục đích nào đó
- Mỗi branch gồm nhiều commit, mỗi commit là một "lần sửa đổi" code base.

Các branch được chia thành hai loại như sau:

### 2.1, Branch chính

- Là những branch tồn tại trong suốt quá trình phát triển (lifetime).
- Protected, không nên thực hiện các hành động sau:
	- Xóa branch
	- Rebase, cherry-pick, hoặc bất cứ hành động thay đổi cấu trúc tree commit

Các branch chính bao gồm:

#### 2.1.1, master

- Là branch chứa code hoàn thiện nhất (sau khi code, review, test) và có thể bàn giao hoặc deploy lên production server
- Thường được deploy trên môi trường production.

#### 2.1.2, develop

- Là branch chứa code mới nhất trong quá trình phát triển (sau khi code, review), đang đợi test và fixbug.
- Thường được deploy trên môi trường testing.

### 2.2, Branch phụ

- Là những branch chỉ tồn tại trong một giai đoạn nào đó của quá trình phát triển (limited)
- Vòng đời của một branch phụ:
	- Tách nhánh (checkout) từ một trong các branch chính
	- Chỉnh sửa code
	- Hợp nhất (merge) với một hoặc cả hai branch chính
	- Xóa branch

Các branch phụ có thể bao gồm:

#### 2.2.1, feature

- Dùng để phát triển tính năng mới hoặc fixbug trong quá trình phát triển.
- Checkout từ `develop`. Merge vào `develop`
- Tùy theo chức năng, có thể đặt tên với tiền tố `feature-`, `enhancement-`, `bug-` hoặc đơn giản là chỉ dùng thống nhất tiền tố `feature-`
- Các task không có redmine issue, nên được đặt với tiền tố `support-`
- Quy ước đặt tên:

```
feature-{redmine-issue-id}-mô-tả-ngắn-gọn
```

Ví dụ với task phát triển cho issue 20001, sẽ được đặt tên là:

```
feature-20001-create-user-page
```

#### 2.2.2, release

- Dùng để chuyển giao code từ branch `develop` sang branch `master`
- Checkout từ `develop`. Merge vào cả `master` và `develop`
- Trong trường hợp code release có bug, có thể tạm tách branch `release-fix-` từ branch `release-`, fixbug và merge vào branch `release-`. Trong trường hợp này bắt buộc phải merge lại branch `release-` vào branch `develop-`
- Trong trường hợp code release không có bug, thông thường sẽ không cần phải merge lại branch `release-` vào branch `develop-`
- Quy ước đặt tên:

```
release-X.X
```

Với X.X là phiên bản (version) của application tại thời điểm release, ví dụ với phiên bản là 0.1, sẽ được đặt tên là:

```
release-0.1
```

#### 2.2.3, hotfix

- Dùng để fixbug phát sinh trên môi trường production sau khi đã release.
- Checkout từ `master`. Merge vào cả `master` và `develop`
- Quy ước đặt tên:

```
hotfix-X.X.Y
```

Với X.X là phiên bản (version) vừa release, Y là thứ tự bản hotfix, ví dụ với phiên bản là 0.1, bản fix thứ nhất, sẽ được đặt tên là:

```
hotfix-0.1.1
```

### 2.3, Hệ thống tag

- Nhằm đánh dấu các version của application, xuất `changelog` khi có yêu cầu
- Các thời điểm cần đánh dấu tag (lightweight tag):
    - Bắt đầu dự án

    ```
    git tag v0.1
    ```

    - Ngay trước khi merge code release

    ```
    git tag v0.2
    ```

    - Ngay trước khi merge code hotfix

    ```
    git tag v0.2.1
    ```

- Xuất `changelog`

```
git log --no-merges v0.2...v0.1
```

## 3, Trình tự thực hiện task

### 3.1, Yêu cầu chung

- Một task cần phải được thực hiện trên một branch phụ, không được thực hiện trực tiếp trên branch chính
- Một task trước khi merge vào branch chính cần được
	- Merge code mới nhất từ branch chính đã checkout
	- Tối thiểu hóa số commit phát sinh, tốt nhất nên là 1 commit trên 1 branch phụ
	- Viết commit message theo đúng quy ước
	- Tạo merge request
	- Review code

### 3.2, Trình tự thực hiện

Giả sử một developer cần làm một task được giao bởi redmine issue 20001, các bước mà anh ta cần thực hiện như sau:

#### 3.2.1, Tách nhánh

- Chuyển branch hiện tại sang `develop`
  ```
  git checkout develop
  ```
- Lấy code mới nhất từ remote
	```
    git pull origin `develop`
    ```
- Tách sang nhánh mới
	```
    git checkout -b feature-20001
    ```

#### 3.2.2, Code
- ....

#### 3.2.3, Tạo commit

- Xem trạng thái hiện tại trên branch
	```
    git status
    ```
- Add/Update/Remove các file code thay đổi
	```
    git add file.php
    git add -u file2.php
    git rm file3.php
    ```
    Lưu ý: Có thể add folder để tăng tốc dộ, tuy nhiên không nên sử dụng lệnh `git add .`

- Kiểm tra lại lần cuối trạng thái staged
	```
    git status
    ```
- Tạo commit
	```
    git commit
    ```

    Lưu ý: Không nên sử dụng lệnh `git commit -m` để tránh việc viết commit message oneline.

    Quy ước về viết commit message:

    - Bao gồm hai phần: subject (%s) và body (%b)
    - Subject không quá 50 kí tự
    - Body markdown. bắt buộc có tag `Resolves`
    - Mỗi phần tách nhau bởi một dòng trắng
    - Ví dụ:
    	```txt
        Init code for login function (L001 page)
        {BLANK LINE}
        - Add login route, controller, view
        - Add AuthService, SmsNotification, CreatedUserEvent
        - Resolves #20001
        {BLANK LINE}
        ```
#### 3.2.4, (Optional) Gộp commit (Áp dụng nếu đã commit >= 2 commit trên branch)

- Kiểm tra số lượng commit từ commit tree:

```
git log --oneline -10
```

- Nếu có 2 commit trở lên tính từ merge request gần nhất thì cần phải gộp các commit này với nhau:

```
git rebase -i HEAD~2
use f (fixup) option
```

(Chú ý: `HEAD~2` là ví dụ với trường hợp cần gộp 2 commit, trường hợp cần gộp 3 commit là `HEAD~3`, tương tự `HEAD~4`, ...)

#### 3.2.5, Kiểm tra và cập nhật thay đổi mới nhất có thể phát sinh

- Cập nhật git reference
	```
    git fetch origin
    ```
- Trong trường hợp nhánh develop đã có sự thay đổi, cần cập nhật
	```
    git pull --rebase origin develop
    ```
- Sửa conflict

#### 3.2.5, Tạo merge request

- Push code
	```
    git push origin feauture-20001
    ```
- Tạo merge request với nhánh develop trên Github, Gitlab, ...
- Assign merge request cho người review code

#### 3.2.6, Technical leader review code

#### 3.2.7, Sửa comment

- Trong trường hợp review code có comment, developer cần sửa lại code theo comment
- Thực hiện lại các bước 3.2.3 (với commit message bất kỳ)
- Rebase code (để merge code với commit đã có trước đó)
	```
    git rebase -i HEAD~2
    use f (fixup) option
    ```
- Thực hiện lại bước 3.2.4
- Push code
	```
    git push -f origin feature-20001
    ```
#### 3.2.8, Technial leader merge code và xóa branch

- Dùng chức năng merge và xóa branch trên Github, Gitlab
- Hoặc sử dụng lệnh
	```
    git merge --no-ff feature-20001
    git push origin :feature-20001
    ```
