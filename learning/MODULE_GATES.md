# MODULE GATES — tiêu chí qua từng module

> **Đạt = ≥ 85/100 VÀ không trượt tiêu chí cốt lõi nào.** Điểm dựa trên bằng chứng (test do mentor tự chạy, log, bài viết của bạn) — không tự đánh giá cảm tính.
> Rubric 100 điểm: Quiz bản chất 25 · Coding [TỰ VIẾT] 25 · Debugging 15 · Test & evaluation 15 · Integration 10 · Giới hạn & failure modes 10.
> Trượt → remediation plan (ghi trong `WEAKNESS_BACKLOG.md`), thi lại phần trượt.

## Bảng trạng thái

| Module | Trạng thái | Điểm | Ngày | Bằng chứng |
|---|---|---|---|---|
| M1.1 – M1.7 | Chưa mở | – | – | – |

_(Thêm một dòng cho mỗi module khi bắt đầu; trạng thái: Chưa mở · Đang học · Chờ gate · Đạt · Trượt → remediation · Gate-only (từ diagnostic))_

---

## Phase 1 — chi tiết

### M1.1 Python & NumPy
- **Quiz:** broadcasting, view vs copy, dtype (float32 vs float64, overflow uint8), mutable default, `is` vs `==`.
- **Coding:** (a) khoảng cách đôi một N×M không vòng lặp; (b) voxel downsample lấy trung bình theo ô bằng NumPy (`np.floor`, `np.unique(..., return_inverse=True)`).
- **Debug:** lỗi broadcasting `(N,)` vs `(N,1)` cho ra ma trận N×N âm thầm.
- **Integration:** package `drivestack.core` cài được (`pip install -e .`), `pytest` chạy từ gốc repo.
- **Cốt lõi:** ① hot path không vòng lặp Python; ② test có edge cases (rỗng, 1 phần tử, NaN).

### M1.2 Linux, Git, debugging
- **Quiz:** process & env vars, `PATH`, quyền file, `git revert` vs `reset`, `.gitignore` cho data.
- **Coding:** script `scripts/env_report.sh` in OS, GPU/VRAM, RAM, disk, phiên bản Python/thư viện.
- **Debug:** dùng `pdb`/`breakpoint()` tìm lỗi trong script cho sẵn; `git bisect` tìm commit làm hỏng test.
- **Cốt lõi:** ① dùng debugger (breakpoint, step, inspect) thay vì rải `print`; ② `git bisect` tìm đúng commit.

### M1.3 Linear algebra & 3D transforms
- **Quiz:** tính chất R, quaternion ↔ R, gimbal lock, quy ước `T_a_b`, active vs passive.
- **Coding:** `core/transforms.py`: `make_T`, `invert_T`, `transform_points`, `euler_to_R`, `R_to_quat`, `quat_to_R` + test round-trip.
- **Debug:** chuỗi transform nhân sai thứ tự; quaternion thứ tự `(w,x,y,z)` vs `(x,y,z,w)`.
- **Cốt lõi:** ① nghịch đảo T 4×4 bằng `Rᵀ` đúng; ② giải thích và tuân thủ quy ước `T_a_b`; ③ phát hiện lỗi thứ tự nhân từ triệu chứng.

### M1.4 Probability & estimation
- **Quiz:** Gaussian, covariance & ellipse, Bayes với base rate, tích hai Gaussian.
- **Coding:** least-squares fit + covariance tham số; lan truyền sai số polar → Cartesian (range-bearing LiDAR) bằng Jacobian và kiểm chứng Monte Carlo.
- **Debug:** covariance không đối xứng / không PSD do lỗi số.
- **Cốt lõi:** ① tính và giải thích covariance 2D bằng hình học; ② áp dụng Bayes đúng với base rate.

