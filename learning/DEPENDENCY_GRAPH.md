# DEPENDENCY GRAPH — kiến thức nối với nhau thế nào

> Bước 2 của master prompt. Mũi tên `A --> B` nghĩa là **phải vững A trước khi học B**. Module ID khớp với `ROADMAP.md` và `MODULE_GATES.md`.
> Biểu đồ Mermaid hiển thị trên GitHub/VS Code (cài extension Markdown Preview Mermaid nếu chưa thấy).

## 1. Đồ thị tổng quan

```mermaid
flowchart TD
  %% Phase 1 — Foundation
  M11[M1.1 Python & NumPy] --> M12[M1.2 Linux, Git, Debug, Test]
  M11 --> M13[M1.3 Linear algebra & 3D transforms]
  M11 --> M14[M1.4 Probability & estimation]
  M12 --> M15[M1.5 C++ & Eigen & CMake]
  M13 --> M15
  M12 --> M16[M1.6 ROS 2 core + TF2 + rosbag2]
  M13 --> M16
  M16 --> M17[M1.7 DriveStack skeleton: schemas + replay]
  M14 --> M17
  M16 --> M18[M1.8 Spike khả thi CARLA → chốt đích capstone]

  %% Phase 2 — Camera
  M13 --> M21[M2.1 Camera geometry & projection]
  M21 --> M22[M2.2 Calibration]
  M21 --> M23[M2.3 Image processing / OpenCV]
  M11 --> M24[M2.4 Deep learning & PyTorch]
  M14 --> M24
  M24 --> M25[M2.5 2D detection: YOLO, IoU, NMS, mAP]
  M24 --> M26[M2.6 Segmentation]
  M25 --> M27[M2.7 Linear KF + 2D MOT / SORT]
  M14 --> M27
  M27 --> M28[M2.8 Camera perception node + eval]
  M17 --> M28

  %% Phase 3 — LiDAR
  M13 --> M31[M3.1 Point cloud fundamentals]
  M31 --> M32[M3.2 Preprocess: ROI, voxel, ground, cluster]
  M31 --> M33[M3.3 3D boxes, frames, 3D IoU]
  M32 --> M34[M3.4 PointPillars / CenterPoint]
  M33 --> M34
  M24 --> M34
  M22 --> M36[M3.6 Camera–LiDAR detection-level fusion]
  M33 --> M36
  M34 --> M36
  M36 --> M35[M3.5 3D MOT trên detection đã fuse]
  M27 --> M35
  M35 --> M37[M3.7 LiDAR node + RViz]

  %% Phase 4 — Localization
  M16 --> M41[M4.1 Frames map/odom/base_link, time]
  M41 --> M42[M4.2 Motion models & odometry]
  M42 --> M43[M4.3 IMU & GNSS]
  M27 --> M44[M4.4 EKF sensor fusion]
  M43 --> M44
  M32 --> M45[M4.5 ICP / LiDAR odometry]
  M45 --> M46[M4.6 SLAM basics: pose graph]
  M44 --> M47[M4.7 Localization node + ATE/RPE]
  M46 --> M47

  %% Phase 5 — Prediction, Planning, Control
  M42 --> M51[M5.1 Kinematic bicycle model]
  M51 --> M52[M5.2 PID, Pure Pursuit, Stanley]
  M11 --> M53[M5.3 Dijkstra, A*, RRT]
  M35 --> M50[M5.0 Object state contract: tracks → frame map]
  M47 --> M50
  M50 --> M54[M5.4 Prediction + collision/TTC]
  M51 --> M54
  M53 --> M55[M5.5 Behavior FSM + trajectory planning]
  M54 --> M55
  M52 --> M56[M5.6 Closed loop in 2D sim]
  M55 --> M56
  M47 --> M56

  %% Phase 6 — System
  M28 --> M62[M6.2 Full pipeline integration + latency]
  M37 --> M62
  M56 --> M62
  M61[M6.1 Môi trường capstone theo đích đã chốt] --> M62
  M18 --> M61
  M62 --> M63[M6.3 Scenario tests & failure analysis]
  M63 --> M64[M6.4 Portfolio, report, mock interviews]
```

## 2. Các chuỗi xương sống (critical paths)

Nếu thiếu một mắt xích, mọi thứ phía sau sẽ "chạy được nhưng sai" — loại lỗi khó tìm nhất.

