# ROADMAP — tới L3 năng lực thực hành theo Skill Matrix

> Bước 3 của master prompt. "Level 3" ở đây là **L3 năng lực thực hành** như định nghĩa trong `SKILL_MATRIX.md` (tích hợp & cải thiện), không phải SAE Level 3.
> Đây là **khung chưa cá nhân hóa**. Sau khi chấm `DIAGNOSTIC_TEST.md`, giờ từng module được ước lượng lại. Không phase nào kết thúc vì "hết thời gian" — chỉ kết thúc khi qua Phase Gate.
> Sửa đổi 2026-11-04 theo `ROADMAP_REVIEW.md` (ngân sách giờ, protocol dữ liệu, kiểm tra CARLA sớm, contract tracks → planning, đo latency).

## 0. Tổng quan

### 0.1 Ngân sách giờ — 24 tuần là mục tiêu **có điều kiện**

Giờ module **đã bao gồm**: lý thuyết, bài tập, debugging, cài đặt môi trường của module đó, tích hợp và thi gate. **Chưa bao gồm** dự phòng (làm lại bài, remediation, lỗi tích hợp ngoài dự kiến).

| Phase | Nội dung | Giờ nội dung | Tuần @ 27,5 h (không dự phòng) | Milestone |
|---|---|---|---|---|
| 1 | Engineering Foundation (+ thử khả thi CARLA) | 116 | 4,2 | **MS1** — DriveStack skeleton replay KITTI Raw qua ROS 2, có TF + test; kết luận khả thi simulator |
| 2 | Camera Perception | 110 | 4,0 | **MS2** — Node camera: detection + tracking + metric theo protocol đã chốt |
| 3 | LiDAR & 3D Perception | 135 | 4,9 | **MS3** — Pipeline LiDAR 3D + fusion + 3D MOT, RViz |
| 4 | Localization & Mapping | 110 | 4,0 | **MS4** — EKF localization + ATE/RPE report |
| 5 | Prediction, Planning & Control | 118 | 4,3 | **MS5** — Closed-loop trong sim 2D: object state → prediction → planning → control |
| 6 | End-to-End | 85 | 3,1 | **MS6** — Capstone theo đích đã chốt ở M1.8 (đầy đủ hoặc dự phòng), báo cáo, portfolio |
| | **Tổng** | **674** | **24,5** | |

| Kịch bản | Quỹ giờ (25–30 h/tuần) | Điều kiện |
|---|---|---|
| Giữ đủ 674 h + 15% dự phòng (~775 h) | ~26–31 tuần | Mặc định nếu Diagnostic không rút được gì |
| **24 tuần** | 600–720 h | Diagnostic phải rút được khoảng **100 h** (gate-only/rút gọn) để còn chỗ cho 15% dự phòng |
| 18 tuần (fast track) | 450–540 h | Phải rút **~240 h** — chỉ khả thi nếu Diagnostic chứng minh vững phần lớn Phase 1 và một phần Phase 2. **Không chọn trước khi chấm.** |

15% dự phòng là giả định lập kế hoạch, sẽ thay bằng số thực sau 2 tuần học. **Không rút kiến thức nền chỉ để khớp lịch.**

### 0.2 Sổ giờ & ước lượng lại

Mỗi Weekly Review ghi vào bảng ở cuối file này giờ thực tế theo 4 loại: **Học mới · Gate/đánh giá · Tích hợp/sửa lỗi · Dự phòng/remediation**. Sau tuần 2 và mỗi phase gate: ước lượng lại ngày hoàn thành từ tốc độ thực (giờ thực / giờ kế hoạch của các module đã xong).

### 0.3 Phạm vi có thể cắt nếu trễ (sau khi đã dùng hết dự phòng)

Chỉ cắt từ danh sách này, theo thứ tự; mỗi lần cắt phải cập nhật `SKILL_MATRIX.md` và `MODULE_GATES.md` cho khớp.

