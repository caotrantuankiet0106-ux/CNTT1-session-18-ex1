# BÁO CÁO: XỬ LÝ SỰ CỐ TRUY CẬP HỆ THỐNG GREENLOGISTICS

---

## Phần 1. Phân tích
* **Input / Output:** Input là tên miền `phong-dieu-phoi.greenlog.vn` $\rightarrow$ Output là địa chỉ IP của máy chủ.
* **Tiến trình DNS Lookup:** Trình duyệt tra cứu lần lượt qua *Browser Cache $\rightarrow$ OS Cache / Hosts File $\rightarrow$ DNS Resolver của nhà mạng $\rightarrow$ Hệ thống phân cấp DNS (Root $\rightarrow$ TLD `.vn` $\rightarrow$ Authoritative Server)* để tìm IP.
* **Hậu quả gõ nhầm (`grenlog.vn` thiếu chữ `e`):** Giống như tra nhầm danh bạ, hệ thống không tìm thấy bản ghi, trả về lỗi **`DNS_PROBE_FINISHED_NXDOMAIN`**. Lỗi này hoàn toàn do phía Client, không phải sập máy chủ.

---

## Phần 2. Triển khai
* **Phân biệt lỗi:** 
  * *Lỗi DNS:* Lỗi ở phía Client/Mạng, yêu cầu **chưa từng chạm tới** máy chủ.
  * *Lỗi 500:* Lỗi ở phía Server, yêu cầu đã kết nối thành công nhưng máy chủ gặp sự cố xử lý code/database bên trong.
* **Quy trình kiểm tra (F12):** Mở **DevTools $\rightarrow$ Network** $\rightarrow$ Tích *Disable cache* $\rightarrow$ Tải lại trang (F5). Nếu không có request hoặc báo lỗi tên miền nghĩa là chưa chạm server; nếu xuất hiện mã HTTP Status Code (`200`, `404`, `500`...) nghĩa là đã kết nối thành công tới server.

---

## Phần 3. Kiểm thử
* **Sửa đúng tên miền nhưng vẫn nhận mã `404 Not Found`:** 
  * Yêu cầu đã kết nối thành công tới máy chủ, nhưng **đường dẫn (URL / Route) hoặc tài nguyên** Nam đang truy cập không tồn tại trên server (hoặc bị sai đường dẫn).