### M1.5 C++ cho robotics
- **Quiz:** value/ref/const-ref, RAII, `unique_ptr`, copy vs move, iterator invalidation.
- **Coding:** class `PointCloud` dùng Eigen: transform + voxel downsample; build bằng CMake; test (GoogleTest hoặc assert).
- **Debug:** dangling reference sau `push_back`; lỗi link do thiếu `target_link_libraries`.
- **Cốt lõi:** ① tự viết `CMakeLists.txt` build được từ đầu; ② API nhận input lớn bằng `const&`.

### M1.6 ROS 2 Humble core
- **Quiz:** topic vs service vs action, QoS (reliability, durability, history), TF static vs dynamic, `use_sim_time`; ROS time vs system time vs steady time — dùng cái nào để đo thời lượng xử lý, cái nào để gắn vào dữ liệu.
- **Coding:** node Python pub/sub + parameter + launch file; một node C++; static TF; record/play rosbag2.
- **Debug:** QoS không khớp (best-effort vs reliable) → không nhận được message; lỗi TF extrapolation.
- **Cốt lõi:** ① tự chẩn đoán "không nhận được message" bằng `ros2 topic info -v`; ② publish TF đúng parent/child.

### M1.7 DriveStack skeleton (Milestone MS1)
- **Coding:** mở rộng package từ M1.1: `core/types.py` theo schema `PROJECT_ARCHITECTURE.md` (có `trace_id`); `io/kitti_raw.py`; node replay publish ảnh + cloud + OXTS + TF; `data/MANIFEST.md` ghi drive đã chốt + checksum.
- **Integration test:** replay drive KITTI Raw đã chốt → timestamp tăng đơn điệu, `frame_id` đúng, tần số đúng (`ros2 topic hz`), RViz hiển thị; đo processing duration của reader bằng steady clock.
- **Cốt lõi:** ① `stamp` = thời điểm cảm biến chụp (từ file timestamp KITTI), không trộn với clock máy; ② replay tái lập (cùng input → cùng output); ③ integration test xanh.

### M1.8 Thử khả thi simulator (timebox 6 h, sau gate M1.6)
- **Không chấm thang 100** — đây là spike kỹ thuật có biên bản.
- **Bằng chứng:** log VRAM/RAM/FPS sim/real-time factor trong 5 phút ở cấu hình nhẹ (synchronous, fixed timestep); xác nhận `VehicleControl` làm xe di chuyển và quan sát kế tiếp thay đổi.
- **Cốt lõi:** ① `reports/m1_8_sim_feasibility.md` chốt **một** đích capstone (đầy đủ-local / đầy đủ-remote / dự phòng) kèm số liệu; ② nếu dự phòng: ghi rõ capstone sẽ không là full sensor-to-control closed-loop.

---

## Phase 2–6 — tiêu chí cốt lõi (chi tiết mở khi tới phase)