| Cắt | Tiết kiệm | Ảnh hưởng mục tiêu |
|---|---|---|
| ByteTrack (giữ SORT) — M2.7 | ~4 h | Không ảnh hưởng ★ |
| Stanley (giữ Pure Pursuit + PID) — M5.2 | ~4 h | D2 vẫn đạt nếu Pure Pursuit đạt |
| So sánh CenterPoint chỉ ở mức khái niệm — M3.4 | ~6 h | B10 vẫn L3 qua PointPillars |
| Segmentation rút còn đánh giá model có sẵn — M2.6 | ~4 h | B6 mục tiêu → L1 |
| Pose graph SLAM chỉ khái niệm — M4.6 | ~10 h | C6 mục tiêu → L1 |

Ưu tiên giữ nguyên chiều sâu **Perception** (Phase 2–3) và khả năng **tích hợp** (M1.7, M5.0, M6.2) theo định hướng nghề nghiệp.

### 0.4 Nhịp tuần (25–30 giờ)

| Ngày | Việc | Giờ |
|---|---|---|
| T2–T6 | 1 buổi học A–J/ngày + Daily Check 20' | 4–5 h/ngày |
| T7 | Tích hợp DriveStack Lab + Weekly Review (ghi sổ giờ) | 4–5 h |
| CN | Nghỉ; tùy chọn 30' ôn cách quãng | 0–0,5 h |

**Lịch:** bắt đầu tuần 1 ngay sau khi chấm Diagnostic. ROS 2 Humble được hỗ trợ tới 5/2027; nếu lịch kéo dài qua mốc đó vẫn dùng được nhưng hết bản vá — ghi chú trong portfolio.

**Ràng buộc máy** (đo 2026-11-03): RTX 3050 **4 GB VRAM** (thấp hơn mức tối thiểu 6 GB của CARLA 0.9.15), RAM 16 GB, ổ trống ~63 GB. Chi tiết: `.claude/skills/adas-mentor/references/environment.md`.

**Phân loại phạm vi:** **[NOW]** Must Learn Now · **[LATER]** Learn Later · **[OPT]** Optional Research.

---

## 1. Kế hoạch dữ liệu & protocol đánh giá

Mỗi metric chỉ có nghĩa khi dữ liệu, split, nhãn và evaluator được chốt **trước** khi chạy. Bảng dưới là lựa chọn mặc định; ứng viên cụ thể (drive, sequence) được chốt và ghi vào `drivestack/data/MANIFEST.md` (nguồn, phiên bản, danh sách file, checksum, license) ở module tương ứng. KITTI dùng giấy phép **CC BY-NC-SA 3.0** — phù hợp mục đích học, không thương mại.

| Công việc (module) | Dữ liệu | Split / lựa chọn | Nhãn & quy tắc | Evaluator / metric |
|---|---|---|---|---|
| Replay, TF, OXTS (M1.7, M4.x) | **KITTI Raw** — 1 drive *synced+rectified* có tracklets (ứng viên `2011_09_26_drive_0005`, xác minh trên trang Raw Data) + calib | Toàn bộ drive | Timestamp từ file của từng cảm biến; OXTS là **reference** (GPS/IMU kết hợp), không phải ground truth tuyệt đối | Tần số topic, tính đơn điệu timestamp |
| Detection 2D (M2.5) | **KITTI Object 2D** (ảnh trái + label) | Split **3712/3769** theo chuẩn 3DOP/MV3D (tách theo drive, chống leakage); dùng tập con cố định (vd. 1500 train / 500 val, danh sách file trong repo) | Car, Pedestrian, Cyclist; `DontCare` = vùng ignore; Van/Truck/Person_sitting xử lý theo quy tắc devkit (không tính FP) | AP kiểu **KITTI** (R40, IoU 0,7 Car / 0,5 Ped-Cyc, Easy/Mod/Hard) **và** mAP kiểu **COCO** (mAP50-95) — báo cáo ghi rõ loại nào |
| Detection 3D (M3.3–M3.4) | **KITTI Object 3D** (velodyne + calib + label) | Cùng split 3712/3769, cùng tập con với 2D | Như trên; box ở frame camera rect → đổi sang LiDAR bằng code đã test | AP3D / AP_BEV R40, theo khoảng cách 0–20 / 20–40 / 40+ m |
| Tracking 2D & 3D (M2.7, M3.5) | **KITTI Tracking** training (21 sequence có track ID) | Tách **theo sequence**: vài sequence để tune, các sequence còn lại để đánh giá (danh sách cố định) | Car, Pedestrian; `DontCare` ignore | 2D: **TrackEval** (HOTA, IDF1, ID switches). 3D: evaluator của AB3DMOT (sAMOTA/AMOTA) — chốt khi tới M3.5 |
| Segmentation (M2.6) | **KITTI Semantic Segmentation** (200 ảnh train có pixel mask, định dạng Cityscapes) | 160 / 40 cố định | Mapping lớp → {road, sidewalk, vehicle, person, other}; lớp `void` ignore | mIoU theo lớp; ghi rõ tập đánh giá nhỏ nên chỉ mang tính tham khảo |
| Localization (M4.4–M4.7) | (a) **Dữ liệu tổng hợp** tự sinh (state thật + đo độc lập); (b) KITTI Raw OXTS + velodyne | (b) drive đã chọn ở M1.7 | (b) reference = OXTS; đo sinh từ OXTS + nhiễu → nhãn *synthetic*; odometry bánh xe **không có trong KITTI** → nếu tự sinh thì nhãn `mock` | ATE, RPE (evo), NEES Monte Carlo (chỉ với (a)) |
| Prediction (M5.4) | Track GT KITTI Tracking đưa về frame thế giới bằng OXTS | Các sequence đánh giá ở trên | `provenance=ground_truth` (tách bạch với track từ model) | ADE/FDE @ 1 s / 2 s, miss rate |

