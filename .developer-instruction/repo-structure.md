# Cấu trúc Repository

Repo này được tổ chức theo mô hình monorepo, chứa cả backend và frontend. Dưới đây là cấu trúc thư mục chính và công dụng của từng phần:

## Root Directory

*   `.github/`: Chứa các workflow của GitHub Actions (CI/CD).
*   `.developer-instruction/`: Chứa tài liệu hướng dẫn cho developer (folder này).
    *   `common-commands.md`: Tổng hợp các lệnh thường dùng.
    *   `development-workflow.md`: Quy trình phát triển.
    *   `repo-structure.md`: Cấu trúc dự án.
    *   `tech-stack.md`: Công nghệ sử dụng.
*   `backend/`: Source code của Backend (FastAPI).
*   `frontend/`: Source code của Frontend (React).
*   `scripts/`: Các script hỗ trợ development và deployment.
*   `docker-compose.yml`: File cấu hình Docker Compose để chạy toàn bộ hệ thống (backend, frontend, db, proxy).
*   `.env`: File chứa biến môi trường (cần tạo từ `.env.example` hoặc tự cấu hình).

## Backend (`/backend`)

Backend được xây dựng bằng FastAPI.

*   `app/`: Thư mục chứa source code chính.
    *   `api/`: Chứa các API endpoints (routes).
        *   `routes/`: Các file định nghĩa route cụ thể cho từng module (users, items, login...).
    *   `core/`: Các cấu hình cốt lõi (config, security, db session).
    *   `models.py`: Định nghĩa các SQLModel (Database Schema & Pydantic Models).
    *   `crud.py`: Các hàm CRUD (Create, Read, Update, Delete) để tương tác với DB.
    *   `alembic/`: Chứa các migration scripts để quản lý thay đổi database.
    *   `email-templates/`: Chứa các template HTML cho email.
    *   `main.py`: Entry point của ứng dụng FastAPI.
*   `tests/`: Chứa các test case (Pytest).
*   `pyproject.toml`: File quản lý dependencies và cấu hình tool (Ruff, Pytest...).

## Frontend (`/frontend`)

Frontend được xây dựng bằng React và Vite.

*   `src/`: Thư mục chứa source code chính.
    *   `components/`: Các UI components tái sử dụng (Button, Input, Modal...).
        *   `Common/`: Các component chung.
    *   `routes/`: Định nghĩa routing của ứng dụng (sử dụng TanStack Router).
        *   `_layout/`: Các layout chung (Admin layout, Auth layout).
    *   `hooks/`: Custom React Hooks (ví dụ: `useAuth`).
    *   `client/`: Code generated từ OpenAPI để gọi Backend API.
    *   `theme/`: Cấu hình theme cho Chakra UI.
*   `tests/`: Chứa các test case.
*   `package.json`: Quản lý dependencies và scripts (npm scripts).
*   `vite.config.ts`: Cấu hình Vite.
