# Hướng dẫn Khởi tạo Dự án (Getting Started)

Tài liệu này hướng dẫn các bước thiết lập và khởi chạy dự án trên thiết bị cá nhân (Local Machine).

---

## 0. Cấu hình ban đầu (Initial Configuration)

Trước khi chạy dự án, hãy kiểm tra file cấu hình:

1.  **File `.env`**:
    *   Dự án đã có sẵn file `.env` với các giá trị mặc định.
    *   **Quan trọng**: Hãy thay đổi các giá trị bảo mật như `SECRET_KEY`, `POSTGRES_PASSWORD`, `FIRST_SUPERUSER_PASSWORD` nếu bạn triển khai thực tế.
    *   Nếu chạy local mà không dùng Docker cho Database, hãy đảm bảo `POSTGRES_SERVER` trỏ đúng đến database của bạn (thường là `localhost`).

---

## 1. Làm việc trực tiếp trên thiết bị (Local Machine)

### Yêu cầu hệ thống (Prerequisites)
Trước khi bắt đầu, hãy đảm bảo bạn đã cài đặt các công cụ sau:
*   **Python**: Phiên bản 3.10 trở lên. [Tải Python](https://www.python.org/downloads/)
*   **Docker**: [Tải và cài đặt Docker](https://www.docker.com/)
*   **Node.js** (Quản lý phiên bản bằng nvm hoặc fnm):
    *   [Cài đặt fnm](https://github.com/Schniz/fnm#installation) (Khuyên dùng)
    *   Hoặc [Cài đặt nvm](https://github.com/nvm-sh/nvm#installing-and-updating)

### Thiết lập Backend
1.  **Cài đặt `uv`** (Công cụ quản lý package Python tốc độ cao):
    *   MacOS/Linux:
        ```bash
        curl -LsSf https://astral.sh/uv/install.sh | sh
        ```
    *   Windows:
        ```powershell
        powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
        ```
    *   Hoặc cài qua pip:
        ```bash
        pip install uv
        ```

2.  Di chuyển vào thư mục `backend`:
    ```bash
    cd backend
    ```

3.  **Khởi tạo môi trường ảo và cài đặt thư viện**:
    Lệnh sau sẽ tự động tạo môi trường ảo (nếu chưa có) và cài đặt tất cả dependencies từ file `pyproject.toml` và `uv.lock`.
    ```bash
    uv sync
    ```

4.  Kích hoạt môi trường ảo:
    *   MacOS/Linux:
        ```bash
        source .venv/bin/activate
        ```
    *   Windows:
        ```powershell
        .venv\Scripts\activate
        ```

5.  **Khởi chạy Backend**:
    *   **Cách 1: Chạy bằng Docker (Khuyên dùng)**
        Lệnh sau sẽ chạy toàn bộ hệ thống (Backend, Frontend, Database):
        ```bash
        docker compose watch
        ```
    *   **Cách 2: Chạy trực tiếp**
        *   Đảm bảo Database đang chạy (ví dụ: `docker compose up db -d`).
        *   Chạy lệnh:
            ```bash
            fastapi run --reload app/main.py
            ```
            *   **Lưu ý**: Lệnh này bỏ qua bước chạy migrations (`prestart.sh`). Nếu bạn chạy lần đầu hoặc có thay đổi DB, hãy chạy migrations thủ công hoặc dùng script `bash scripts/prestart.sh` (cần cài đặt `alembic` và cấu hình DB).

### Thiết lập Frontend
1.  Mở một terminal mới và di chuyển vào thư mục `frontend`:
    ```bash
    cd frontend
    ```
2.  Cài đặt và sử dụng phiên bản Node.js phù hợp (dựa trên `.nvmrc`):
    ```bash
    fnm install # hoặc nvm install
    fnm use     # hoặc nvm use
    ```
3.  Cài đặt các thư viện frontend:
    ```bash
    npm install
    ```
4.  **Khởi chạy Frontend**:
    *   **Cách 1: Chạy bằng Docker**
        Frontend đã được khởi chạy nếu bạn dùng `docker compose watch`.
    *   **Cách 2: Chạy trực tiếp**
        ```bash
        npm run dev
        ```
5.  Truy cập ứng dụng tại địa chỉ: http://localhost:5173/

---

## Các lệnh thường dùng khác

### Backend
*   **Chạy Tests**:
    ```bash
    bash ./scripts/test.sh
    ```
*   **Database Migrations (Alembic)**:
    ```bash
    # Chạy trong môi trường backend đã kích hoạt
    alembic revision --autogenerate -m "Mô tả thay đổi"
    alembic upgrade head
    ```

### Frontend
*   **Cập nhật API Client**:
    ```bash
    ./scripts/generate-client.sh
    ```
