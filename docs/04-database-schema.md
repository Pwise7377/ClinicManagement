# Lược đồ CSDL: Clinic Management System

Bản nháp v0.1 (21/11/2026), sinh từ `schema-page.xml`. SQL Server, EF Core 8 (Code First). Kiểu dữ liệu là đề xuất. Cột không ghi NULL là NOT NULL.

## Các bảng

### PatientTier (mức P2)

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| TierId | int | PK |
| Name | nvarchar(50) | UQ |
| DiscountPercent | decimal(5,2) |  |

### Patient

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| PatientId | int | PK |
| FullName | nvarchar(100) |  |
| DateOfBirth | date |  |
| Gender | nvarchar(10) |  |
| Phone | varchar(15) |  |
| Address | nvarchar(200) |  |
| TierId | int | FK |
| IsDeleted | bit |  |
| CreatedAt | datetime2 |  |

### PatientAllergy (mức P2)

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| AllergyId | int | PK |
| PatientId | int | FK |
| ActiveIngredient | nvarchar(100) |  |
| Note | nvarchar(200) |  |

### InsuranceCard

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| CardId | int | PK |
| PatientId | int | FK |
| CardNumber | varchar(15) | UQ |
| LevelId | int | FK |
| ExpiryDate | date |  |

### InsuranceLevel

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| LevelId | int | PK |
| Name | nvarchar(50) | UQ |
| CoveragePercent | decimal(5,2) |  |

### Appointment

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| AppointmentId | int | PK |
| PatientId | int | FK |
| DoctorId | int | FK |
| StartTime | datetime2 |  |
| EndTime | datetime2 |  |
| Reason | nvarchar(200) |  |
| Status | nvarchar(20) |  |
| CancelReason | nvarchar(200) |  |
| CreatedAt | datetime2 |  |

### Visit

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| VisitId | int | PK |
| AppointmentId | int | FK,UQ |
| Symptoms | nvarchar(max) |  |
| Diagnosis | nvarchar(max) |  |
| Note | nvarchar(max) |  |
| FollowUpDate | date |  |
| CreatedAt | datetime2 |  |

### Prescription

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| PrescriptionId | int | PK |
| VisitId | int | FK,UQ |
| CreatedAt | datetime2 |  |

### PrescriptionDetail

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| DetailId | int | PK |
| PrescriptionId | int | FK |
| MedicineId | int | FK |
| Quantity | int |  |
| UnitPrice | decimal(18,0) |  |
| Dosage | nvarchar(200) |  |

### VisitService

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| VisitServiceId | int | PK |
| VisitId | int | FK |
| ServiceId | int | FK |
| Quantity | int |  |
| UnitPrice | decimal(18,0) |  |

### Invoice

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| InvoiceId | int | PK |
| VisitId | int | FK,UQ |
| CardId | int | FK, NULL |
| ConsultationFee | decimal(18,0) |  |
| ServiceTotal | decimal(18,0) |  |
| MedicineTotal | decimal(18,0) |  |
| InsuranceAmount | decimal(18,0) |  |
| TierDiscount | decimal(18,0) |  |
| ManualDiscount | decimal(18,0) |  |
| ManualDiscountReason | nvarchar(200) |  |
| ManualDiscountStatus | nvarchar(20) |  |
| DiscountApprovedBy | int | FK, NULL |
| PatientPay | decimal(18,0) |  |
| Status | nvarchar(20) |  |
| PaymentMethod | nvarchar(20) |  |
| PaidAt | datetime2 | NULL |
| CreatedBy | int | FK |
| CreatedAt | datetime2 |  |

### Specialty

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| SpecialtyId | int | PK |
| Name | nvarchar(100) | UQ |
| ConsultationFee | decimal(18,0) |  |
| IsActive | bit |  |

### Doctor

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| DoctorId | int | PK |
| AccountId | int | FK,UQ |
| SpecialtyId | int | FK |
| BaseSalary | decimal(18,0) |  |
| RevenueShareRate | decimal(5,2) |  |

### DoctorAvailability

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| AvailabilityId | int | PK |
| DoctorId | int | FK |
| WorkDate | date |  |
| StartTime | time |  |
| EndTime | time |  |
| Status | nvarchar(20) |  |
| CreatedAt | datetime2 |  |

### Medicine

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| MedicineId | int | PK |
| Name | nvarchar(100) |  |
| ActiveIngredient | nvarchar(100) |  |
| Unit | nvarchar(20) |  |
| Price | decimal(18,0) |  |
| StockQuantity | int |  |
| IsInsuranceCovered | bit |  |
| IsActive | bit |  |

