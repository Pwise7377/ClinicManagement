# Clinic Management System

Đồ án môn IT008 – Phần mềm quản lý phòng khám (WPF · .NET 8 · EF Core · SQL Server).

## Thành viên
| Họ tên | GitHub |
|---|---|
| Phạm Minh Phát | [@PwiseV](https://github.com/PwiseV) |
| Nguyễn Tấn Phát | _(điền username)_ |

## Tính năng chính
- Phân quyền 3 vai trò: **Admin**, **Bác sĩ**, **Lễ tân**
- Quản lý bệnh nhân, lịch hẹn (không trùng lịch bác sĩ), lịch làm việc bác sĩ
- Khám bệnh, kê đơn (trừ kho trong cùng transaction), hóa đơn (BHYT, ưu đãi hạng khách, giảm giá cần Admin duyệt)
- Thống kê / báo cáo thu nhập cho Admin
- (Mở rộng) Trợ lý AI phân loại triệu chứng – Microsoft Agent Framework

## Công nghệ
| Thành phần | Công nghệ |
|---|---|
| UI | WPF (.NET 8), MVVM – CommunityToolkit.Mvvm |
| DI / Config | Microsoft.Extensions.Hosting |
| ORM | Entity Framework Core 8 (Code First) |
| CSDL | SQL Server (LocalDB khi dev) |

## Yêu cầu môi trường
- Windows 10/11
- Visual Studio 2026 (workload **.NET desktop development**)
- .NET 8 SDK
- SQL Server LocalDB (đi kèm Visual Studio)
- Công cụ EF: `dotnet tool install --global dotnet-ef --version 8.*`

## Chạy dự án
```bash
git clone https://github.com/PwiseV/ClinicManagement.git
cd ClinicManagement
```
1. Mở solution trong Visual Studio.
2. Chuỗi kết nối mặc định (LocalDB) nằm trong `appsettings.json`. Muốn dùng DB khác: tạo `appsettings.Local.json` (đã gitignore) để ghi đè.
3. Tạo database:
   ```bash
   cd src/ClinicManagement.App
   dotnet ef database update
   ```
4. Nhấn **F5**.

## Cấu trúc thư mục
```
ClinicManagement/
├─ docs/                 # Tài liệu: brief, requirements, use case, schema, diagrams
├─ src/
│  └─ ClinicManagement.App/
│     ├─ Models/         # Entities
│     ├─ Data/           # DbContext, Migrations, Seed
│     ├─ Services/       # Nghiệp vụ
│     ├─ ViewModels/
│     ├─ Views/          # XAML
│     └─ Resources/      # Styles, icons
└─ README.md
```

## Quy ước làm việc
- Nhánh: `main` (ổn định) ← `feature/<tên-ngắn>`; không push thẳng vào `main`.
- Commit: `feat: ...`, `fix: ...`, `docs: ...`, `refactor: ...`.
- Mỗi thay đổi schema = 1 migration riêng, đặt tên rõ nghĩa.

## Tài liệu
- [Project Brief](docs/01-project-brief.md)
- [Đặc tả yêu cầu](docs/02-requirements.md)
- [Use Case](docs/03-use-cases.md)
- [Lược đồ CSDL](docs/04-database-schema.md) · [ERD draw.io](docs/diagrams/clinic-db.drawio)
