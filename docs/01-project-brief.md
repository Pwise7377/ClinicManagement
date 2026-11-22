# Project Brief – Clinic Management System

> Trạng thái: **Nháp v0.3** (28/10/2026) – đã chốt các câu hỏi mở của v0.2. Câu hỏi còn lại ở mục 9.

## 1. Thông tin chung

| Mục | Nội dung |
|---|---|
| Môn học | IT008 – Lập trình trực quan (UIT) |
| Đề tài | Clinic Management System – phần mềm quản lý phòng khám |
| Loại ứng dụng | Desktop (WPF) |
| Nhóm | 2 thành viên: Phạm Minh Phát (21520392), Nguyễn Tấn Phát (25521371) |
| Thời gian | 26/10/2026 → 10/01/2027 |
| Quản lý mã nguồn | Git + GitHub |

## 2. Bối cảnh và mục tiêu

**Phòng khám mục tiêu:** phòng khám đa chuyên khoa ngoại trú quy mô nhỏ, chỉ tiếp nhận ca nhẹ (chấn thương chỉnh hình như bó bột tay/chân, tai mũi họng, mắt, răng hàm mặt, tiêu hóa/siêu âm bụng). Chủ đầu tư là Admin; bác sĩ là cộng tác viên được thuê về làm việc theo lịch; lễ tân điều phối và là người tiếp xúc đầu tiên với bệnh nhân.

Phòng khám loại này thường quản lý bệnh nhân, lịch hẹn, đơn thuốc và hóa đơn bằng sổ sách hoặc Excel, dễ trùng lịch, sai sót và khó thống kê.

**Mục tiêu của đồ án:**
- Số hóa quy trình khám: tiếp nhận → đặt lịch → khám → kê đơn → thanh toán.
- Lưu trữ tập trung hồ sơ bệnh nhân và lịch sử khám.
- Quản lý bác sĩ theo chuyên khoa và lịch làm việc; thống kê doanh thu theo bác sĩ.
- Cung cấp thống kê nhanh (doanh thu, lượt khám) cho quản lý.
- Thực hành thiết kế cơ sở dữ liệu (SQL Server), WPF/MVVM và Entity Framework Core.

**Giả định:** phòng khám là cơ sở khám bệnh BHYT (đã ký hợp đồng) nên nhận được thẻ BHYT; mức hưởng lấy theo quy định hiện hành, cần đối chiếu nguồn chính thức khi hoàn thiện.

**Nguyên tắc:** làm cơ bản, đi từ nền tảng lên. Ngoài CRUD, giữ 3 nghiệp vụ trọng tâm để học DB: chống trùng lịch, trừ tồn kho trong transaction, tính hóa đơn có bảo hiểm.

## 3. Người dùng và quy mô

| Vai trò | Mô tả ngắn | Số tài khoản (dự kiến) |
|---|---|:-:|
| Admin | Chủ đầu tư/người đứng đầu: quản lý bác sĩ, nhân viên, chuyên khoa, lịch làm việc, danh mục; duyệt giảm giá; xem thống kê | 1 – 3 |
| Bác sĩ | Cộng tác viên hưởng lương cứng + chia doanh thu, mỗi người một chuyên khoa: đăng ký lịch rảnh, ghi hồ sơ bệnh án, kê đơn | 10 – 30 |
| Lễ tân | Tiếp nhận, đặt/đổi/hủy lịch, xem lịch hẹn và ngày tái khám, lập hóa đơn, thu tiền | 5 – 15 |

**Quy mô thiết kế:** khoảng 30–50 tài khoản, khoảng 10 máy kết nối cùng lúc, vài trăm lịch hẹn mỗi ngày. Ứng dụng WPF kết nối thẳng SQL Server phù hợp quy mô này; lớn hơn thì cần thêm tầng API (ghi nhận là giới hạn của đồ án). Nhiều lễ tân cùng đặt lịch là lý do cần chống trùng lịch bằng transaction.

## 4. Phạm vi

### 4.1. Trong phạm vi (MVP)
1. Đăng nhập, phân quyền theo vai trò
2. Quản lý chuyên khoa (kèm phí khám) và bác sĩ (lương cứng + tỷ lệ chia doanh thu)
3. Lịch làm việc của bác sĩ theo từng ngày (bác sĩ đăng ký lịch rảnh → Admin xếp và duyệt)
4. Quản lý bệnh nhân (kèm thông tin bảo hiểm), hồ sơ bệnh án, lịch sử khám, ngày tái khám
5. Quản lý lịch hẹn (kiểm tra trùng lịch, chỉ đặt trong giờ làm việc của bác sĩ)
6. Lập hóa đơn có bảo hiểm, ưu đãi hạng VIP và giảm giá (Admin duyệt), thanh toán, in hóa đơn
7. Danh mục thuốc, tồn kho, kê đơn và in đơn thuốc
8. Quản lý nhân viên, tài khoản
9. Dashboard thống kê (doanh thu, lượt khám, theo bác sĩ)

### 4.2. Mở rộng (làm sau MVP, chốt sau Giai đoạn 4)
- **Trợ lý phân loại triệu chứng (Microsoft Agent Framework, P2):** lễ tân nhập triệu chứng, trợ lý gợi ý chuyên khoa và các bác sĩ còn khung giờ trống; lễ tân xác nhận mới đặt lịch.
- **Trợ lý gợi ý xếp lịch làm việc (P3):** từ các đăng ký lịch rảnh, trợ lý đề xuất lịch tuần cho Admin; ràng buộc do code kiểm tra, Admin duyệt.
- Làm một ý trước, ý còn lại chỉ làm nếu còn thời gian. Ý dự phòng: tóm tắt lịch sử khám cho bác sĩ.
- Không làm chatbot hỏi đáp tự do.

