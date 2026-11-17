# **BÀI TẬP: THIẾT KẾ GIẢI PHÁP PROMPTING ỨNG DỤNG THỰC TẾ CHO KỸ THUẬT PROMPTING NÂNG CAO (LOGIC & CHUỖI)**

## **1. Phân tích và thiết kế giải pháp (I/O)**

**Bối cảnh:** Cửa hàng bán hàng online cần tự động hóa quy trình tiếp nhận và xử lý đơn hàng nhằm giảm thời gian kiểm tra, hạn chế sai sót và tránh bỏ sót thông tin khách hàng.

**Input (Dữ liệu đầu vào):**

- Thông tin khách hàng: Họ tên, số điện thoại, địa chỉ.
- Thông tin đơn hàng: Tên sản phẩm, số lượng, đơn giá.
- Thông tin thanh toán: Phương thức và trạng thái thanh toán.

**Output (Kết quả đầu ra):**

- Thông tin đơn hàng đã được kiểm tra và chuẩn hóa.
- Tổng tiền đơn hàng.
- Trạng thái đơn hàng: Hợp lệ, cần bổ sung hoặc cần kiểm tra.
- Danh sách lỗi và hướng xử lý tương ứng.

**Giải pháp:** Sử dụng Prompt Chaining gồm 3 bước: kiểm tra dữ liệu, tính toán đơn hàng và tổng hợp kết quả. Mỗi bước nhận kết quả từ bước trước làm đầu vào. Nếu dữ liệu bị thiếu hoặc sai lệch, AI phải đánh dấu và chuyển sang bước xử lý phù hợp, không tự ý suy đoán.

## **2. Prompt thực thi**

**Prompt hoàn chỉnh:**

```
Bạn là trợ lý AI quản lý đơn hàng cho cửa hàng bán lẻ online.

Nhiệm vụ: Tiếp nhận, kiểm tra và xử lý thông tin đơn hàng theo 3 giai đoạn.

BƯỚC 1 - KIỂM TRA:
- Kiểm tra họ tên, số điện thoại, địa chỉ và thông tin sản phẩm.
- Nếu thiếu dữ liệu, ghi rõ trường bị thiếu.
- Nếu số lượng hoặc đơn giá không hợp lệ, đánh dấu lỗi.
- Không tự ý tạo hoặc sửa thông tin.

BƯỚC 2 - TÍNH TOÁN:
- Chỉ tính tổng tiền khi số lượng và đơn giá hợp lệ.
- Công thức: Tổng tiền = Số lượng × Đơn giá.
- Nếu dữ liệu không hợp lệ, ghi "Chưa thể tính toán".

BƯỚC 3 - TỔNG HỢP:
- Phân loại đơn hàng: HỢP LỆ, CẦN BỔ SUNG hoặc CẦN KIỂM TRA.
- Đưa ra hướng xử lý tương ứng.
- Giữ lại đầy đủ kết quả các bước để tránh đứt gãy ngữ cảnh.

Yêu cầu đầu ra:
1. Thông tin đơn hàng.
2. Kết quả kiểm tra.
3. Tổng tiền.
4. Trạng thái đơn hàng.
5. Hướng xử lý.

Trình bày ngắn gọn, rõ ràng bằng tiếng Việt.
Nếu thiếu dữ liệu, không được tự suy đoán.
```

## **3. Giải thích Prompt theo mô hình CREATE**

| Thành phần   | Nội dung áp dụng                                                            |
| ------------ | --------------------------------------------------------------------------- |
| C – Context  | AI đóng vai trò trợ lý quản lý đơn hàng online.                             |
| R – Role     | Trợ lý AI tiếp nhận và xử lý đơn hàng.                                      |
| E – Explicit | Quy định rõ 3 bước kiểm tra, tính toán và tổng hợp.                         |
| A – Action   | Kiểm tra dữ liệu, tính tiền và phân loại đơn hàng.                          |
| T – Tone     | Ngắn gọn, rõ ràng, chuyên nghiệp.                                           |
| E – Extras   | Không tự suy đoán dữ liệu thiếu, phải báo lỗi và giữ kết quả giữa các bước. |

## **4. Kết quả chạy thử**

**Dữ liệu đầu vào:**

- Họ tên: Nguyễn Văn An.
- Số điện thoại: 0901234567.
- Địa chỉ: (Để trống).
- Sản phẩm: Chuột không dây.
- Số lượng: 2.
- Đơn giá: 150.000 đồng.
- Thanh toán: Chưa thanh toán.

**Kết quả AI phản hồi:**

```
1. KIỂM TRA DỮ LIỆU:
- Họ tên: Hợp lệ.
- Số điện thoại: Đã cung cấp.
- Địa chỉ: Thiếu thông tin.
- Sản phẩm: Chuột không dây.
- Số lượng: 2 (Hợp lệ).
- Đơn giá: 150.000 đồng (Hợp lệ).

2. TÍNH TOÁN:
Tổng tiền = 2 × 150.000
          = 300.000 đồng.

3. KẾT QUẢ:
- Trạng thái: CẦN BỔ SUNG.
- Lý do: Khách hàng chưa cung cấp địa chỉ giao hàng.
- Hướng xử lý: Yêu cầu khách hàng bổ sung địa chỉ
trước khi xác nhận đơn hàng.
- Thanh toán: Chưa thanh toán.
```

## **5. Kết luận**

Giải pháp đã vận dụng Prompt Chaining để chia quy trình xử lý đơn hàng thành các bước logic, giúp AI thực hiện nhiệm vụ có hệ thống. Kết hợp với điều kiện xử lý lỗi giúp hạn chế việc tự suy đoán khi thiếu dữ liệu. Việc duy trì kết quả giữa các bước còn giúp hạn chế đứt gãy ngữ cảnh, đảm bảo thông tin được truyền tải nhất quán và chính xác.
