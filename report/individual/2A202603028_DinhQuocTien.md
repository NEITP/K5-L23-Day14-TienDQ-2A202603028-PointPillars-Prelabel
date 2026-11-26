# Báo cáo Day 14 — Đinh Quốc Tiến — 2A202603028

- Mã học viên dùng với `--student`: `2A202603028`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr`, tăng ngưỡng từ mức gốc `0.1`.
- Vì sao: ảnh BEV tập A có nhiều cuboid dự đoán; tổng cộng `pred_A.json` có 103 box trên 3 frame, trong khi sau khi rà soát tôi giữ 36 box trong `ref_A.json`. Chênh lệch này khiến tôi nghiêng về khả năng box thừa đang chiếm ưu thế. Tôi dự đoán tăng `score_thr` sẽ loại được một phần box điểm thấp và giảm công xóa. Đây là dự đoán trước khi xem kết quả sweep; ngưỡng quá cao có thể làm sót vật thể.

## 2. Tập A: sửa tay và tune

- Cấu hình gốc: xóa 70, thêm 3, sửa 3; chi phí 25.5.
- `best_config.json`: xóa 20, thêm 12, sửa 2; chi phí 19.0.
- Tham số thay đổi: `score_thr` từ `0.1` lên `0.5`; `min_points` giữ `0`.
- Chi phí giảm khoảng 25.5% trên tập A. Ba giá trị `min_points` ở `score_thr=0.5` cùng đạt 19.0 nên sweep không phân biệt được lựa chọn tốt nhất giữa chúng; script chọn giá trị đầu tiên là `0`.
- Leave-one-out: bỏ `s01f49`, cấu hình chọn `score_thr=0.5, min_points=0`, chi phí 8.5 (oracle 6.0); bỏ `s02f11`, cùng cấu hình, chi phí 7.8 (oracle 7.7); bỏ `s03f48`, cấu hình chọn `score_thr=0.4, min_points=10`, chi phí 4.5 (oracle 2.7). Lựa chọn không hoàn toàn ổn định, nhất là trên `s03f48`, nên mức cải thiện có thể bị overfit với chỉ 3 frame.

## 3. Trước/sau (bước 8)

![So sánh frame s03f48](../../d14_2A202603028/compare/s03f48.jpg)

- `pp_compare.py` cho thấy chi phí tập A giảm 25.5 xuống 19.0, tức giảm 25%.
- Dự đoán ở mục 1 đúng về hướng: tăng `score_thr` làm giảm đáng kể box thừa. Tuy nhiên số box cần thêm tăng từ 3 lên 12, nên ngưỡng cao hơn cũng làm sót thêm vật thể. Mức cải thiện ròng trên ba frame là 6.5 điểm chi phí.

## 4. Tập B

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| 68 | 8 | 3 | 29.9 |

- Box thừa tốn công sửa nhiều nhất: cần xóa 68 box; chúng chiếm 20.4 điểm trong tổng chi phí 29.9.

## 5. Giới hạn

- Chỉ có 3 frame A để chọn cấu hình; leave-one-out cũng cho thấy cấu hình được chọn thay đổi theo frame.
- Chi phí là công thức ước lượng trên box trong phạm vi 50 m, không phải thời gian thao tác thực tế trên CVAT.

