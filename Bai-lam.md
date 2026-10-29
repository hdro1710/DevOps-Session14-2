# 4. Hiển thị dung lượng RAM trống theo MB/GB trên Grafana

## 4.1. Câu truy vấn PromQL

Sử dụng truy vấn sau:

```promql
node_memory_MemFree_bytes
```

Metric `node_memory_MemFree_bytes` trả về dung lượng RAM hoàn toàn chưa được sử dụng, với đơn vị gốc là **Bytes**.

Không cần thực hiện phép nhân hoặc chia trong câu truy vấn. Grafana có thể tự chuyển đổi và định dạng giá trị dựa trên đơn vị được cấu hình cho panel.

## 4.2. Cấu hình Unit trên Grafana

Thực hiện các bước sau:

1. Mở Dashboard và chọn panel RAM cần chỉnh sửa.
2. Chọn **Edit** để mở giao diện chỉnh sửa panel.
3. Tại thanh cấu hình bên phải, tìm mục **Standard options**.
4. Tìm trường **Unit**.
5. Chọn đơn vị phù hợp:
   - **Data → bytes (IEC)**: Hiển thị theo hệ nhị phân, thường tự định dạng thành KiB, MiB hoặc GiB.
   - **Data → bytes (SI)**: Hiển thị theo hệ thập phân, thường tự định dạng thành kB, MB hoặc GB.
6. Có thể đặt **Decimals** thành `2` để kiểm soát số chữ số thập phân.
7. Nhấn **Apply**, sau đó lưu Dashboard bằng **Save dashboard**.

Tên hoặc vị trí các tùy chọn có thể hơi khác nhau tùy phiên bản Grafana.

## 4.3. Giải thích bẫy trong bài thực hành

Metric có hậu tố `_bytes` biểu thị dữ liệu đầu vào tính bằng Bytes. Nếu chỉ sử dụng truy vấn mà không cấu hình Unit, Grafana có thể hiển thị một số nguyên lớn khó đọc.

Ví dụ minh họa:

- Giá trị gốc: `1073741824` Bytes.
- Nếu chọn **bytes (IEC)**: Grafana hiển thị khoảng `1 GiB`.
- Nếu chọn **bytes (SI)**: Grafana hiển thị khoảng `1.07 GB`.

Các giá trị trên chỉ nhằm minh họa sự khác biệt giữa hai hệ đơn vị.

**Điểm quan trọng:** Chuyển đổi đơn vị chỉ thay đổi cách Grafana trình bày dữ liệu, không làm thay đổi giá trị gốc mà Prometheus thu thập.

## 4.4. Kết quả mong muốn

Sau khi hoàn thành:

- Câu truy vấn PromQL vẫn giữ nguyên: `node_memory_MemFree_bytes`.
- Không cần viết phép chia cho `1024`, `1024²` hoặc `1000²`.
- Giá trị được trình bày bằng đơn vị dễ đọc trên panel Grafana.
- Có thể điều chỉnh số chữ số thập phân thông qua tùy chọn **Decimals**.

## 4.5. Lưu ý kỹ thuật

`MemFree` chỉ biểu thị lượng RAM hoàn toàn chưa được sử dụng. Trên Linux, một phần RAM có thể được sử dụng làm cache nhưng vẫn có thể cấp phát lại khi cần.

Nếu mục tiêu là theo dõi lượng RAM mà hệ thống ước tính có thể cấp cho ứng dụng, nên cân nhắc sử dụng:

```promql
node_memory_MemAvailable_bytes
```

Trong trường hợp này, vẫn có thể chọn **Unit → Data → bytes (IEC/SI)** để Grafana tự định dạng giá trị.