**Quy tắc chung:**
- Kiểm tra **overlap theo drive/sequence** giữa các nguồn (Object và Tracking đều cắt từ KITTI Raw — dùng file mapping của devkit), không chỉ so tên file.
- Cố định: danh sách file split, checkpoint model, seed, phiên bản evaluator → ghi trong `reports/`.
- **Dung lượng:** các file zip KITTI lớn (ảnh Object ~12 GB, velodyne Object ~29 GB, Tracking còn lớn hơn — xác minh trên trang tải). Quy trình: kiểm tra `df -h` → tải → giải nén **chỉ** tập con → xóa zip. Tính cả dung lượng đỉnh lúc tải + giải nén. Không giữ đồng thời dataset lớn và CARLA.

---

## Phase 1 — Engineering Foundation (~116 giờ)

1. **Mục tiêu năng lực:** viết Python/NumPy sạch có test; đọc/viết C++ cơ bản với Eigen + CMake; thành thạo terminal, Git, debugger; làm chủ transform 3D và xác suất cơ bản; chạy và tự viết node ROS 2 với TF2, rosbag2; biết simulator có chạy được trên máy hay không.
2. **Dependency:** M1.1 → (M1.2, M1.3, M1.4) → M1.5, M1.6 → M1.7; M1.6 → M1.8 (xem `DEPENDENCY_GRAPH.md`).
3. **Module & thứ tự:**

| ID | Module | Giờ | Tài liệu chính | Bổ sung |
|---|---|---|---|---|
| M1.1 | Python & NumPy: typing, dataclass, vectorization, broadcasting, pytest, logging; **khởi tạo package `drivestack`** (`pyproject.toml`, `core/`, `pip install -e .`) | 14 | NumPy User Guide | pytest docs |
| M1.2 | Linux, Git, debugging: bash, env vars, venv, pdb, `git revert/bisect` | 10 | *The Missing Semester* (MIT) | Pro Git ch.2–3 |
| M1.3 | Linear algebra & 3D transforms: rotation matrix, Euler/quaternion, homogeneous 4×4, quy ước `T_a_b` | 20 | *Modern Robotics* ch.3 | 3Blue1Brown *Essence of Linear Algebra* |
| M1.4 | Probability & estimation: Gaussian, covariance, Bayes, least squares, lan truyền sai số | 14 | *Kalman and Bayesian Filters in Python* (Labbe) ch.1–3 | *Probabilistic Robotics* ch.2 |
| M1.5 | C++ cho robotics: C++17, reference/const, STL, RAII, CMake, Eigen | 20 | learncpp.com (chọn lọc) | Eigen docs |
| M1.6 | ROS 2 Humble: node, topic, service, action, params, launch, TF2, rosbag2, RViz, **clock (ROS time / system / steady), `use_sim_time`** | 22 | docs.ros.org/en/humble | REP-103, REP-105; ROS 2 Clock & Time design |
| M1.7 | DriveStack skeleton (mở rộng package từ M1.1): schemas, frames, units, timestamp, trace ID, replay KITTI Raw → ROS 2 | 10 | `PROJECT_ARCHITECTURE.md` | — |
| M1.8 | **Thử khả thi simulator** (timebox 6 h, sau gate M1.6) — xem mục 3 dưới | 6 | CARLA 0.9.15 docs (quickstart, synchrony) | — |

