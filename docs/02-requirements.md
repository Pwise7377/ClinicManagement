# Đặc tả yêu cầu – Clinic Management System

> Trạng thái: **Nháp v0.3** (28/10/2026) – đã chốt các câu hỏi mở: quy trình xếp lịch, lương + chia doanh thu, bảo hiểm theo mức hưởng, hạng VIP, trợ lý AI. Mức ưu tiên: **P1** = bắt buộc (MVP), **P2** = nên có, **P3** = mở rộng. Mục ❓ còn cần cả nhóm chốt.

## 1. Vai trò và phân quyền

| Chức năng | Admin | Bác sĩ | Lễ tân |
|---|:-:|:-:|:-:|
| Đăng nhập / đổi mật khẩu | ✓ | ✓ | ✓ |
| Quản lý chuyên khoa, phí khám | ✓ | xem | xem |
| Quản lý bác sĩ, nhân viên, tài khoản | ✓ | – | – |
| Đăng ký lịch rảnh | – | ✓ | – |
| Xếp và duyệt lịch làm việc | ✓ | xem lịch của mình | xem |
| Quản lý bệnh nhân (thêm/sửa/tìm) | ✓ | xem | ✓ |
| Xem thông tin hành chính, lịch hẹn, ngày tái khám, tóm tắt các lần khám (ngày, bác sĩ, chuyên khoa) | ✓ | ✓ | ✓ |
| Xem chẩn đoán, triệu chứng, đơn thuốc chi tiết | ✓ | ✓ | – |
| Ghi hồ sơ bệnh án, đặt ngày tái khám | – | ✓ | – |
| Quản lý lịch hẹn | ✓ | xem lịch của mình | ✓ |
| Kê đơn thuốc | – | ✓ | – |
| Lập hóa đơn, thanh toán | ✓ | – | ✓ |
| Duyệt giảm giá thủ công | ✓ | – | – |
| Quản lý hạng khách hàng (VIP) | ✓ | – | – |
| Xem báo cáo thu nhập bác sĩ | ✓ | – | – |
| Danh mục thuốc, tồn kho | ✓ | xem | xem |
| Dashboard thống kê | ✓ | – | – |
| Trợ lý phân loại triệu chứng | ✓ | – | ✓ |
| Trợ lý gợi ý xếp lịch làm việc (P3) | ✓ | – | – |

## 2. Yêu cầu chức năng

### 2.1. Xác thực và phân quyền (AUTH)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| AUTH-01 | Đăng nhập bằng tên đăng nhập và mật khẩu | P1 |
| AUTH-02 | Mật khẩu lưu dạng băm (hash), không lưu plain text | P1 |
| AUTH-03 | Sau đăng nhập chỉ hiển thị menu/màn hình theo vai trò | P1 |
| AUTH-04 | Đăng xuất, đổi mật khẩu | P2 |
| AUTH-05 | Admin tạo, khóa/mở khóa tài khoản | P1 |

### 2.2. Chuyên khoa (SPC)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| SPC-01 | Danh mục chuyên khoa (chỉnh hình, tai mũi họng, mắt, răng hàm mặt, tiêu hóa, …) | P1 |
| SPC-02 | Mỗi chuyên khoa có phí khám cố định; Admin chỉnh được | P1 |
| SPC-03 | Mỗi bác sĩ thuộc đúng một chuyên khoa chính | P1 |
| SPC-04 | Ngưng sử dụng chuyên khoa (không xóa nếu đã có dữ liệu khám) | P2 |

