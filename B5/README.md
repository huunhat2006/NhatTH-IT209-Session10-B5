# Bài 5: Tự động hóa điều phối đa dịch vụ với Docker Compose & File .env

Thư mục này chứa cấu hình Docker Compose theo chuẩn Infrastructure as Code (IaC) để tự động hóa việc triển khai hạ tầng được cấu hình thủ công ở Bài tập 4.

## Cấu trúc thư mục
- `docker-compose.yml`: File cấu hình chính, định nghĩa các services (`database`, `backend`), volumes (`pgdata`) và networks (`backend-net`).
- `.env.example`: File mẫu chứa các biến môi trường nhạy cảm.
- `.gitignore`: Bỏ qua file `.env` khi push code lên Git.
- `Dockerfile`: File xây dựng image cho dịch vụ `backend`. Mặc định dùng Alpine Linux với khả năng ping.

## Hướng dẫn sử dụng

### 1. Chuẩn bị biến môi trường
Tạo file `.env` từ file mẫu `.env.example`:
```bash
cp .env.example .env
```
(Sau đó bạn có thể sửa đổi các giá trị trong `.env` theo ý muốn).

### 2. Khởi chạy hệ thống
Sử dụng Docker Compose để build image và chạy các container ngầm:
```bash
docker compose up -d
```
Lệnh này sẽ tự động:
- Tạo network `backend-net`.
- Tạo volume `pgdata`.
- Tải image `postgres:15-alpine` và chạy database container.
- Build image cho `backend` từ `Dockerfile` và chạy container, truyền biến môi trường từ `.env` vào.

### 3. Kiểm tra các dịch vụ
- Xem trạng thái các container:
```bash
docker compose ps
```
- Xem log của hệ thống:
```bash
docker compose logs -f
```

### 4. Dọn dẹp
Khi muốn dừng và xóa toàn bộ container cùng với network (nhưng vẫn giữ lại dữ liệu trong volume):
```bash
docker compose down
```
Nếu muốn xóa cả volume dữ liệu:
```bash
docker compose down -v
```
