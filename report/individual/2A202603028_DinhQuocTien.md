# Báo cáo Day 14 — Đinh Quốc Tiến — 2A202603028

- Mã học viên dùng với `--student`: `2A202603028`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr`, tăng ngưỡng từ mức gốc `0.1`.
- Vì sao: ảnh BEV tập A có nhiều cuboid dự đoán; tổng cộng `pred_A.json` có 103 box trên 3 frame, trong khi sau khi rà soát tôi giữ 36 box trong `ref_A.json`. Chênh lệch này khiến tôi nghiêng về khả năng box thừa đang chiếm ưu thế. Tôi dự đoán tăng `score_thr` sẽ loại được một phần box điểm thấp và giảm công xóa. Đây là dự đoán trước khi xem kết quả sweep; ngưỡng quá cao có thể làm sót vật thể.

## 2. Tập A: sửa tay và tune

- Cấu hình gốc: xóa 67, thêm 3, sửa 3; chi phí 25.5.
- `best_config.json`: xóa 20, thêm 12, sửa 2; chi phí 19.0.
- Tham số thay đổi: `score_thr` từ `0.1` lên `0.5`; `min_points` giữ `0`.
- Chi phí giảm khoảng 25.5% trên tập A. Ba giá trị `min_points` ở `score_thr=0.5` cùng đạt 19.0 nên sweep không phân biệt được lựa chọn tốt nhất giữa chúng; script chọn giá trị đầu tiên là `0`.
- Leave-one-out: bỏ `s01f49`, cấu hình chọn `score_thr=0.5, min_points=0`, chi phí 8.5 (oracle 6.0); bỏ `s02f11`, cùng cấu hình, chi phí 7.8 (oracle 7.7); bỏ `s03f48`, cấu hình chọn `score_thr=0.4, min_points=10`, chi phí 4.5 (oracle 2.7). Lựa chọn không hoàn toàn ổn định, nhất là trên `s03f48`, nên mức cải thiện có thể bị overfit với chỉ 3 frame.

