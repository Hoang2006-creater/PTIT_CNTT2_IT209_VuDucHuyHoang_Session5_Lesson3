# Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## 1. Mục tiêu

Thực hiện rebase nhánh `feature-api` lên nhánh `main` và xử lý các conflict phát sinh trong từng commit.

Không sử dụng merge thông thường.

## 2. Các commit ban đầu

### Nhánh main

```text
init config
update port on main
add env config
```

### Nhánh feature-api

```text
init config
feat: change port
feat: enable debug
```

## 3. Thực hiện Rebase

Chuyển sang nhánh `feature-api`:

```bash
git checkout feature-api
```

Sau đó thực hiện:

```bash
git rebase main
```

## 4. Conflict lần 1

Conflict xảy ra khi Git áp dụng commit:

```text
feat: change port
```

Nguyên nhân là cả `main` và `feature-api` đều thay đổi trường `port` trong `config.json`.

### Cách giải quyết

Giữ thay đổi `port: 9000` của nhánh `feature-api`, đồng thời giữ trường `env` đã được thêm từ `main`.

Sau khi sửa:

```json
{
  "port": 9000,
  "debug": false,
  "env": "production"
}
```

Sau đó chạy:

```bash
git add config.json
git rebase --continue
```

## 5. Conflict lần 2

Conflict tiếp tục xảy ra khi Git áp dụng commit:

```text
feat: enable debug
```

Nguyên nhân là `config.json` tiếp tục được thay đổi trong quá trình rebase.

### Cách giải quyết

Giữ các thay đổi cần thiết của cả hai nhánh:

```json
{
  "port": 9000,
  "debug": true,
  "env": "production"
}
```

Sau đó chạy:

```bash
git add config.json
git rebase --continue
```

## 6. Kiểm tra kết quả

Kiểm tra trạng thái:

```bash
git status
```

Kết quả:

```text
nothing to commit, working tree clean
```

Kiểm tra lịch sử:

```bash
git log --graph --oneline
```

Lịch sử sau khi rebase có dạng:

```text
* feat: enable debug
* feat: change port
* add env config
* update port on main
* init config
```

Lịch sử đã được đưa về dạng thẳng, không xuất hiện merge commit.

## 7. Kết quả cuối cùng

File `config.json` sau khi giải quyết toàn bộ conflict:

```json
{
  "port": 9000,
  "debug": true,
  "env": "production"
}
```

Các thay đổi của nhánh `main` được bảo toàn và các commit của `feature-api` được đặt nối tiếp phía trên `main` bằng Interactive Rebase.
