# Báo cáo Day 14 — Đinh Quốc Tiến — 2A202603028

- Mã học viên dùng với `--student`: `2A202603028`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

- Tham số tôi nghĩ giảm công sửa nhiều nhất: `score_thr`, tăng ngưỡng từ mức gốc `0.1`.
- Vì sao: ảnh BEV tập A có nhiều cuboid dự đoán; tổng cộng `pred_A.json` có 103 box trên 3 frame, trong khi sau khi rà soát tôi giữ 36 box trong `ref_A.json`. Chênh lệch này khiến tôi nghiêng về khả năng box thừa đang chiếm ưu thế. Tôi dự đoán tăng `score_thr` sẽ loại được một phần box điểm thấp và giảm công xóa. Đây là dự đoán trước khi xem kết quả sweep; ngưỡng quá cao có thể làm sót vật thể.

## 2. Tập A: sửa tay và tune

Chưa chạy sweep.