4. **Thời gian:** ~4 tuần. Nếu Diagnostic cho phép gate-only M1.1/M1.2/M1.4: bớt ~38 h dạy nhưng vẫn tốn giờ thi gate → còn ~80 h ≈ **3 tuần** (không phải 2).
5. **Lý thuyết [NOW]:** vectorization; rotation & homogeneous transforms; REP-103 (x trước, y trái, z lên) và camera optical (x phải, y xuống, z trước); Gaussian & covariance; Bayes; pub/sub & QoS; TF tree; ba loại clock. **[LATER]:** Lie group SE(3), C++ templates nâng cao. **[OPT]:** ROS 2 C++ component, DDS tuning.
6. **Thực hành:** `transforms.py` tự viết + test; Euler↔quaternion↔R round-trip; bài Bayes cho detector; node C++ pub/sub; ghi/đọc rosbag2; đo processing duration bằng steady clock.
7. **Mini-project:** *KITTI Raw replay* — drive đã chốt (ảnh + velodyne + calib + OXTS) → topic ROS 2 với stamp = thời điểm cảm biến, TF static (velodyne, camera) → RViz.
8. **Test & đánh giá:** unit test transforms; test replay giữ thứ tự timestamp và tái lập; `ros2 topic hz`.
9. **Liên kết:** transforms → M2.1, M3.6, M4.x; schemas & replay là xương sống; probability → M2.7, M4.4; kết luận M1.8 quyết định đích Phase 6.
10. **Exit criteria (Phase Gate 1):** mọi module gate ≥ 85 & đạt cốt lõi; MS1 chạy từ README; ≥ 20 unit test xanh; **biên bản M1.8 đã chốt đích capstone**; phỏng vấn Phase 1 ≥ 85.

### M1.8 — Thử khả thi simulator (quyết định sớm thay vì tuần 22)

- **Làm gì:** CARLA 0.9.15 (UE4), `-RenderOffScreen -quality-level=Low`, synchronous mode + `fixed_delta_seconds` (vd. 0,05); spawn ego + 1 camera độ phân giải thấp + 1 LiDAR ít kênh; gửi `VehicleControl` và xác nhận xe phản hồi; đo VRAM, RAM, FPS sim, real-time factor trong 5 phút.
- **Trước khi tải:** kiểm tra dung lượng (gói CARLA lớn, ~20 GB sau giải nén — xác minh); nếu không đủ chỗ cùng dataset, làm thử rồi xóa.
- **Kết luận bắt buộc (ghi vào `LEARNING_LOG.md` + `reports/m1_8_sim_feasibility.md`):**
  - **Đích đầy đủ — local:** chạy ổn định ở cấu hình nhẹ.
  - **Đích đầy đủ — remote GPU:** không ổn trên máy nhưng có ngân sách GPU thuê/Colab-không-phù-hợp → chốt nhà cung cấp và chi phí ước tính.
  - **Đích dự phòng:** không chạy được và không có remote → capstone đổi như mục Phase 6.
- Lưu ý: synchronous mode giúp **tái lập**, không chứng minh pipeline đạt real-time — báo real-time factor riêng.

---

## Phase 2 — Camera Perception (~110 giờ)

1. **Mục tiêu:** project 3D↔2D chính xác; calibrate camera và đánh giá; train/fine-tune detector có kiểm soát; tự cài SORT; đo mAP/AP, HOTA/IDF1, latency **theo protocol mục 1**.
2. **Dependency:** M1.3 → M2.1 → M2.2; M1.4 + M2.4 → M2.5 → M2.7 → M2.8.
3. **Module:**