### StockEntry (mức P2)

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| EntryId | int | PK |
| MedicineId | int | FK |
| Quantity | int |  |
| ReceivedAt | datetime2 |  |

### Service

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| ServiceId | int | PK |
| Name | nvarchar(100) |  |
| Price | decimal(18,0) |  |
| IsInsuranceCovered | bit |  |
| IsActive | bit |  |

### Account

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| AccountId | int | PK |
| Username | nvarchar(50) | UQ |
| PasswordHash | nvarchar(200) |  |
| FullName | nvarchar(100) |  |
| Phone | varchar(15) |  |
| Role | nvarchar(20) |  |
| IsActive | bit |  |
| CreatedAt | datetime2 |  |

### WorkShift

| Cột | Kiểu | Ràng buộc |
|---|---|---|
| ShiftId | int | PK |
| DoctorId | int | FK |
| AvailabilityId | int | FK, NULL |
| WorkDate | date |  |
| StartTime | time |  |
| EndTime | time |  |
| ApprovedBy | int | FK |
| Status | nvarchar(20) |  |

## Quan hệ

| Bảng cha (1) | Bảng con (nhiều) | Khóa ngoại |
|---|---|---|
| Account | Doctor | Doctor.AccountId |
| Specialty | Doctor | Doctor.SpecialtyId |
| Doctor | DoctorAvailability | DoctorAvailability.DoctorId |
| Doctor | WorkShift | WorkShift.DoctorId |
| DoctorAvailability | WorkShift | WorkShift.AvailabilityId |
| Account | WorkShift | WorkShift.ApprovedBy |
| PatientTier | Patient | Patient.TierId |
| Patient | InsuranceCard | InsuranceCard.PatientId |
| InsuranceLevel | InsuranceCard | InsuranceCard.LevelId |
| Patient | PatientAllergy | PatientAllergy.PatientId |
| Patient | Appointment | Appointment.PatientId |
| Doctor | Appointment | Appointment.DoctorId |
| Appointment | Visit | Visit.AppointmentId |
| Visit | Prescription | Prescription.VisitId |
| Prescription | PrescriptionDetail | PrescriptionDetail.PrescriptionId |
| Medicine | PrescriptionDetail | PrescriptionDetail.MedicineId |
| Medicine | StockEntry | StockEntry.MedicineId |
| Visit | VisitService | VisitService.VisitId |
| Service | VisitService | VisitService.ServiceId |
| Visit | Invoice | Invoice.VisitId |
| InsuranceCard | Invoice | Invoice.CardId |
| Account | Invoice | Invoice.CreatedBy |
| Account | Invoice | Invoice.DiscountApprovedBy |

## Giá trị liệt kê (lưu chuỗi, đề xuất)

| Cột | Giá trị |
|---|---|
| Account.Role | Admin, Doctor, Receptionist |
| DoctorAvailability.Status | Pending, Approved, Rejected |
| WorkShift.Status | Active, Cancelled |
| Appointment.Status | Booked, Arrived, Examined, Cancelled, NoShow |
| Invoice.ManualDiscountStatus | None, Pending, Approved, Rejected |
| Invoice.Status | Unpaid, Paid |
| Invoice.PaymentMethod | Cash, Card, Transfer |

## Ràng buộc bổ sung

- CHECK: EndTime > StartTime (DoctorAvailability, WorkShift, Appointment); Quantity > 0; StockQuantity >= 0; phần trăm trong 0..100.
- Tiền VND dùng decimal(18,0); phần trăm dùng decimal(5,2).
- Xóa mềm bằng IsDeleted/IsActive; không xóa cứng dữ liệu khám bệnh.
- Kiểm tra trong transaction ở tầng Service (không biểu diễn được bằng ràng buộc cột): lịch hẹn không giao nhau và nằm trong ca đã duyệt (BR-01, BR-02); kê đơn không vượt tồn và trừ tồn cùng transaction (BR-05); hóa đơn đã thanh toán không được sửa (BR-09).
- Giá chụp lại khi phát sinh: PrescriptionDetail.UnitPrice, VisitService.UnitPrice, các khoản trong Invoice.
- Dị ứng khớp theo hoạt chất: PatientAllergy.ActiveIngredient so với Medicine.ActiveIngredient (không có khóa ngoại).
- Thu nhập bác sĩ không có bảng riêng, tính bằng truy vấn từ Invoice đã thanh toán, BaseSalary và RevenueShareRate.