### 2.3. Bác sĩ, nhân viên và lịch làm việc (STF, SCH)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| STF-01 | Thêm/sửa/khóa bác sĩ: họ tên, chuyên khoa, SĐT | P1 |
| STF-02 | Thêm/sửa/khóa nhân viên (lễ tân) | P1 |
| STF-03 | Gắn tài khoản đăng nhập với bác sĩ/nhân viên | P1 |
| STF-04 | Lưu lương cứng và tỷ lệ chia doanh thu (%) của từng bác sĩ | P1 |
| STF-05 | Báo cáo thu nhập bác sĩ theo tháng = lương cứng + tỷ lệ chia × doanh thu tính chia (xem câu hỏi mở 1) | P2 |
| SCH-01 | Bác sĩ đăng ký lịch rảnh theo từng ngày: ngày, giờ bắt đầu, giờ kết thúc (trạng thái Chờ duyệt) | P1 |
| SCH-02 | Admin xem tổng hợp đăng ký theo ngày/chuyên khoa, xếp ca và duyệt/từ chối; Admin là người quyết định lịch cuối cùng | P1 |
| SCH-03 | Lịch làm việc chính thức xếp theo từng ngày: ngày, bác sĩ, giờ bắt đầu, giờ kết thúc; chỉ ca đã duyệt mới cho phép đặt lịch hẹn | P1 |
| SCH-04 | Không xếp hai ca làm việc chồng giờ cho cùng một bác sĩ | P1 |
| SCH-05 | Bác sĩ xem lịch làm việc của mình và trạng thái các đăng ký | P1 |
| SCH-06 | Tìm các khung giờ trống của bác sĩ/chuyên khoa theo ngày (= giờ làm việc trừ lịch hẹn đã có) | P1 |
| SCH-07 | Bác sĩ gửi yêu cầu đổi/hủy ca đã duyệt; Admin duyệt; ca đã có lịch hẹn thì phải xử lý các lịch hẹn đó trước | P2 |

### 2.4. Quản lý bệnh nhân (PAT)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| PAT-01 | Thêm bệnh nhân: họ tên, ngày sinh, giới tính, SĐT, địa chỉ | P1 |
| PAT-02 | Sửa thông tin bệnh nhân | P1 |
| PAT-03 | Danh sách bệnh nhân, phân trang, tìm kiếm theo tên/SĐT/mã | P1 |
| PAT-04 | Validate dữ liệu (không trống, SĐT hợp lệ, ngày sinh không ở tương lai) | P1 |
| PAT-05 | Xóa mềm bệnh nhân (đánh dấu, không xóa dữ liệu khám) | P2 |
| PAT-06 | Ghi nhận tiền sử dị ứng, ghi chú | P2 |
| PAT-07 | Thông tin bảo hiểm: mã thẻ BHYT, mức hưởng (%) chọn từ danh mục do Admin cấu hình (vd 100% / 95% / 80%, theo quy định hiện hành), hạn sử dụng | P1 |
| PAT-08 | Phân biệt bệnh nhân mới / bệnh nhân cũ (có lượt khám trước đó) để lễ tân điều phối | P1 |
| PAT-09 | Kiểm tra mã thẻ BHYT qua interface `IInsuranceLookupService`: bản đồ án dùng mock (kiểm tra định dạng, tra dữ liệu mẫu); bản thật có thể gọi API tra cứu của Cổng tiếp nhận dữ liệu giám định BHYT (BHXH), cần tài khoản cơ sở khám chữa bệnh | P2 |
| PAT-10 | Hạng khách hàng (Thường / VIP) do Admin đặt cho bệnh nhân | P2 |

### 2.5. Hồ sơ bệnh án và tái khám (REC)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| REC-01 | Mỗi lượt khám tạo một bản ghi: bác sĩ, ngày khám, triệu chứng, chẩn đoán, ghi chú | P1 |
| REC-02 | Xem lịch sử khám của một bệnh nhân theo thời gian | P1 |
| REC-03 | Bác sĩ chỉ sửa bản ghi do mình lập | P2 |
| REC-04 | Liên kết lượt khám với đơn thuốc và hóa đơn | P1 |
| REC-05 | Bác sĩ đặt **ngày tái khám dự kiến** cho lượt khám | P1 |
| REC-06 | Lễ tân xem được ngày tái khám và tóm tắt các lần khám (ngày, bác sĩ, chuyên khoa), **không** xem chẩn đoán/triệu chứng/đơn thuốc chi tiết | P1 |
| REC-07 | Danh sách bệnh nhân đến hạn tái khám để lễ tân liên hệ | P2 |