| ID | Module | Giờ | Tài liệu chính | Bổ sung |
|---|---|---|---|---|
| M2.1 | Camera geometry: pinhole, K, extrinsic, distortion, projection, IPM | 14 | Szeliski *CV: Algorithms & Applications* 2e ch.2 | OpenCV calib3d docs |
| M2.2 | Calibration (Zhang), reprojection error, chất lượng calib | 8 | OpenCV camera calibration tutorial | Szeliski ch.11 |
| M2.3 | Image processing: filter, edge, feature, homography | 8 | OpenCV-Python tutorials | — |
| M2.4 | Deep learning với PyTorch: tensor, autograd, CNN, training loop, overfit, **split chống leakage** | 22 | *Dive into Deep Learning* (d2l.ai) | PyTorch tutorials |
| M2.5 | 2D detection: YOLO, IoU, NMS, AP KITTI vs mAP COCO, fine-tune trên tập con KITTI Object đã chốt | 22 | Ultralytics docs (xác minh phiên bản) | YOLO papers |
| M2.6 | Segmentation: semantic vs instance, mIoU, drivable area trên KITTI Semantic | 8 | d2l.ai (segmentation) | [OPT] lane detection |
| M2.7 | Linear KF + 2D MOT: SORT (KF + Hungarian); ByteTrack [cắt được]; đánh giá TrackEval trên KITTI Tracking | 18 | Labbe ch.4–8 + SORT paper | ByteTrack paper, TrackEval docs |
| M2.8 | Camera perception node + evaluation + **latency đúng cách** (processing duration, pipeline latency, data age) | 10 | `PROJECT_ARCHITECTURE.md` §3 | — |

4. **Thời gian:** ~4 tuần; ước lượng lại sau Diagnostic.
5. **Lý thuyết [NOW]:** pinhole & distortion; precision/recall, AP vs mAP; NMS; overfitting & leakage; KF predict/update; Hungarian. **[LATER]:** DETR. **[OPT]:** BEV từ camera.
6. **Thực hành:** tự viết `project_points` + undistort vs `cv2.projectPoints`; calib camera laptop; tự viết IoU/NMS/AP rồi so với evaluator; tự viết SORT.
7. **Mini-project:** *Camera perception node* — ảnh replay → YOLO → SORT → `Detection2DArray`/`Track2DArray` + overlay. Báo cáo AP/mAP, HOTA/IDF1, latency p50/p95, FPS trên RTX 3050.
8. **Test:** IoU/NMS edge cases; AP tự tính khớp evaluator; regression Phase 1 xanh.
9. **Liên kết:** projection → M3.6; KF → M3.5, M4.4. **Track 2D ở đơn vị pixel — không dùng trực tiếp cho prediction/TTC theo mét** (xem M5.0).
10. **Exit criteria:** module gates đạt; MS2; `reports/phase2.md` ghi rõ dataset/split/evaluator + ≥ 3 failure cases; phỏng vấn ≥ 85.

---

## Phase 3 — LiDAR & 3D Perception (~135 giờ)

1. **Mục tiêu:** xử lý point cloud từ thô đến cluster; đánh giá 3D detection có protocol; fine-tune ngắn một model 3D; project LiDAR lên ảnh, late fusion; tự cài 3D MOT trên detection đã fuse.
2. **Dependency:** M1.3 → M3.1 → (M3.2, M3.3) → M3.4; M2.2 + M3.3 → M3.6; M3.4 + M3.6 + M2.7 → M3.5 → M3.7.
3. **Ranh giới fusion/tracking (baseline):** fusion ở **mức detection** (late fusion: 3D LiDAR detection được xác nhận/bổ sung lớp bởi detection 2D camera) → **một** 3D tracker duy nhất chạy trên detection đã fuse. Track 2D của Phase 2 giữ là sản phẩm riêng, **không** ghép ID vào track 3D.
4. **Module:**

