# SKILL MATRIX — Competency Map Level 1 → Level 3

> Bước 1 của master prompt. Cột **Hiện tại** chỉ được cập nhật khi có bằng chứng (test/log/bài chấm). Hiện tất cả là **L0 — chưa đánh giá**: không mặc định bạn đã vững kỹ năng nào.

## Định nghĩa Level

| Level | Tên | Bạn làm được gì | Bằng chứng điển hình |
|---|---|---|---|
| **L1** | Hiểu & chạy được | Giải thích đúng khái niệm, chạy tool/pipeline có sẵn, đọc hiểu output | Trả lời quiz, chạy được demo theo README |
| **L2** | Tự cài đặt & debug | Tự cài bản tối giản, dùng thư viện đúng quy ước, viết test, debug lỗi thường gặp, đo metric | Code [TỰ VIẾT] + test xanh + bài debugging |
| **L3** | Tích hợp & cải thiện | Tích hợp vào hệ thống với interface/frame/timestamp đúng, phân tích failure modes, đo latency, đề xuất & kiểm chứng cải tiến, giải thích trade-off | Module trong DriveStack Lab + báo cáo metric + phân tích lỗi + phỏng vấn |

**Mục tiêu cuối:** L3 ở các dòng đánh dấu ★ (lõi cho vị trí Perception/AD Software), tối thiểu L2 ở các dòng còn lại — **ngoại lệ:** A8 Docker mục tiêu **L1** (Learn Later). Nếu cắt phạm vi theo `ROADMAP.md` §0.3, mục tiêu B6 và C6 hạ xuống L1 và phải ghi lại ở đây.

---

## A. Engineering

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| A1★ | Python & NumPy | Viết script, hàm, class; hiểu list/dict/array | Vectorization, broadcasting, type hints, pytest, logging | Thiết kế package, profiling (cProfile), tối ưu hot path | 1 | – | L0 | |
| A2 | C++ cho robotics | Đọc hiểu C++ hiện đại, biên dịch được | Viết class, STL, Eigen, CMake, const-ref, RAII | Node ROS 2 C++, đo hiệu năng so với Python | 1 | – | L0 | |
| A3 | Linux & shell | Điều hướng, quyền, env vars, process | Script bash, `htop`/`nvidia-smi`, venv, apt | Tái lập môi trường (Docker/scripts), xử lý xung đột phiên bản | 1 | – | L0 | |
| A4 | Git | add/commit/push/branch | Merge, revert, `.gitignore`, commit có nghĩa | Workflow PR, bisect tìm commit gây lỗi | 1 | – | L0 | |
| A5★ | Testing & debugging | Chạy test, đọc traceback | Unit test, debugger (pdb/gdb), đọc log, cô lập lỗi | Integration/regression test, trace dữ liệu qua nhiều module | 1→6 | – | L0 | |
| A6★ | ROS 2 core | Node, topic, `ros2 topic echo` | Pub/sub, service, action, params, launch, rosbag2 | Thiết kế graph node, QoS, đo latency giữa node | 1 | – | L0 | |
| A7★ | TF2 & frames trong ROS | Hiểu cây frame | Publish static/dynamic TF, lookup đúng thời điểm | Debug lỗi extrapolation, sai frame trong pipeline đa cảm biến | 1, 4 | – | L0 | |
| A8 | Docker / reproducibility (mục tiêu L1) | Hiểu image/container | Viết Dockerfile chạy project | Môi trường GPU tái lập, pin phiên bản | 6 (Learn Later) | – | L0 | |

## Toán nền (dùng xuyên suốt)

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| M1★ | Đại số tuyến tính & 3D transforms | Vector, ma trận, tích vô hướng | Rotation matrix, quaternion, homogeneous 4×4, nghịch đảo, chuỗi transform | Debug lỗi frame trong hệ thống thật, chọn biểu diễn rotation phù hợp | 1 | – | L0 | |
| M2★ | Xác suất & ước lượng | Mean, variance, Gaussian | Covariance, Bayes, least squares, lan truyền sai số | Phân tích độ bất định của output, consistency (NEES) | 1, 4 | – | L0 | |

