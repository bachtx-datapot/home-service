# Tech Stack

Dự án này sử dụng kiến trúc Full Stack hiện đại với Backend là Python (FastAPI) và Frontend là React (Vite). Dưới đây là chi tiết các công nghệ được sử dụng:

## Backend

*   **Language:** Python 3.10+
*   **Framework:** [FastAPI](https://fastapi.tiangolo.com/) - Framework hiện đại, hiệu năng cao để xây dựng API.
*   **ORM:** [SQLModel](https://sqlmodel.tiangolo.com/) - Thư viện tương tác với SQL database, kết hợp sức mạnh của SQLAlchemy và Pydantic.
*   **Database:** [PostgreSQL](https://www.postgresql.org/) - Hệ quản trị cơ sở dữ liệu quan hệ mạnh mẽ.
*   **Validation:** [Pydantic](https://docs.pydantic.dev/) - Sử dụng để validate dữ liệu và quản lý settings.
*   **Migrations:** [Alembic](https://alembic.sqlalchemy.org/) - Công cụ quản lý thay đổi schema database.
*   **Authentication:** JWT (JSON Web Token) với `python-jose` và `passlib` để hash password (bcrypt).
*   **Templating:** [Jinja2](https://jinja.palletsprojects.com/) - Sử dụng cho email templates.
*   **Testing:** [Pytest](https://pytest.org/) - Framework testing mạnh mẽ cho Python.
*   **Linting/Formatting:** [Ruff](https://docs.astral.sh/ruff/) - Linter và formatter tốc độ cao.

## Frontend

*   **Library:** [React](https://react.dev/) 19 - Thư viện UI phổ biến nhất.
*   **Build Tool:** [Vite](https://vitejs.dev/) - Build tool thế hệ mới, cực nhanh.
*   **Language:** [TypeScript](https://www.typescriptlang.org/) - Superset của JavaScript, giúp code an toàn và dễ bảo trì hơn.
*   **UI Framework:** [Chakra UI](https://chakra-ui.com/) - Thư viện component React đơn giản, module và dễ tiếp cận.
*   **State Management/Data Fetching:** [TanStack Query](https://tanstack.com/query/latest) (React Query) - Quản lý server state mạnh mẽ.
*   **Routing:** [TanStack Router](https://tanstack.com/router/latest) - Router type-safe cho React.
*   **HTTP Client:** [Axios](https://axios-http.com/) - Thư viện gọi API.
*   **Forms:** [React Hook Form](https://react-hook-form.com/) - Quản lý form hiệu năng cao.
*   **E2E Testing:** [Playwright](https://playwright.dev/) - Công cụ test tự động end-to-end.
*   **Linting:** [Biome](https://biomejs.dev/) - Toolchain web project hiệu năng cao (linter, formatter).

## Infrastructure & DevOps

*   **Containerization:** [Docker](https://www.docker.com/) & Docker Compose - Đóng gói ứng dụng. **Dev Containers** được sử dụng để chuẩn hóa môi trường phát triển.
*   **Reverse Proxy:** [Traefik](https://traefik.io/) - Load balancer và reverse proxy hiện đại.
*   **CI/CD:** GitHub Actions - Tự động hóa quy trình test và deploy.
*   **Project Generation:** [Copier](https://copier.readthedocs.io/) - Công cụ tạo và update project từ template.