### 2.6. Lịch hẹn (APT)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| APT-01 | Đặt lịch hẹn: bệnh nhân, bác sĩ, ngày, giờ bắt đầu, giờ kết thúc, lý do khám | P1 |
| APT-02 | Thời lượng chọn tự do theo bội số 15 phút; mặc định 20 phút; thủ thuật/bó bột chọn dài hơn | P1 |
| APT-03 | Không cho đặt nếu khoảng giờ giao nhau với lịch hẹn khác của bác sĩ | P1 |
| APT-04 | Chỉ cho đặt trong khung giờ làm việc của bác sĩ (SCH-03) | P1 |
| APT-05 | Xem lịch theo ngày và theo tuần, lọc theo bác sĩ/chuyên khoa | P1 |
| APT-06 | Đổi lịch, hủy lịch (ghi lý do hủy) | P1 |
| APT-07 | Trạng thái lịch: Đã đặt → Đã đến → Đã khám / Đã hủy / Vắng mặt | P1 |
| APT-08 | Bác sĩ xem danh sách bệnh nhân chờ khám trong ngày | P2 |
| APT-09 | Lễ tân xem bệnh nhân có hẹn với bác sĩ nào, kèm đánh dấu tái khám | P1 |

### 2.7. Hóa đơn và thanh toán (INV)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| INV-01 | Lập hóa đơn cho lượt khám: phí khám (theo chuyên khoa) + dịch vụ + tiền thuốc | P1 |
| INV-02 | **Bảo hiểm:** áp mức hưởng (%) của thẻ lên các khoản thuộc danh mục BHYT chi trả (dịch vụ/thuốc có cờ bảo hiểm) | P1 |
| INV-03 | **Giảm giá thủ công:** có lý do, cần Admin duyệt | P2 |
| INV-04 | Bệnh nhân phải trả = tổng − bảo hiểm − ưu đãi hạng − giảm giá thủ công (không âm) | P1 |
| INV-05 | Trạng thái: Chưa thanh toán / Đã thanh toán | P1 |
| INV-06 | Ghi nhận thanh toán (tiền mặt/chuyển khoản), ngày thanh toán | P1 |
| INV-07 | Danh sách hóa đơn, lọc theo ngày/trạng thái | P1 |
| INV-08 | In hóa đơn (xem mục 4, NFR-08) | P1 |
| INV-09 | Hóa đơn đã thanh toán không được sửa | P1 |
| INV-10 | Danh mục dịch vụ (thủ thuật, siêu âm, …), giá, thuộc danh mục BHYT chi trả hay không | P1 |
| INV-11 | **Ưu đãi hạng khách hàng:** hạng VIP tự giảm theo % Admin cấu hình, tính trên phần bệnh nhân tự trả sau bảo hiểm | P2 |

### 2.8. Thuốc và kê đơn (MED, RX)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| MED-01 | Danh mục thuốc: tên, đơn vị, giá, số lượng tồn, thuộc danh mục BHYT chi trả hay không | P1 |
| MED-02 | Thêm/sửa/ngưng sử dụng thuốc | P1 |
| MED-03 | Nhập thêm kho (cập nhật tồn) | P2 |
| MED-04 | Cảnh báo thuốc sắp hết (dưới ngưỡng) | P3 |
| RX-01 | Bác sĩ kê đơn trong lượt khám: chọn thuốc, số lượng, liều dùng, ghi chú | P1 |
| RX-02 | Không cho kê vượt số lượng tồn | P1 |
| RX-03 | Kê đơn xong tự trừ tồn kho (cùng transaction) | P1 |
| RX-04 | In đơn thuốc (xem NFR-08) | P1 |
| RX-05 | Tiền thuốc của đơn được đưa vào hóa đơn | P1 |
| RX-06 | Cảnh báo khi thuốc trùng với dị ứng đã ghi nhận (kiểm tra bằng dữ liệu/luật trong DB, không dùng LLM) | P2 |