## B. Perception

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| B1★ | Camera model & projection | Hiểu pinhole, intrinsics | Tự project 3D→2D, undistort, IPM | Debug sai calibration qua triệu chứng trên ảnh | 2 | – | L0 | |
| B2★ | Camera calibration | Chạy calib theo tutorial | Tự calib checkerboard, đọc reprojection error | Đánh giá chất lượng calib, ảnh hưởng tới fusion | 2 | – | L0 | |
| B3 | Image processing (OpenCV) | Đọc/ghi/hiển thị, filter | Edge, feature, homography, color space | Pipeline tiền xử lý có đo latency | 2 | – | L0 | |
| B4★ | Deep learning (PyTorch, CNN) | Hiểu CNN, loss, epoch | Viết training loop, dataset/dataloader, chẩn đoán overfit | Fine-tune có kiểm soát thí nghiệm, phân tích lỗi theo lớp | 2 | – | L0 | |
| B5★ | 2D detection (YOLO) | Chạy inference | Fine-tune trên subset, tự tính IoU/NMS/mAP | Tích hợp node, đo latency/accuracy trade-off, phân tích FN/FP | 2 | – | L0 | |
| B6 | Segmentation | Hiểu semantic vs instance | Chạy/fine-tune model nhẹ, tính mIoU | Dùng mask cho drivable area/lane trong pipeline | 2 | – | L0 | |
| B7★ | 2D multi-object tracking | Hiểu track/ID | Tự cài SORT (KF + Hungarian) | Tuning, đo HOTA/IDF1, phân tích ID switch | 2 | – | L0 | |
| B8★ | Point cloud processing | Hiểu x,y,z,intensity; xem trong Open3D | ROI, voxel downsample, ground removal, clustering, kd-tree | Pipeline LiDAR classic chạy realtime trên replay | 3 | – | L0 | |
| B9★ | 3D bounding boxes & đánh giá | Hiểu 7-DoF box | Tự tính BEV IoU và 3D IoU, chuyển frame label KITTI | Đánh giá AP3D theo protocol, phân tích theo khoảng cách | 3 | – | L0 | |
| B10★ | LiDAR 3D detection (PointPillars/CenterPoint) | Chạy pretrained | Hiểu voxel/pillar/BEV, eval theo protocol, fine-tune ngắn (local hoặc Colab/remote) | So sánh trước/sau fine-tune và giữa model, phân tích failure (xa, che khuất, thưa) | 3 | – | L0 | |
| B11★ | 3D MOT | Hiểu khác biệt 2D vs 3D tracking | Tự cài AB3DMOT-style | Đo metric, tích hợp prediction | 3 | – | L0 | |
| B12★ | Camera–LiDAR calibration & fusion | Hiểu extrinsic | Project LiDAR lên ảnh, late fusion | Đánh giá lợi ích fusion, xử lý lệch thời gian | 3 | – | L0 | |

## C. Localization & Mapping

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| C1★ | Frames: map/odom/base_link/sensor | Kể tên, hiểu ý nghĩa | Dựng cây TF đúng REP-105 | Debug nhảy pose, sai frame trong localization | 1, 4 | – | L0 | |
| C2 | Odometry & motion models | Hiểu dead reckoning | Cài wheel odometry / motion model, đo drift | Phân tích nguồn drift | 4 | – | L0 | |
| C3 | IMU & GNSS | Hiểu dữ liệu, tần số | Xử lý bias/noise, chuyển LLA→ENU | Đồng bộ thời gian đa cảm biến | 4 | – | L0 | |
| C4★ | Kalman Filter / EKF | Hiểu predict/update | Tự cài KF, EKF với Jacobian | Fusion IMU+GNSS+odom, tuning Q/R, kiểm tra consistency | 2, 4 | – | L0 | |
| C5 | Scan matching (ICP/NDT) | Hiểu ý tưởng | Tự cài ICP point-to-point | LiDAR odometry, phân tích hội tụ | 4 | – | L0 | |
| C6 | SLAM cơ bản | Hiểu front-end/back-end, loop closure | Pose graph 2D nhỏ | Chạy SLAM có sẵn, đánh giá ATE/RPE | 4 | – | L0 | |