| ID | Module | Giờ | Tài liệu chính | Bổ sung |
|---|---|---|---|---|
| M3.1 | Point cloud: KITTI `.bin`, intensity, mật độ theo khoảng cách, Open3D | 12 | Open3D docs | KITTI devkit readme |
| M3.2 | Preprocessing: ROI, voxel downsample, RANSAC ground, Euclidean/DBSCAN, kd-tree | 20 | Open3D tutorials | PCL concepts |
| M3.3 | 3D boxes 7-DoF, frame KITTI (camera rect vs velodyne), **BEV IoU và 3D IoU** tự viết, AP3D | 16 | KITTI object devkit | — |
| M3.4 | Voxel/pillar/BEV, PointPillars: inference + eval theo protocol + **fine-tune ngắn** (local nếu vừa VRAM, không thì Colab/remote); CenterPoint so sánh [cắt được → khái niệm] | 32 | PointPillars & CenterPoint papers + OpenPCDet (xác minh phiên bản) | mmdetection3d |
| M3.6 | Camera–LiDAR fusion: extrinsic, project, frustum, late fusion, xử lý lệch thời gian | 22 | KITTI calib format + bài M2.1 | [OPT] BEV fusion |
| M3.5 | 3D MOT (AB3DMOT-style) trên detection đã fuse, **tracking trong frame thế giới** khi có pose (xem M5.0) | 18 | AB3DMOT paper | — |
| M3.7 | LiDAR perception node + RViz markers + latency | 15 | ROS 2 visualization_msgs | — |

(Thứ tự học: M3.1 → M3.2 → M3.3 → M3.4 → M3.6 → M3.5 → M3.7.)

5. **Thời gian:** ~5 tuần; ước lượng lại sau Diagnostic (tận dụng kinh nghiệm annotation cuboid ở M3.3).
6. **Lý thuyết [NOW]:** sparsity theo khoảng cách; ground segmentation; voxel/pillar/BEV trade-off; quy ước yaw; AP3D vs BEV; frame KITTI. **[LATER]:** sparse convolution. **[OPT]:** BEVFusion, 4D radar.
7. **Thực hành:** pipeline classic (ground + cluster + box fitting); BEV/3D IoU tự viết; eval + fine-tune PointPillars; fusion; 3D MOT.
8. **Mini-project:** *LiDAR 3D perception + fusion* — cloud replay → PointPillars (+ classic baseline) → fusion → 3D MOT → RViz.
9. **Test:** round-trip label camera↔LiDAR; IoU vs thư viện; so sánh trước/sau fine-tune và classic vs learning theo khoảng cách.
10. **Exit criteria:** module gates đạt; MS3; `reports/phase3.md` có AP theo khoảng cách, so sánh fine-tune, latency, ≥ 3 failure cases; phỏng vấn ≥ 85.

---

## Phase 4 — Localization & Mapping (~110 giờ)

1. **Mục tiêu:** dựng cây frame đúng; motion model; EKF đúng và **consistent** (kiểm chứng trên dữ liệu tổng hợp) rồi áp dụng trên KITTI có ghi rõ giả định; ICP; hiểu SLAM; ATE/RPE.
2. **Dependency:** M1.6 → M4.1 → M4.2 → M4.3 → M4.4; M3.2 → M4.5 → M4.6 → M4.7.
3. **Module:**

| ID | Module | Giờ | Tài liệu chính | Bổ sung |
|---|---|---|---|---|
| M4.1 | Frames map/odom/base_link (REP-105), TF đúng timestamp | 8 | REP-105 + TF2 docs | — |
| M4.2 | Motion models (unicycle, bicycle), dead reckoning, drift | 12 | *Probabilistic Robotics* ch.5 | Labbe |
| M4.3 | IMU (bias, noise) & GNSS (LLA→ENU), KITTI OXTS — hiểu OXTS là GPS/IMU **đã kết hợp**, không phải các phép đo độc lập | 12 | KITTI Raw devkit + Geiger et al. 2013 (IJRR) | — |
| M4.4 | EKF: Jacobian, tuning Q/R. **(a)** dữ liệu tổng hợp có state thật + đo độc lập → kiểm tra đúng + NEES/NIS Monte Carlo. **(b)** KITTI: reference = OXTS, đo = OXTS + nhiễu (nhãn *synthetic*), odometry tự sinh (nhãn `mock`) | 26 | Labbe ch.11 | *Probabilistic Robotics* ch.3 |
| M4.5 | ICP point-to-point/plane, LiDAR odometry trên drive KITTI Raw | 18 | Cyrill Stachniss lectures (ICP) | Open3D registration |
| M4.6 | SLAM basics: pose graph 2D, loop closure [cắt được → khái niệm] | 18 | Cyrill Stachniss (Graph SLAM) | [OPT] KISS-ICP |
| M4.7 | Localization node → `EgoState` + covariance + TF; ATE/RPE (evo) so với reference OXTS | 16 | evo docs | — |