### 2.9. Dashboard thống kê (DSH)

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| DSH-01 | Thẻ tổng quan: lượt khám hôm nay, doanh thu hôm nay, lịch hẹn sắp tới | P1 |
| DSH-02 | Biểu đồ doanh thu theo ngày/tháng | P1 |
| DSH-03 | Biểu đồ lượt khám theo ngày/tháng | P1 |
| DSH-04 | Thống kê theo bác sĩ: số lượt khám, doanh thu | P1 |
| DSH-05 | Thống kê theo chuyên khoa | P2 |
| DSH-06 | Top thuốc kê nhiều nhất | P3 |

### 2.10. Trợ lý AI (AI) – Microsoft Agent Framework

| ID | Yêu cầu | Ưu tiên |
|---|---|:-:|
| AI-01 | Lễ tân nhập mô tả triệu chứng; trợ lý gợi ý chuyên khoa phù hợp trong danh mục SPC-01 | P2 |
| AI-02 | Trợ lý gọi hàm tìm khung giờ trống (SCH-06) và đề xuất các bác sĩ còn lịch | P2 |
| AI-03 | Lễ tân xác nhận gợi ý rồi mới đặt lịch qua luồng APT thông thường (vẫn kiểm tra trùng lịch) | P2 |
| AI-04 | Chỉ gửi triệu chứng/ngữ cảnh lên LLM, **không** gửi tên, SĐT hay mã định danh bệnh nhân | P2 |
| AI-05 | Trợ lý chỉ đọc dữ liệu; không tự thêm/sửa/xóa | P2 |
| AI-06 | Hiển thị rõ "gợi ý tham khảo, không thay thế chẩn đoán của bác sĩ" | P2 |
| AI-07 | Admin yêu cầu trợ lý gợi ý xếp lịch làm việc cho một tuần từ các đăng ký lịch rảnh (SCH-01) | P3 |
| AI-08 | Kiểm tra ràng buộc (không chồng ca, mỗi chuyên khoa có bác sĩ) do code thực hiện; trợ lý chỉ đề xuất và giải thích | P3 |
| AI-09 | Gợi ý xếp lịch ở trạng thái "Đề xuất"; chỉ có hiệu lực sau khi Admin duyệt (SCH-02) | P3 |

> Làm một ý trước (khuyến nghị AI-01 → AI-06), ý còn lại chỉ làm nếu còn thời gian; chốt sau Giai đoạn 4. Ý dự phòng: tóm tắt lịch sử khám cho bác sĩ (chỉ đọc); đặt lịch bằng câu lệnh tự nhiên (có bước xác nhận).

## 3. Quy tắc nghiệp vụ

| ID | Quy tắc |
|---|---|
| BR-01 | Một bác sĩ không có hai lịch hẹn có khoảng giờ giao nhau. |
| BR-02 | Lịch hẹn phải nằm trong khung giờ làm việc của bác sĩ và không đặt trong quá khứ. |
| BR-03 | Một bệnh nhân có nhiều lượt khám; mỗi lượt khám có tối đa một hồ sơ, một đơn thuốc và một hóa đơn. |
| BR-04 | Mỗi bác sĩ thuộc đúng một chuyên khoa; phí khám lấy theo chuyên khoa. |
| BR-05 | Số lượng thuốc kê không vượt tồn kho; kê đơn thành công thì trừ tồn trong cùng một transaction. |
| BR-06 | Tổng hóa đơn = phí khám + tổng dịch vụ + tổng tiền thuốc. Bệnh nhân trả = tổng − bảo hiểm − ưu đãi hạng − giảm giá thủ công (không âm). |
| BR-07 | Bảo hiểm chỉ áp dụng khi thẻ còn hạn tại ngày khám. |
| BR-08 | Giảm giá thủ công bắt buộc có lý do và do Admin duyệt. |
| BR-09 | Hóa đơn đã thanh toán không được sửa hoặc xóa. |
| BR-10 | Tài khoản bị khóa không đăng nhập được. |
| BR-11 | Dữ liệu bệnh nhân/khám bệnh không xóa cứng, chỉ xóa mềm. |
| BR-12 | Hủy lịch phải ghi lý do. |
| BR-13 | Lễ tân không xem chẩn đoán, triệu chứng, đơn thuốc chi tiết của bệnh nhân. |
| BR-14 | Bảo hiểm chỉ áp lên khoản thuộc danh mục BHYT chi trả, theo mức hưởng ghi trên thẻ của bệnh nhân. |
| BR-15 | Ưu đãi hạng VIP tính trên phần bệnh nhân tự trả sau khi trừ bảo hiểm. |
| BR-16 | Lịch làm việc xếp theo từng ngày; ca chỉ có hiệu lực sau khi Admin duyệt. |
| BR-17 | Thu nhập tháng của bác sĩ = lương cứng + tỷ lệ chia × doanh thu tính chia của các hóa đơn đã thanh toán trong tháng. |

