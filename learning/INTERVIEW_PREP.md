# INTERVIEW PREP

> Mỗi phase: 15 câu kỹ thuật · 5 tình huống debugging · 3 bài coding · 1 mock interview · 1 bài giải thích kiến trúc. Câu hỏi để **tự trả lời thành lời** (nói to hoặc viết), không tra đáp án trước. Mock interview chấm theo thang 0/1/2 mỗi câu, quy về 100; đạt phase khi ≥ 85.
> Vị trí mục tiêu: AI/CV Engineer · ADAS Perception · 3D Perception · AD Software Engineer · Robotics Perception.

## Kết quả mock interview

| Phase | Ngày | Điểm | Điểm mạnh | Cần cải thiện |
|---|---|---|---|---|
| 1 | – | – | – | – |

---

## Phase 1 — Engineering Foundation

### 15 câu kỹ thuật
1. Vì sao NumPy vectorization nhanh hơn vòng lặp Python? Khi nào vectorization **không** giúp ích?
2. Broadcasting hoạt động thế nào? Cho một ví dụ broadcasting gây bug âm thầm.
3. View và copy trong NumPy khác nhau thế nào? Khi nào sửa một slice làm thay đổi mảng gốc?
4. `git merge` vs `git rebase` — khi nào dùng cái nào trong team?
5. `const&` trong C++ giải quyết vấn đề gì? Khi nào nên truyền by value rồi `std::move`?
6. RAII là gì? Liên quan gì đến tài nguyên như file, GPU memory, mutex?
7. Tính chất của ma trận xoay; vì sao `R⁻¹ = Rᵀ`?
8. Euler angles, quaternion, rotation matrix — ưu/nhược từng loại; gimbal lock là gì?
9. Giải thích quy ước `T_a_b` và cách ghép chuỗi transform. Nghịch đảo T 4×4 thế nào?
10. Covariance 2D biểu diễn hình học ra sao? Ý nghĩa phần tử ngoài đường chéo?
11. Áp dụng định lý Bayes cho detector có base rate thấp — kết luận thiết kế?
12. Topic, service, action trong ROS 2 — chọn cái nào cho: stream LiDAR, đổi tham số, "lái tới điểm X"?
13. QoS reliability/durability là gì? Vì sao sensor data thường dùng best-effort?
14. TF2 giải quyết vấn đề gì? Static vs dynamic transform? Lỗi "extrapolation into the future" nghĩa là gì?
15. Vì sao timestamp message phải là thời điểm cảm biến chụp? Hậu quả nếu dùng thời điểm xử lý? _(nối Phase 2–6)_

### 5 tình huống debugging
1. `ros2 topic echo` thấy dữ liệu nhưng node của bạn không nhận được message nào. Bạn điều tra thế nào?
2. Point cloud hiển thị trong RViz bị lệch và xoay so với ảnh, nhưng chỉ khi xe rẽ.
3. Test transform pass trên máy bạn nhưng fail trên CI với sai số 1e-7. Nguyên nhân và cách viết test đúng?
4. Script xử lý tăng RAM dần qua từng frame rồi bị kill. Bạn tìm nguyên nhân thế nào?
5. Sau một loạt commit, metric giảm 10% mà không ai biết vì sao. Quy trình tìm commit gây lỗi?

### 3 bài coding
1. Vectorized: cho `points (N,3)` và `T (4,4)`, trả về điểm đã transform; viết luôn `invert_T`.
2. Cho các timestamp camera (30 Hz) và LiDAR (10 Hz) dạng mảng đã sắp xếp, với mỗi scan LiDAR tìm ảnh có timestamp gần nhất trong ngưỡng 20 ms — O(N + M).
3. Voxel downsample point cloud (lấy trung bình mỗi voxel) bằng NumPy, không vòng lặp Python.

### Giải thích kiến trúc (3–5 phút)
"DriveStack Lab sau Phase 1": data flow từ KITTI → replay → topics/TF; quy ước frame, đơn vị, timestamp; cách test; điều gì còn là stub và vì sao.

---

## Phase 2–6

_(Mở khi tới phase — mentor bổ sung theo cùng cấu trúc.)_
