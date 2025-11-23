# Quy trình Phát triển Tính năng Mới

Tài liệu này hướng dẫn các bước để phát triển một tính năng mới từ Backend lên Frontend.

## 1. Chuẩn bị Môi trường

Đảm bảo bạn đã setup môi trường local:
1.  Clone repo.
2.  Tạo file `.env` (tham khảo `README.md`).
3.  Khởi chạy hệ thống với Docker Compose:
    ```bash
    docker compose up -d
    ```
    *Lệnh này sẽ chạy Backend, Frontend, Database và Proxy.*

## 2. Phát triển Backend

Quy trình phát triển backend thường tuân theo thứ tự: **Model -> Migration -> CRUD -> API**.

### Bước 2.1: Định nghĩa Model
Mở file `backend/app/models.py` và thêm class SQLModel mới cho tính năng của bạn.
Ví dụ: `class Item(SQLModel, table=True): ...`

### Bước 2.2: Tạo Migration
Sau khi sửa model, cần tạo migration script để update database schema:
```bash
# Chạy trong container backend hoặc môi trường venv
alembic revision --autogenerate -m "Add Item model"
alembic upgrade head
```
*Lưu ý: Nếu dùng Docker, bạn có thể exec vào container backend để chạy lệnh này.*

### Bước 2.3: Viết CRUD Operations
Mở file `backend/app/crud.py` (hoặc tạo file mới nếu cần) và viết các hàm để thêm/sửa/xóa dữ liệu cho model mới.

### Bước 2.4: Tạo API Endpoint
1.  Tạo file mới trong `backend/app/api/routes/` (ví dụ `items.py`).
2.  Định nghĩa các endpoint (GET, POST, PUT, DELETE) sử dụng `APIRouter`.
3.  Đăng ký router mới vào `backend/app/api/main.py`.

### Bước 2.5: Test Backend
Viết test case trong `backend/tests/` và chạy `pytest` để đảm bảo logic đúng đắn.

## 3. Phát triển Frontend

Sau khi Backend API đã sẵn sàng, chuyển sang Frontend.

### Bước 3.1: Generate API Client
Frontend sử dụng code generation để tạo client gọi API. Khi backend thay đổi, cần chạy lệnh sau ở thư mục `frontend`:
```bash
npm run generate-client
```
Lệnh này sẽ cập nhật `frontend/src/client` dựa trên `openapi.json` của Backend.

### Bước 3.2: Tạo Component & UI
1.  Tạo các component giao diện cần thiết trong `frontend/src/components`.
2.  Sử dụng Chakra UI để style nhanh chóng.

### Bước 3.3: Tạo Route & Logic
1.  Tạo route mới trong `frontend/src/routes`.
2.  Sử dụng `TanStack Query` (React Query) và client đã generate ở Bước 3.1 để gọi API.
    *   Dùng `useQuery` để lấy dữ liệu (GET).
    *   Dùng `useMutation` để thay đổi dữ liệu (POST, PUT, DELETE).
3.  Xử lý loading state và error state.

## 4. Review & Merge

1.  Chạy linting để đảm bảo code style:
    *   Backend: `ruff check .`
    *   Frontend: `npm run lint`
2.  Chạy test toàn bộ.
3.  Tạo Pull Request (PR) lên GitHub.
4.  Sau khi PR được merge, CI/CD sẽ tự động deploy (nếu đã cấu hình).
