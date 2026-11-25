# Ý tưởng đề tài (QL32-W)

Nguồn đối chiếu: Đề tài 94 / `QL32-W` trong `BM01a - De bai do an QLDA CNTT (MAN104) v1.pdf` (`99-tham-chieu/trich-de-QL32-W.md`) và `00-ke-hoach/THONG-TIN-NGHIEP-VU.md`.

## 1. Bối cảnh & nỗi đau thực tế

Tại các xưởng may / xưởng gia công quy mô nhỏ, quy trình nhận đơn, chia chuyền và ghi nhận sản lượng hàng ngày thường dựa vào trao đổi miệng, bảng phấn đầu chuyền hoặc sổ tay / file Excel rời rạc. Hệ quả:

- **Phòng kế hoạch** khó theo dõi một mã hàng đang chia cho những chuyền nào theo từng ngày/ca; khi khách thay đổi số lượng hoặc hạn giao (*đơn hàng biến động*) dễ gây lệch kế hoạch.
- **Tổ trưởng** cập nhật sản lượng giờ chậm (*sản lượng không được nhập kịp thời*), nhầm lẫn giữa sản lượng đạt chuẩn và hàng lỗi, số liệu sau ca không được khóa dẫn đến tranh cãi khi tính lương khoán cuối kỳ.
- **Giám đốc** mất nhiều thời gian hỏi miệng mới biết đơn nào sắp trễ hoặc chuyền nào đang hụt định mức; thiếu màn hình trực quan tại xưởng để cả chuyền bám mục tiêu trong ngày.
- **Công nhân** không tra cứu được sản lượng đạt chuẩn đã ghi nhận và tiền lương khoán tạm tính của bản thân.
- **Thiếu kênh hỏi đáp tức thời:** Không có trợ lý AI bám sát dữ liệu thực và quy chế xưởng để trả lời nhanh tiến độ đơn hàng, cảnh báo trễ.

## 2. Ý tưởng giải pháp

Xây dựng **Website quản lý đơn hàng sản xuất và tiến độ xưởng may tích hợp Chatbot AI** hướng tới trọn vẹn **MVP + mức Tốt**:

- **Quản trị danh mục & kế hoạch khép kín:** Tách **Danh mục Mã hàng (SKU/Style) dùng chung** (lưu định mức năng suất chuẩn và đơn giá khoán) → tạo **Đơn hàng** (hỗ trợ xử lý biến động đơn khi đang sản xuất) → lập **Kế hoạch sản xuất theo chuyền (ngày + ca)**.
- **Ghi nhận sản lượng 2 tầng sát thực tế xưởng:**
  - *Theo giờ (cấp Chuyền):* Tổ trưởng nhập nhanh trên điện thoại/máy tính (`SL đạt chuẩn`, `SL lỗi tổng`, `số công nhân đứng chuyền`) để cập nhật tiến độ tức thời.
  - *Cuối ca (cấp Công nhân):* Tổ trưởng phân bổ và chốt SL đạt chuẩn cho từng công nhân trong chuyền (khớp tổng SL đạt chuẩn của ca) làm căn cứ tính lương khoán.
  - *Cơ chế khóa an toàn:* Chốt xong khóa cứng với Tổ trưởng; chỉ Admin được mở khóa kèm lý do audit khi kỳ lương chưa chốt.
- **Cảnh báo chủ động & trực quan hóa:** Tự động phát hiện 3 nguy cơ (*đơn quá hạn giao*, *hụt kế hoạch ngày*, *chậm nhập sản lượng giờ*) hiển thị đồng bộ trên Web, **Dashboard TV Kiosk 16:9 tại xưởng** (dùng Kiosk Token chỉ đọc) và Chatbot.
- **Báo cáo năng suất & Lương khoán minh bạch:** Tính năng suất theo giờ công thực tế (nguồn công thức nền: [VTI Solutions](https://vti-solutions.vn/cong-thuc-tinh-nang-suat-lao-dong/)); tự động tổng hợp kỳ lương khoán cho Giám đốc/Admin duyệt và cho Công nhân xem phiếu lương cá nhân chỉ đọc.
- **Chatbot AI (CommandCode Provider API):** Trả lời dựa trên dữ liệu hệ thống **và tài liệu SOP quy chế xưởng**; hỗ trợ tool calling gồm bộ công cụ tra cứu theo vai trò và **1 thao tác nghiệp vụ an toàn có xác nhận** (cập nhật trạng thái xử lý cảnh báo).

## 3. Giá trị đo được tại đơn vị (khung đo trước / sau khảo sát & UAT)

1. **Rút ngắn thời gian tra cứu tiến độ đơn hàng / chuyền:** Chuyển từ hỏi miệng / tổng hợp sổ cuối ngày sang tra cứu tức thời trên Web hoặc hỏi Chatbot.
2. **Phát hiện sớm nguy cơ trễ ngay trong ca/ngày:** Cảnh báo tự động khi chuyền hụt kế hoạch ngày hoặc chậm nhập sản lượng giờ trên Web và TV xưởng thay vì đợi đến sát ngày giao hàng.
3. **Chuẩn hóa số liệu năng suất & đối soát lương khoán:** Có báo cáo năng suất chuyền theo ngày/tuần và bảng lương khoán khớp 100% với sản lượng đạt chuẩn đã khóa.

*(Số liệu baseline trước triển khai và kết quả thực đo sẽ được ghi nhận qua khảo sát tuần 1 và đợt UAT tuần 6–7 tại đơn vị thực tế, không tự gán số liệu giả định).*

## 4. Định hướng phạm vi & lộ trình dữ liệu trong học kỳ

- **Chức năng:** Hoàn thiện đầy đủ MVP + 3 hạng mục mức Tốt trên Web công khai (**Render**); không huấn luyện mô hình ML; không mở rộng sang quản lý kho nguyên phụ liệu, QC phân tích mã lỗi phức tạp, IoT máy may hay Mobile App native.
- **Dữ liệu & Áp dụng thực tế:** Giai đoạn phát triển (Sprint 1–2) sử dụng bộ dữ liệu seed mô phỏng chuẩn hóa; giai đoạn kiểm thử chấp nhận UAT (tuần 6–7) nạp dữ liệu mẫu khảo sát từ xưởng thực tế đã **ẩn danh thông tin cá nhân (PII)** để người dùng thật trải nghiệm và nghiệm thu.
