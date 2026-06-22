# BÀI 1: Phân tích & Lựa chọn (Thực hành thiết kế prompt tối ưu hóa mã nguồn)

## 1. Đáp án lựa chọn
**Phương án B** là phương án tối ưu nhất.

---

## 2. Phân tích chi tiết sự hiện diện của 5 thành phần Prompt trong Phương án B
Phương án B tuân thủ hoàn hảo mô hình **Role - Goal - Context - Constraint - Format** để định hướng đầu ra chính xác của AI:

*   **Vai trò (Role):** *"Hãy đóng vai trò là một Java Senior Developer."*
    *   *Ý nghĩa:* Giúp AI kích hoạt đúng vùng kiến thức chuyên môn về lập trình Java chất lượng cao, viết code sạch (clean code), dễ bảo trì và tối ưu hiệu năng.
*   **Mục tiêu (Goal / Task):** *"Nhiệm vụ của bạn là tái cấu trúc (refactor) logic rẽ nhánh lồng nhau phức tạp của class DiscountService trên thành các câu lệnh điều kiện bảo vệ (guard clauses / return sớm)."*
    *   *Ý nghĩa:* Xác định rõ hành động kỹ thuật cần làm là refactor và cấu trúc cụ thể cần hướng tới là "guard clauses / return sớm", thay vì để AI tự do tối ưu theo cách khác (như dùng design pattern phức tạp không cần thiết).
*   **Ngữ cảnh (Context):** Class `DiscountService` với đoạn mã nguồn Java chưa tối ưu được cung cấp kèm theo prompt.
    *   *Ý nghĩa:* Cung cấp mã nguồn thực tế để AI phân tích và áp dụng trực tiếp, tránh hiện tượng sinh mã chung chung hoặc ảo giác (hallucination).
*   **Ràng buộc (Constraint):** *"Giữ nguyên logic nghiệp vụ tính chiết khấu ban đầu, không thay đổi kiểu dữ liệu đầu vào/đầu ra, sử dụng Java 11."*
    *   *Ý nghĩa:* Đặt ra giới hạn an toàn để đảm bảo code chạy đúng nghiệp vụ cũ, không làm lỗi các phần code khác đang gọi đến class này (giữ nguyên kiểu dữ liệu), và chỉ sử dụng tính năng tương thích với Java 11.
*   **Định dạng (Format):** *"Trình bày kết quả dưới dạng khối mã nguồn Java hoàn chỉnh kèm giải thích ngắn bằng tiếng Việt."*
    *   *Ý nghĩa:* Giúp lập trình viên có thể sao chép trực tiếp code vào dự án mà không phải định dạng hay sửa thủ công, đồng thời hiểu nhanh ý đồ sửa đổi qua phần giải thích bằng ngôn ngữ mẹ đẻ.

---

## 3. Phân tích lý do loại trừ các phương án còn lại

### Phương án A: *"Tái cấu trúc code Java trên để nó đẹp hơn."*
*   **Nhược điểm:**
    *   **Quá mơ hồ và thiếu các thành phần quan trọng:** Thiếu Vai trò (Role), Ngữ cảnh (Context), Ràng buộc (Constraint) và Định dạng (Format).
    *   **Khái niệm "đẹp hơn" mang tính cảm quan:** AI có thể hiểu "đẹp hơn" theo nhiều cách khác nhau, ví dụ: chuyển sang sử dụng OOP Polymorphism (đa hình) với nhiều class con, hoặc viết code ngắn lại bằng toán tử ba ngôi lồng nhau phức tạp. Điều này làm tăng độ phức tạp không cần thiết của mã nguồn.
    *   **Nguy cơ lỗi:** Do không có ràng buộc về kiểu dữ liệu và nghiệp vụ ban đầu, AI có thể thay đổi signature của phương thức, thay đổi kiểu dữ liệu đầu vào/đầu ra (ví dụ: dùng `BigDecimal` thay vì `double`), hoặc bỏ qua các trường hợp biên của nghiệp vụ.

