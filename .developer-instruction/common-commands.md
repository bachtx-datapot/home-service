# Common Commands Cheat Sheet

Tài liệu này tổng hợp các câu lệnh thường xuyên được sử dụng trong quá trình phát triển dự án.

## Backend (`/backend`)

Đảm bảo bạn đang ở trong thư mục `backend` hoặc đã kích hoạt môi trường ảo.

### Database & Migrations
*   **Tạo migration mới** (sau khi sửa model):
    ```bash
    alembic revision --autogenerate -m "Mô tả thay đổi"
    ```
*   **Cập nhật database** (apply migration):
    ```bash
    alembic upgrade head
    ```
*   **Downgrade database** (quay lại phiên bản trước):
    ```bash
    alembic downgrade -1
    ```

### Development
*   **Khởi chạy server** (Dev mode with reload):
    ```bash
    fastapi dev app/main.py
    ```
    *Hoặc nếu dùng uv:*
    ```bash
    uv run fastapi dev app/main.py
    ```

### Testing & Quality
*   **Chạy test**:
    ```bash
    pytest
    ```
*   **Lint code**:
    ```bash
    ruff check .
    ```
*   **Format code**:
    ```bash
    ruff format .
    ```

## Frontend (`/frontend`)

Đảm bảo bạn đang ở trong thư mục `frontend`.

### Development
*   **Khởi chạy server** (Dev mode):
    ```bash
    npm run dev
    ```
*   **Tạo API Client** (sau khi Backend thay đổi API):
    ```bash
    npm run generate-client
    ```

### Build & Quality
*   **Build production**:
    ```bash
    npm run build
    ```
*   **Preview production build**:
    ```bash
    npm run preview
    ```
*   **Lint & Format code**:
    ```bash
    npm run lint
    ```

## Docker & Deployment (Root)

Các lệnh này thường chạy ở thư mục gốc của dự án.

*   **Khởi chạy toàn bộ hệ thống** (Local):
    ```bash
    docker-compose up -d
    ```
*   **Xem logs**:
    ```bash
    docker-compose logs -f
    ```
    *Hoặc xem log của service cụ thể:*
    ```bash
    docker-compose logs -f backend
    ```
*   **Build lại images** (khi có thay đổi Dockerfile hoặc dependencies):
    ```bash
    docker-compose up -d --build
    ```
*   **Dừng toàn bộ hệ thống**:
    ```bash
    docker-compose down
    ```