4. **Thời gian:** ~4 tuần.
5. **Lý thuyết [NOW]:** KF→EKF; Jacobian; consistency (NEES với giới hạn chi-square); drift; ENU; reference ≠ ground truth. **[LATER]:** UKF, factor graph. **[OPT]:** VIO.
6. **Thực hành:** EKF tổng hợp với Monte Carlo; EKF trên KITTI có bảng input/reference/giả định tương quan; ICP tự viết vs Open3D.
7. **Mini-project:** *Localization module* → `EgoState` + TF `map→odom→base_link`; ATE/RPE so với OXTS.
8. **Test:** EKF với dữ liệu biết đáp án; NEES theo protocol ở `MODULE_GATES.md`; regression Phase 1–3 xanh.
9. **Liên kết:** ego pose cho M5.0 (đưa object về frame planning), planning/control.
10. **Exit criteria:** module gates đạt; MS4; `reports/phase4.md` có ATE/RPE, drift, kịch bản mất GNSS, **bảng giả định dữ liệu**; phỏng vấn ≥ 85.

---

## Phase 5 — Prediction, Planning & Control (~118 giờ)

1. **Mục tiêu:** contract trạng thái vật thể cho planning; mô hình xe; controller ổn định có xử lý giới hạn; planner tìm đường và sinh quỹ đạo khả thi; prediction baseline; closed-loop thật trong sim 2D.
2. **Dependency:** M3.5 + M4.7 → **M5.0** → M5.4; M4.2 → M5.1 → M5.2; M5.3; M5.4 → M5.5 → M5.6.
3. **Module:**

| ID | Module | Giờ | Tài liệu chính | Bổ sung |
|---|---|---|---|---|
| **M5.0** | **Object state contract:** đưa 3D tracks + covariance về frame planning tại đúng timestamp; bù ego-motion; vận tốc tuyệt đối; xử lý dữ liệu quá cũ; nguồn map/route | 8 | `PROJECT_ARCHITECTURE.md` §4.1 | — |
| M5.1 | Kinematic bicycle model, giới hạn steering/accel | 10 | PythonRobotics (tham khảo) | Rajamani *Vehicle Dynamics* ch.2 |
| M5.2 | PID dọc, Pure Pursuit, Stanley [cắt được]; delay, saturation, rate limit | 20 | Snider 2009 *Automatic Steering Methods* | PythonRobotics |
| M5.3 | Dijkstra, A* (lưới), **RRT** (không gian liên tục 2D); occupancy grid | 16 | Red Blob Games (A*) | LaValle *Planning Algorithms* (chọn lọc) |
| M5.4 | Prediction: CV/CTRV từ `ObjectState`, ADE/FDE, collision check, TTC (mét, frame planning) | 16 | Bài tự viết | nuScenes prediction docs |
| M5.5 | Behavior FSM, Frenet frame, polynomial trajectory, cost function | 24 | Werling et al. 2010 | PythonRobotics |
| M5.6 | Closed loop trong sim 2D tự viết: lệnh điều khiển **thay đổi** trạng thái xe và quan sát kế tiếp | 24 | `PROJECT_ARCHITECTURE.md` | — |

4. **Thời gian:** ~4,3 tuần.
5. **Lý thuyết [NOW]:** ego-motion compensation; bicycle model; PID & windup; lookahead; A* admissible; RRT; Frenet; TTC. **[LATER]:** MPC. **[OPT]:** learning-based prediction, RL planning.
6. **Thực hành:** test "vật đứng yên, ego chạy → vận tốc trong map ≈ 0"; tune controller; A* + RRT; Frenet planner tránh xe đỗ.
7. **Mini-project:** *Closed-loop 2D* — map/route từ file kịch bản (polyline làn trong frame `map`); object từ stub có `provenance` rõ; kịch bản: bám làn, vượt xe đỗ, xe trước phanh. Metric: collision rate, route completion, tracking error, overshoot, jerk, constraint violations.
8. **Test:** controller ổn định nhiều tốc độ; planner không cắt qua vật cản; ADE/FDE baseline.
9. **Liên kết:** dùng `ObjectState` (Phase 3 + 4); chuẩn bị adapter CARLA hoặc đích dự phòng.
10. **Exit criteria:** module gates đạt; MS5; `reports/phase5.md`; phỏng vấn ≥ 85.