### Phương án C: *"Hãy sửa code Java này, bỏ bớt if-else lồng nhau đi và sử dụng cấu trúc Java Stream API để viết ngắn gọn nhất có thể."*
*   **Nhược điểm:**
    *   **Sai lệch về mặt kỹ thuật:** Java Stream API được thiết kế để xử lý dữ liệu dạng tập hợp (Collections/Streams) chứ không phải để xử lý các biến đơn lẻ (`type`, `amount`, `isHoliday`).
    *   **Code phức tạp và khó bảo trì hơn:** Việc cố ép Stream API vào đây (ví dụ: tạo list tạm rồi filter) sẽ làm mã nguồn trở nên cực kỳ "quái dị", khó đọc, làm giảm hiệu năng hệ thống (overhead do khởi tạo stream và lambda expression) và hoàn toàn đi ngược lại nguyên lý Clean Code.
    *   **Thiếu ràng buộc:** Thiếu vai trò cụ thể, thiếu giới hạn phiên bản Java và không chỉ rõ cấu trúc "guard clauses" dẫn đến việc AI có thể tối ưu sai hướng.

---

## 4. Minh chứng thực tế chạy thử nghiệm với AI

### Nội dung Prompt gửi AI (Áp dụng Phương án B)
```text
Hãy đóng vai trò là một Java Senior Developer. Nhiệm vụ của bạn là tái cấu trúc (refactor) logic rẽ nhánh lồng nhau phức tạp của class DiscountService dưới đây thành các câu lệnh điều kiện bảo vệ (guard clauses / return sớm). 

Ràng buộc: 
- Giữ nguyên logic nghiệp vụ tính chiết khấu ban đầu.
- Không thay đổi kiểu dữ liệu đầu vào/đầu ra.
- Sử dụng Java 11. 

Trình bày kết quả dưới dạng khối mã nguồn Java hoàn chỉnh kèm giải thích ngắn bằng tiếng Việt.

Mã nguồn cần refactor:
public class DiscountService {
    public double calculateDiscount(String type, double amount, boolean isHoliday) {
        if (type.equals("VIP")) {
            if (amount > 1000) {
                if (isHoliday) {
                    return amount * 0.25;
                } else {
                    return amount * 0.20;
                }
            } else {
                return amount * 0.15;
            }
        } else {
            if (amount > 500) {
                if (isHoliday) {
                  return amount * 0.10;
                } else {
                    return amount * 0.05;
                }
            }
        }
        return 0;
    }
}
```

### Phản hồi của AI
```java
public class DiscountService {

    public double calculateDiscount(String type, double amount, boolean isHoliday) {
        // Sử dụng "VIP".equals(type) để tránh lỗi NullPointerException nếu biến type truyền vào là null
        if ("VIP".equals(type)) {
            if (amount > 1000) {
                return isHoliday ? amount * 0.25 : amount * 0.20;
            }
            return amount * 0.15;
        }

        // Logic xử lý cho khách hàng thông thường (Không phải VIP)
        if (amount > 500) {
            return isHoliday ? amount * 0.10 : amount * 0.05;
        }

        // Trường hợp mặc định khi không thỏa mãn các điều kiện trên
        return 0;
    }
}
```

**Giải thích các cải tiến (tiếng Việt):**
1. **Áp dụng Guard Clauses (Return sớm):** Nhánh xử lý `"VIP"` và nhánh xử lý khách hàng thông thường được tách biệt rõ ràng. Khi điều kiện khớp, giá trị lập tức được trả về giúp giảm số cấp thụt lề (indentation levels) từ 4 cấp xuống tối đa 2 cấp.
2. **An toàn với Null Pointer:** Thay đổi từ `type.equals("VIP")` sang `"VIP".equals(type)`. Cách viết này bảo vệ hệ thống khỏi ngoại lệ `NullPointerException` nếu tham số `type` là `null`.
3. **Toán tử 3 ngôi (Ternary Operator) hợp lý:** Thay vì viết `if-else` lồng nhau chỉ để trả về 2 trường hợp ngày lễ/ngày thường (`isHoliday`), việc dùng toán tử 3 ngôi giúp code cực kỳ ngắn gọn và dễ hiểu.
4. **Bảo toàn nghiệp vụ và kiểu dữ liệu:** Toàn bộ logic chiết khấu của VIP (25%, 20%, 15%) và khách thường (10%, 5%, 0%) được giữ nguyên chính xác 100%. Signature của phương thức không thay đổi.