## 4. Yêu cầu phi chức năng

| ID | Yêu cầu |
|---|---|
| NFR-01 | Giao diện tiếng Việt, thống nhất style/theme toàn ứng dụng. |
| NFR-02 | Thao tác thông thường (tìm kiếm, mở danh sách) phản hồi dưới 2 giây với dữ liệu mẫu cỡ vài nghìn bản ghi. |
| NFR-03 | Tải dữ liệu bất đồng bộ (async/await), không làm treo giao diện. |
| NFR-04 | Báo lỗi dễ hiểu, không hiển thị stack trace cho người dùng. |
| NFR-05 | Mật khẩu băm; connection string và API key không đưa lên Git (User Secrets, `appsettings.Local.json`). |
| NFR-06 | Có seed data mẫu (chuyên khoa, bác sĩ, lịch làm việc, thuốc, bệnh nhân) để chạy demo ngay sau khi clone repo. |
| NFR-07 | Mã nguồn theo quy ước chung (xem `06-conventions.md`), mỗi tính năng làm trên một nhánh và được review trước khi merge. |
| NFR-08 | In ấn dùng `PrintDialog` + `FlowDocument` của WPF; chọn máy in "Microsoft Print to PDF" để xuất PDF, không cần thư viện ngoài. |
| NFR-09 | Quy mô thiết kế: khoảng 30–50 tài khoản (Admin 1–3, bác sĩ 10–30, lễ tân 5–15), khoảng 10 máy kết nối cùng lúc, vài trăm lịch hẹn mỗi ngày. |
| NFR-10 | Đặt lịch đồng thời từ nhiều máy không tạo lịch trùng (kiểm tra trong transaction). |
| NFR-11 | Dịch vụ bên ngoài (tra cứu BHYT, LLM) truy cập qua interface để thay bản mock bằng bản thật mà không sửa nghiệp vụ. |

## 5. Quyết định đã chốt (v0.3)

| # | Nội dung | Quyết định |
|---|---|---|
| 1 | Lịch làm việc | Xếp theo từng ngày; bác sĩ đăng ký lịch rảnh, Admin xếp và duyệt |
| 2 | Bác sĩ cộng tác viên | Lương cứng + chia doanh thu |
| 3 | Bảo hiểm | Theo mức hưởng quy định (danh mục cấu hình); thêm hạng khách hàng VIP (P2) |
| 4 | Giảm giá thủ công | Admin duyệt; tra cứu thẻ BHYT qua interface (bản đồ án dùng mock) |
| 5 | Trợ lý AI | Có làm, sau MVP: phân loại triệu chứng (P2), gợi ý xếp lịch (P3) |

## 6. Câu hỏi còn mở

1. Doanh thu tính chia cho bác sĩ gồm những khoản nào (phí khám + dịch vụ, có tính tiền thuốc không) và tính trước hay sau bảo hiểm?
2. Hạn đăng ký lịch rảnh (vd trước một ngày cố định mỗi tuần) và cách đổi ca sau khi Admin đã duyệt?
3. Danh mục nhóm đối tượng và mức hưởng BHYT: đối chiếu quy định hiện hành từ nguồn chính thức; giả định phòng khám là cơ sở khám chữa bệnh BHYT.
4. Hạng VIP: tiêu chí lên hạng (số lượt khám/chi tiêu) hay Admin đặt tay? Mức ưu đãi bao nhiêu %?
5. Làm trước ý tưởng AI nào: phân loại triệu chứng hay gợi ý xếp lịch (chốt sau Giai đoạn 4)?
