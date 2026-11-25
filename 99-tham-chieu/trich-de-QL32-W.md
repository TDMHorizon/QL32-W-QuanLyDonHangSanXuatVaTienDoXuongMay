# Trích đề tài 94 / QL32-W

Nguồn: `BM01a - De bai do an QLDA CNTT (MAN104) v1.pdf` — Lĩnh vực 32. Sản xuất.

## Nội dung đề

**Tên:** Website quản lý đơn hàng sản xuất và tiến độ xưởng may tích hợp chatbot AI  

**Mã:** QL32-W  
**Nền tảng:** Web app (front-end + back-end API)  

**Chatbot AI (bắt buộc):** Hỏi đáp tiến độ đơn hàng sản xuất.  

**Áp dụng thực tế:** Xưởng may, nhà máy/xưởng sản xuất nhỏ – theo dõi sản lượng theo giờ; cảnh báo đơn hàng trễ.  

**Bài toán:** Phòng kế hoạch nhận đơn, lập kế hoạch theo chuyền; tổ trưởng cập nhật sản lượng; giám đốc xem tiến độ.  

**Chức năng MVP:** Đơn hàng & mã hàng; Kế hoạch sản xuất theo chuyền; Cập nhật sản lượng theo giờ; Tiến độ & cảnh báo trễ; Báo cáo năng suất.  

**Để đạt mức Tốt:** Dashboard trên TV tại xưởng; Tính lương khoán; chatbot thao tác nghiệp vụ qua API (tool calling).  

**Demo hoàn thiện:** đường dẫn web công khai, đủ MVP và chatbot AI, dùng thử tại đơn vị.  

**Quản lý dự án:** GitHub, Jira, MS Project; D01 – D27; làm hay mua: dịch vụ LLM, Hosting; rủi ro: sản lượng không được nhập kịp thời; đơn hàng biến động.  

**CLO:** CLO2 (Bài 2–6), CLO3 (Bài 7–9).

## Quyết định nhóm (không thuộc PDF)

Xem `00-ke-hoach/THONG-TIN-NGHIEP-VU.md` — MVP + Tốt, lộ trình seed mô phỏng → dữ liệu UAT ẩn danh PII, CommandCode, Render, SL giờ chuyền + SL ca công nhân, chốt khóa (Admin unlock có lý do), SOP + read-tools + 1 write-tool cảnh báo có xác nhận, Kiosk Token TV, v.v.