| Module | Tiêu chí cốt lõi |
|---|---|
| M2.1 Camera geometry | Tự project 3D→2D khớp `cv2.projectPoints` (< 1e-6 px); lọc điểm sau camera |
| M2.2 Calibration | Calib camera thật, reprojection RMS < 0.5 px và giải thích vì sao chưa đủ |
| M2.3 Image processing | Homography/IPM đúng trên ảnh đường |
| M2.4 Deep learning | Tự viết training loop; chẩn đoán over/underfit từ đường loss; split chống leakage |
| M2.5 2D detection | Split 3712/3769 (tập con cố định) + class mapping/ignore ghi trong report; tự viết IoU/NMS/AP khớp evaluator; báo cáo ghi rõ AP KITTI vs mAP COCO |
| M2.6 Segmentation | mIoU đúng trên KITTI Semantic (split 160/40, mapping lớp cố định); giải thích lỗi theo class |
| M2.7 KF + 2D MOT | Tự cài KF tuyến tính + SORT; HOTA/IDF1 bằng TrackEval trên sequence KITTI Tracking tách riêng khỏi tập tune; giải thích ID switch |
| M2.8 Camera node | Node chạy trên replay; báo cáo **riêng** processing duration (steady), pipeline latency (trace_id) và data age (stamp) + replay rate; regression Phase 1 xanh |
| M3.1 Point cloud | Đọc/hiển thị KITTI `.bin`; giải thích mật độ theo khoảng cách bằng số liệu |
| M3.2 Preprocessing | Ground removal + cluster tự viết, so với Open3D |
| M3.3 3D boxes | Đổi label KITTI camera ↔ LiDAR round-trip đúng; tự viết **BEV IoU và 3D IoU** (BEV × chồng lấn chiều cao), khớp thư viện |
| M3.4 3D detection | Eval PointPillars theo protocol (AP3D/BEV R40 theo khoảng cách); **một lần fine-tune ngắn** (local hoặc Colab/remote) có so sánh trước/sau; giải thích pillars/BEV trade-off |
| M3.6 Fusion (học trước M3.5) | Project LiDAR lên ảnh đúng; late fusion ở mức detection có đánh giá so với LiDAR-only; xử lý lệch thời gian |
| M3.5 3D MOT | Tự cài 3D MOT trên detection đã fuse; tracking trong frame thế giới khi có pose; metric 3D theo evaluator đã chốt; failure cases |
| M3.7 LiDAR node | RViz markers phân biệt provenance; latency 3 loại theo `PROJECT_ARCHITECTURE.md` §3.1 |
| M4.1 Frames | Cây TF map→odom→base_link→sensors đúng REP-105 |
| M4.2 Odometry | Motion model tự viết; đo drift theo quãng đường |
| M4.3 IMU/GNSS | LLA→ENU đúng; xử lý bias |
| M4.4 EKF | (a) Dữ liệu tổng hợp (state thật + đo độc lập): EKF tự viết có Jacobian; **NEES trung bình qua ≥ 50 lần Monte Carlo nằm trong khoảng tin cậy 95% chi-square** (ghi N, bậc tự do, cách tổng hợp). (b) KITTI: bảng input / reference / giả định tương quan; đo từ OXTS + nhiễu ghi *synthetic*, odometry tự sinh ghi `mock`. Giải thích tuning Q/R |
| M4.5 ICP | Tự cài ICP; phân tích hội tụ/khởi tạo |
| M4.6 SLAM | Pose graph 2D nhỏ tự giải; giải thích loop closure |
| M4.7 Localization node | ATE/RPE với evo **so với reference OXTS** (ghi rõ không phải GT tuyệt đối); kịch bản mất GNSS |
| M5.0 Object state | `ObjectState` trong `map` theo contract §4.1; 3 test bắt buộc xanh (vật đứng yên → v≈0 khi ego chạy & rẽ; vận tốc tuyệt đối đúng; object quá cũ bị loại); không dùng track pixel |
| M5.1 Bicycle model | Mô phỏng đúng; nêu giới hạn ở tốc độ cao |
| M5.2 Control | Pure Pursuit/Stanley + PID ổn định nhiều tốc độ; xử lý đơn vị, saturation, rate limit, delay |
| M5.3 Search planning | A*/RRT tự viết, có test tối ưu/không tìm thấy đường |
| M5.4 Prediction | CV/CTRV + ADE/FDE; TTC đúng |
| M5.5 Behavior/trajectory | Frenet planner tránh vật cản không vi phạm ràng buộc |
| M5.6 Closed loop | Lệnh điều khiển thay đổi trạng thái xe và quan sát kế tiếp; map/route từ file kịch bản; metric planning/control; regression xanh |
| M6.1 Môi trường capstone | Dựng đúng đích đã chốt ở M1.8; nếu CARLA: synchronous + fixed timestep + adapter `Route` từ `carla.Map` |
| M6.2 Full pipeline | Processing duration, pipeline latency p50/p95, data age, drop rate, real-time factor báo riêng; provenance đúng mọi output; đích dự phòng không được gọi là full closed-loop |
| M6.3 Scenario tests | ≥ 5 failure cases có phân tích nguyên nhân |
| M6.4 Portfolio | README tái lập được; giải thích kiến trúc 5 phút; mock interview ≥ 85 |
