# Báo cáo Day 14 — Nguyễn Thanh Sơn — 2A202603318

> Sao chép file này thành `report/individual/<MSSV>_<HoTen>.md` (không dấu), ví dụ `2A20260000_NguyenVanAn.md`.

- Mã học viên dùng với `--student`: `2A202603318`
- `seed_tag` trong `frames_A.txt`: `d23d5cbcd4`

## 1. Dự đoán trước sweep (bước 5b)

Ghi **trước** khi chạy `scripts/pp_sweep.py`, rồi commit và push ngay (lịch sử commit là bằng chứng thời điểm viết). Không sửa phần này sau khi có kết quả.

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr` (tăng ngưỡng độ tin cậy lên khoảng `0.3` – `0.4`).
- Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?): Quan sát 3 ảnh BEV và camera chiếu góc của tập A (`s01f28`, `s04f21`, `s05f37`), lỗi chủ yếu là **box thừa (False Positives) quá nhiều** so với box sót. Cụ thể ở hai frame `s01f28` (29 box) và `s05f37` (43 box), mô hình sinh ra hàng loạt cuboid giả (`Car` màu đỏ và `Cyclist` màu xanh lá) đè lên dải phân cách trồng cây bụi, gốc cây và lề đường, trong khi các xe thật chạy trên làn đường hầu hết đều đã được bắt trúng. Vì các bụi cây ven đường có mật độ điểm LiDAR khá dày (nên `min_points` khó lọc hết) nhưng độ tự tin dự đoán của mô hình thường thấp, việc tăng `score_thr` sẽ loại bỏ trực tiếp nhóm box nhiễu này, giúp giảm mạnh thao tác xóa tay khi gán nhãn.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_<ID>/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_<ID>/frames_B.txt --config d14_<ID>/best_config.json --gt d14_<ID>/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