### 4.3. Ngoài phạm vi
- Ứng dụng web/mobile, đặt lịch online cho bệnh nhân
- Tích hợp thanh toán điện tử; liên thông thật với Cổng tiếp nhận dữ liệu giám định BHYT của BHXH (đồ án dùng bản mock qua interface)
- Quản lý xét nghiệm, chẩn đoán hình ảnh, ca nặng/nội trú
- Đa chi nhánh

## 5. Công nghệ

| Thành phần | Lựa chọn |
|---|---|
| Nền tảng | .NET 8, WPF |
| Mô hình | MVVM (CommunityToolkit.Mvvm) |
| Dependency Injection | Microsoft.Extensions.Hosting / DependencyInjection |
| Cơ sở dữ liệu | SQL Server (LocalDB khi phát triển) |
| ORM | Entity Framework Core 8 (Code First, migration, seed data) |
| In ấn | `PrintDialog` + `FlowDocument` của WPF; chọn máy in "Microsoft Print to PDF" để xuất PDF |
| Thiết kế giao diện | Figma |
| Công cụ | Visual Studio 2026, Git, GitHub |
| Mở rộng | Microsoft Agent Framework (trợ lý phân loại triệu chứng) |

## 6. Kiến trúc dự kiến

```
Views (XAML)  ⇄  ViewModels  →  Services  →  DbContext (EF Core)  →  SQL Server
   data binding      commands      nghiệp vụ        entity, migration
```

- **Views:** chỉ chứa giao diện và binding, không chứa nghiệp vụ.
- **ViewModels:** trạng thái màn hình, command; gọi Services.
- **Services:** quy tắc nghiệp vụ (trùng lịch, tính hóa đơn, trừ tồn kho, tìm khung giờ trống).
- **Data:** entity, `DbContext`, migration, seed data.

## 7. Phân công và mốc thời gian

Chi tiết xem bảng phân công trên Google Sheets `[IT008] BẢNG PHÂN CÔNG CÔNG VIỆC`.

| Giai đoạn | Thời gian | Nội dung chính |
|---|---|---|
| 1. Phân tích, thiết kế | 26/10 – 08/11 | Đặc tả yêu cầu, use case, ERD, thiết kế Figma |
| 2. Khởi tạo dự án | 09/11 – 15/11 | Solution WPF, MVVM, DI, EF Core, layout chính, đăng nhập/phân quyền |
| 3. Patient Records | 16/11 – 29/11 | Bệnh nhân, hồ sơ bệnh án, lịch sử khám |
| 4. Appointments & Billing | 30/11 – 13/12 | Lịch hẹn, hóa đơn, thanh toán |
| 5. Prescriptions | 14/12 – 27/12 | Thuốc, tồn kho, kê đơn, quản lý nhân sự, dashboard |
| 6. Kiểm thử, báo cáo | 28/12 – 10/01 | Test case, sửa lỗi, báo cáo, slides, demo |

> Cần bổ sung vào bảng phân công: **chuyên khoa + đăng ký/xếp lịch làm việc bác sĩ** (đề xuất Giai đoạn 2–3), **lương/chia doanh thu bác sĩ** (Giai đoạn 5), **thông tin bảo hiểm/giảm giá** (Giai đoạn 4). Nếu làm trợ lý AI thì thêm task và chia lại điểm số (mỗi người tối thiểu 40%).

## 8. Rủi ro

| Rủi ro | Cách giảm thiểu |
|---|---|
| Hai người sửa cùng entity gây xung đột migration | Chỉ một người tạo migration tại một thời điểm; commit file migration ngay |
| Phạm vi phình to | Chốt MVP ở mục 4.1; mở rộng chỉ làm sau Giai đoạn 4; chỉ làm một ý tưởng AI trước |
| Lệch giao diện giữa hai người | Thống nhất style/theme ở Giai đoạn 2, bám Figma |
| Trùng lịch bác sĩ khi nhiều lễ tân đặt đồng thời | Kiểm tra trong transaction ở tầng Service |
| Lộ dữ liệu bệnh nhân khi dùng LLM | Chỉ gửi triệu chứng/ngữ cảnh, không gửi tên, SĐT; trợ lý chỉ đọc, thao tác ghi cần người dùng xác nhận |
| Lộ API key / connection string | Dùng User Secrets và `appsettings.Local.json` (đã gitignore) |
| Tính hóa đơn bảo hiểm sai | Viết test case cho công thức: bệnh nhân trả = tổng − bảo hiểm − giảm giá |

## 9. Câu hỏi còn mở

1. Doanh thu tính chia cho bác sĩ gồm những khoản nào, trước hay sau bảo hiểm?
2. Hạn đăng ký lịch rảnh và cách đổi ca sau khi Admin đã duyệt?
3. Danh mục mức hưởng BHYT theo quy định hiện hành (cần đối chiếu nguồn chính thức).
4. Hạng VIP: tiêu chí lên hạng và mức ưu đãi?
5. Làm trước ý tưởng AI nào (phân loại triệu chứng hay gợi ý xếp lịch)?