---

## Phase 6 — End-to-End Autonomous Driving (~85 giờ)

1. **Mục tiêu:** tích hợp toàn hệ thống theo **đích đã chốt ở M1.8**; đo latency đúng cách; test kịch bản; phân tích lỗi; portfolio + phỏng vấn.
2. **Hai đích deliverable (chọn theo biên bản M1.8, không đổi tên cho đẹp):**
   - **Đích đầy đủ** (CARLA local hoặc remote): sensor → model → fusion/tracking → `ObjectState` → prediction → planning → control → xe → sensor. Synchronous mode + fixed timestep, ghép sensor theo frame id. Báo cáo **riêng** chất lượng closed-loop và tốc độ thực thi (real-time factor, latency p50/p95).
   - **Đích dự phòng:** (i) perception/fusion/tracking trên **replay** (open-loop, đánh giá perception); (ii) **closed-loop 2D** (Phase 5) với object stub ghi rõ provenance. Hai kết quả báo cáo riêng; **không gọi là full sensor-to-control closed-loop**.
3. **Module:**

| ID | Module | Giờ | Tài liệu chính |
|---|---|---|---|
| M6.1 | Dựng môi trường theo đích đã chốt: CARLA + ROS 2 bridge/adapter map-route (đích đầy đủ) hoặc đóng gói replay + sim 2D (dự phòng) | 15 | CARLA docs |
| M6.2 | Full pipeline: đồng bộ, latency 3 loại, dropped frames, provenance | 25 | `PROJECT_ARCHITECTURE.md` §3 |
| M6.3 | Scenario tests, failure analysis, regression suite | 20 | — |
| M6.4 | README, báo cáo kỹ thuật, video demo, 2 mock interview, giải thích kiến trúc | 25 | `INTERVIEW_PREP.md` |

4. **Thời gian:** ~3 tuần + dự phòng.
5. **Lý thuyết [NOW]:** latency budget; clock domains; ODD; simulation vs replay vs real. **[LATER]:** Docker hóa GPU (A8). **[OPT]:** OpenSCENARIO.
6. **Thực hành:** đo latency từng node; kịch bản thất bại có chủ đích (cảm biến trễ, mất GNSS, đêm).
7. **Capstone:** DriveStack Lab theo đích đã chốt, mọi output gắn provenance.
8. **Test:** regression suite toàn bộ; metric theo tầng + hệ thống.
9. **Liên kết:** tổng hợp mọi phase.
10. **Exit criteria:** demo theo đích đã chốt; báo cáo metric theo tầng; ≥ 5 failure cases; mock interview ≥ 85; giải thích kiến trúc 5 phút đạt. **Không tuyên bố an toàn** — báo cáo nêu ODD và giới hạn.

---

## Sổ giờ thực tế (điền ở Weekly Review)

| Tuần | Học mới | Gate/đánh giá | Tích hợp/sửa lỗi | Dự phòng/remediation | Tổng | Module xong | Ghi chú |
|---|---|---|---|---|---|---|---|
| – | | | | | | | |

## Lịch sử điều chỉnh

| Ngày | Thay đổi | Lý do / bằng chứng |
|---|---|---|
| 2026-11-03 | Khởi tạo khung 24 tuần | Chưa có diagnostic |
| 2026-11-04 | Ngân sách giờ có điều kiện (674 h + 15% dự phòng); kế hoạch dữ liệu & protocol; thêm M1.8 thử khả thi CARLA và M5.0 object state contract; hai đích capstone; ranh giới fusion/tracking; RRT; fine-tune 3D bắt buộc (local/Colab); protocol EKF; bootstrap package ở M1.1 | `ROADMAP_REVIEW.md` (P1 #1–5, P2 #6–7, điều chỉnh nhỏ) |
