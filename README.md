### **1\. Sơ đồ luồng IPO (Input – Process – Output)**

* **Input (Dữ liệu đầu vào):**  
  * Toàn bộ danh sách tệp tin và thư mục hỗn độn nằm bên trong thư mục Downloads/ của máy chủ trang trại.  
* **Process (Xử lý logic):**  
  * Khởi tạo môi trường: Kiểm tra và tạo sẵn thư mục media/ và scripts/ (nếu chưa có).  
  * Quét từng đối tượng (item) trong Downloads/.  
  * **Kiểm tra an toàn:** Lọc bỏ các thư mục, chỉ chọn tệp tin thông thường (\-f) để tránh lỗi "Is a directory".  
  * Phân loại tệp dựa vào phần mở rộng (Extension):  
    * Đuôi .png, .jpg $\rightarrow$ Chuẩn bị chuyển vào media/.  
    * Đuôi .py $\rightarrow$ Chuẩn bị chuyển vào scripts/.  
    * Đuôi .tmp $\rightarrow$ Chuẩn bị xóa (rm).  
  * Xử lý xung đột đích: Kiểm tra xem tại thư mục đích đã tồn tại tệp/thư mục trùng tên hay chưa để tránh lỗi hệ thống.  
* **Output (Đầu ra / Hành động):**  
  * Tệp ảnh được lưu trữ ngăn nắp trong media/.  
  * Tệp mã nguồn được gom gọn gàng trong scripts/.  
  * Các tệp tạm rác bị xóa vĩnh viễn, giải phóng dung lượng bộ nhớ.

### **2\. Xử lý tình huống biên (Edge Case: "Is a directory")**

* **Nguyên nhân lỗi:** Lỗi Is a directory xảy ra khi Robot dùng lệnh mv hoặc rm nhầm vào một **thư mục** thay vì **tệp tin**, hoặc khi đường dẫn đích đã tồn tại một thư mục có tên trùng với tệp chuẩn bị di chuyển.  
* **Cách giải quyết của Robot:**  
  * Sử dụng toán tử kiểm tra tệp thông thường \[ \-f "\$item" \] trong vòng lặp để lọc tuyệt đối các thư mục ra khỏi danh sách thao tác.  
  * Sử dụng cờ \-n (no clobber) của lệnh mv kết hợp cơ chế chặn lỗi ngầm (2\>/dev/null) để Robot không bị crash (dừng đột ngột) khi gặp xung đột tên trùng lặp ở đích.

### **3\. Danh sách câu lệnh kịch bản cho Robot**

mkdir media scripts  
mv \*.png media/  
mv \*.jpg media/  
mv \*.py scripts/  
rm \*.tmp