| Chuỗi | Vì sao quan trọng |
|---|---|
| **Transforms (M1.3) → Projection (M2.1) → Calibration (M2.2) → Camera–LiDAR fusion (M3.6) → 3D perception (M3.7)** | Mọi phép nối dữ liệu đa cảm biến đều là nhân ma trận transform. Sai quy ước một lần là lệch toàn hệ. |
| **Transforms (M1.3) → TF2 (M1.6) → Frames (M4.1) → Odometry (M4.2) → EKF (M4.4) → Planning/Control (M5.x)** | Planner lập kế hoạch trong frame `map`; nếu pose sai, quỹ đạo đúng về toán vẫn đưa xe đi sai. |
| **Probability (M1.4) → Linear KF (M2.7) → 3D MOT (M3.5) → EKF (M4.4)** | KF xuất hiện ba lần: tracking 2D, tracking 3D, localization. Hiểu một lần cho đúng, dùng ba lần. |
| **Detection (M3.4) → Fusion (M3.6) → 3D Tracking (M3.5) → Object state (M5.0) → Prediction (M5.4) → Behavior (M5.5)** | Prediction cần vận tốc **tuyệt đối theo mét trong frame map**; vận tốc đến từ tracking, chỉ đúng khi đã bù ego-motion (M5.0). |
| **Localization (M4.7) → Object state (M5.0)** | Đưa object về `map` cần pose ego tại đúng `stamp` của detection. |
| **Timestamp & clock (M1.6, M1.7) → Sync (M2.8, M3.6) → Latency (M6.2)** | Ở 20 m/s, 100 ms trễ = 2 m sai vị trí. Data time, steady time, system time không được trộn (`PROJECT_ARCHITECTURE.md` §3.1). |

## 3. Bảng "sai ở đây thì hỏng ở đâu"

| Module | Input → Output | Ai dùng output | Nếu sai thì phía sau hỏng thế nào |
|---|---|---|---|
| M1.3 Transforms | điểm/pose ở frame A → frame B | Gần như mọi module | Box lệch, point cloud chồng sai lên ảnh, pose nhảy, planner tránh vật cản "ma" |
| M1.7 Schemas & replay | dataset → message có timestamp/frame/unit | Mọi module | Đơn vị lẫn lộn (độ vs rad, ms vs s), dữ liệu không đồng bộ, test không tái lập được |
| M2.1–M2.2 Camera model & calib | ảnh checkerboard → K, distortion, extrinsic | Projection, fusion, IPM, đo khoảng cách | Project LiDAR lệch vật thể; ước lượng khoảng cách bằng camera sai theo độ sâu |
| M2.5 2D detection | ảnh → box + class + score | Tracking, fusion | FN → mất track, prediction không có vật cản; FP → phanh gấp vô cớ |
| M2.7 / M3.5 Tracking | detections theo thời gian → track ID + trạng thái (vị trí, vận tốc) | Prediction, behavior | ID switch → lịch sử quỹ đạo đứt; vận tốc ước lượng sai → TTC sai |
| M3.1–M3.2 Point cloud preprocess | raw cloud → cloud đã lọc, ground, cluster | 3D detection classic, ICP | Ground removal sai → mất người đi bộ thấp hoặc tạo vật cản giả |
| M3.3 3D boxes | label/prediction → box 7-DoF trong frame chỉ định | Eval, tracking, fusion | Sai quy ước yaw/frame KITTI → AP gần 0 dù model đúng |
| M3.4 3D detection | point cloud → box 3D + score | 3D MOT, fusion, planning | Bỏ sót vật thể xa/thưa → planner không thấy vật cản |
| M3.6 Fusion | box 2D + cloud + extrinsic + time → object hợp nhất | Tracking, planning | Lệch thời gian → ghép nhầm vật thể khi xe/vật đang chạy |
| M4.4 EKF | IMU/GNSS/odom → pose + covariance | Planning, control, map | Covariance quá tự tin → bỏ qua đo mới, drift không sửa được |
| M5.0 Object state | Track 3D + `EgoState` + TF → `ObjectState` trong `map` | Prediction, planning | Không bù ego-motion → vật đứng yên "chạy" với vận tốc ngược ego → phanh ma hoặc bỏ sót nguy cơ; dùng object cũ → TTC sai |
| M5.1 Vehicle model | lệnh lái → trạng thái kế tiếp | Control, planning, prediction | Quỹ đạo không khả thi động học, controller dao động |
| M5.2 Control | quỹ đạo + pose → steer/accel | Xe/simulator | Dao động, overshoot, vượt giới hạn lái |
| M5.5 Planning | map + objects + prediction → trajectory | Control | Va chạm, vi phạm ràng buộc, đứng yên không cần thiết |

## 4. Phụ thuộc chéo đáng chú ý

- **Linear KF được dạy ở M2.7** (vì SORT cần) với đầy đủ predict/update; **M4.4** mở rộng lên EKF. Không dạy EKF khi chưa qua gate M2.7.
- **M3.6 Fusion** cần cả nhánh camera (M2.2) lẫn nhánh LiDAR (M3.3, M3.4) — điểm hội tụ của Phase 2 và 3. Fusion ở mức **detection**, rồi **một** 3D tracker (M3.5) chạy trên kết quả fuse; track 2D pixel của Phase 2 không ghép ID vào track 3D.
- **M1.8** (spike CARLA, ngay sau gate M1.6) quyết định đích capstone sớm; Phase 6 không chờ tới tuần 22 mới biết simulator có chạy được không.
- **M5.6** dùng localization từ Phase 4; nếu Phase 4 chưa đạt, dùng **stub pose từ ground truth sim, ghi rõ `provenance=ground_truth`** — không được báo cáo như kết quả localization thật.
- **ROS 2 (M1.6)** là điều kiện để mọi module trở thành node; logic thuật toán viết thuần Python/C++ trước, bọc thành node sau (dễ test hơn).