## D. Prediction, Planning & Control

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| D1★ | Vehicle model (kinematic bicycle) | Hiểu tham số | Tự mô phỏng | Biết giới hạn mô hình ở tốc độ cao | 5 | – | L0 | |
| D2★ | Control: PID, Pure Pursuit, Stanley | Hiểu vai trò P/I/D | Tự cài, tune, đo overshoot/tracking error | Xử lý delay, saturation, rate limit | 5 | – | L0 | |
| D3 | Search planning: Dijkstra, A*, RRT | Hiểu ý tưởng | Tự cài A*, RRT | Chọn thuật toán theo bài toán, đo complexity | 5 | – | L0 | |
| D4 | Motion prediction | Constant velocity | CV/CTRV baselines, ADE/FDE | Dùng prediction cho collision check, TTC | 5 | – | L0 | |
| D5★ | Behavior & trajectory planning | FSM, Frenet frame | Sinh quỹ đạo polynomial, cost function | Closed-loop trong sim, constraint violations | 5 | – | L0 | |

## E. System Integration

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| E1★ | Timestamp, clock, sync, latency | Hiểu capture time vs receive time; data/steady/system clock | Đồng bộ đa cảm biến; đo riêng processing duration, pipeline latency, data age | Latency p50/p95 end-to-end, drop rate, real-time factor; không trộn clock domain | 1→6 | – | L0 | |
| E2★ | Simulation & evaluation | Chạy sim | Kịch bản test, metric theo tầng; phân biệt open-loop replay vs closed-loop | Failure analysis có hệ thống, regression suite | 5, 6 | – | L0 | |
| E3★ | Contract dữ liệu giữa module (frame, ego-motion, staleness) | Đọc được schema, frame, đơn vị | Đưa track về frame planning có bù ego-motion và covariance | Thiết kế/kiểm thử contract liên module, phát hiện lỗi frame/thời gian qua test | 1, 5 | – | L0 | |

## F. Career Readiness

| # | Năng lực | L1 | L2 | L3 | Phase | Diagnostic | Hiện tại | Bằng chứng |
|---|---|---|---|---|---|---|---|---|
| F1★ | Giải thích kiến trúc end-to-end | Kể các module | Giải thích data flow, frame, timestamp | Trình bày trade-off, failure mode, hướng cải tiến trong 5 phút | 1→6 | – | L0 | |
| F2 | Đọc codebase & paper | Đọc README | Lần theo data flow trong repo lạ | Tóm tắt paper → cài thử ý tưởng chính | 2→6 | – | L0 | |
| F3★ | Phỏng vấn Fresher/Junior | – | ≥ 70/100 mock interview | ≥ 85/100 mock interview mỗi phase | 1→6 | – | L0 | |

---

## Ánh xạ năng lực ★ → sản phẩm → gate

Mỗi Level chỉ được nâng khi sản phẩm (artifact) tương ứng có bằng chứng và gate liên quan đạt.

| Năng lực | Sản phẩm chứng minh L3 | Gate |
|---|---|---|
| A1, A5 | Package `drivestack` + test suite unit/integration/regression | M1.1, M1.7, mọi phase gate |
| A6, A7, C1 | Replay KITTI Raw + cây TF + TF `map→odom→base_link` | M1.6, M1.7, M4.1 |
| M1 | `core/transforms.py` + test; debug lỗi frame trong fusion | M1.3, M3.6 |
| M2, C4 | KF (M2.7) → EKF có NEES Monte Carlo | M1.4, M2.7, M4.4 |
| B1, B2 | Projection tự viết + calib camera thật | M2.1, M2.2 |
| B4, B5 | Detector fine-tune + report AP/mAP theo protocol | M2.4, M2.5 |
| B7 | SORT tự viết + HOTA/IDF1 | M2.7 |
| B8 | Pipeline classic ground + cluster | M3.2 |
| B9, B10 | BEV/3D IoU tự viết; PointPillars eval + fine-tune | M3.3, M3.4 |
| B11, B12 | Detection-level fusion + 3D MOT | M3.6, M3.5 |
| D1, D2 | Controller ổn định nhiều tốc độ trong sim 2D | M5.1, M5.2 |
| D5, E3 | `ObjectState` contract + Frenet planner closed-loop | M5.0, M5.5, M5.6 |
| E1, E2 | Báo cáo latency 3 loại + scenario tests | M2.8, M6.2, M6.3 |
| F1, F3 | Giải thích kiến trúc 5 phút + mock interview | Mọi phase gate, M6.4 |

---

_Cập nhật lần cuối: 2026-11-04 — đồng bộ với roadmap sửa đổi (A8 L1, B9/B10, E1, E3, bảng ánh xạ); chưa có dữ liệu đánh giá._
