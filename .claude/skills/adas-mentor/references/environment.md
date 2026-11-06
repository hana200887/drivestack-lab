# Environment — phần cứng, tương thích, quyết định cài đặt

> Đo bằng lệnh read-only ngày **2026-11-03**. Đo lại nếu người học báo thay đổi phần cứng/OS.
> Các mục đánh dấu ⚠️ cần xác minh tài liệu chính thức ngay trước khi cài, vì phiên bản thay đổi theo thời gian.

## 1. Phần cứng đã đo

| Thành phần | Giá trị | Hệ quả |
|---|---|---|
| Máy | Acer Nitro 5 AN515-57 | Laptop — chú ý nhiệt độ khi train lâu |
| OS | Ubuntu 22.04.5 LTS, kernel 6.8 | Khớp ROS 2 Humble (Tier 1) |
| CPU | Intel i5-11400H, 12 threads | Đủ cho point-cloud processing, build C++ |
| RAM | 15 GiB (lúc đo đã dùng ~8 GiB) | Đóng trình duyệt nặng khi chạy CARLA/train |
| GPU | RTX 3050 Laptop, **4 GB VRAM**, driver 580, CUDA driver 13.0 | Ràng buộc lớn nhất (xem mục 3) |
| Ổ đĩa | 96 GB, trống ~63 GB | Không tải full KITTI/nuScenes; CARLA chiếm đáng kể |
| Python | 3.10.12 (system) | = Python của ROS 2 Humble |
| Toolchain | g++ 11.4, git có sẵn; **chưa có** cmake, docker, nvcc, ROS 2, numpy, torch, opencv, open3d | Cài dần theo module, không cài trước |

## 2. Chiến lược môi trường (2 môi trường tách biệt)

1. **ROS 2 env** — Python system 3.10 + ROS 2 Humble (apt). Mọi node ROS 2 chạy ở đây. Giữ `numpy<2` trong phạm vi mà ROS 2 Humble dùng (cv_bridge và các gói apt của Humble được build với NumPy 1.x). ⚠️
2. **ML env** — venv riêng (`python3 -m venv`, hoặc `uv` nếu cần Python mới hơn) cho PyTorch/Ultralytics/Open3D/OpenPCDet. Không cài pip package vào system Python.

Lý do tách: tránh xung đột NumPy/OpenCV giữa ROS và stack deep learning. Giao tiếp giữa hai env bằng file (rosbag/npz) hoặc chạy model trong node với env đã kiểm tra tương thích ở Phase 2.

Ghi chú: Python 3.10 hết hạn hỗ trợ (EOL) tháng 10/2026 → một số thư viện bản mới có thể ngừng hỗ trợ 3.10. ⚠️ Nếu gặp, dùng Python 3.11/3.12 cho ML env (qua `uv`), giữ 3.10 cho ROS 2 Humble.

## 3. Ràng buộc 4 GB VRAM — quyết định theo phase

| Việc | Phương án chính trên máy | Fallback |
|---|---|---|
| PyTorch | Wheel có CUDA runtime đi kèm — không cần cài CUDA toolkit/nvcc. Chọn bản cu12x tương thích driver 580 ⚠️ | CPU |
| YOLO fine-tune (Phase 2) | Model nhỏ (nano/small), imgsz 640, batch 4–8, AMP; subset KITTI/BDD vài nghìn ảnh | Colab / remote GPU |
| PointPillars (Phase 3) | **Inference + evaluation** pretrained trên subset KITTI; fine-tune ngắn nếu vừa VRAM | Train trên Colab/remote GPU |
| Build CUDA ops (OpenPCDet/spconv) | Cần CUDA toolkit khớp phiên bản torch ⚠️ — làm khi tới M3.4, không làm sớm | mmdet3d hoặc Colab |
| CARLA (Phase 6) | 0.9.15 (UE4) — tài liệu chính thức nêu GPU **tối thiểu 6 GB, khuyến nghị 8 GB** (https://carla.readthedocs.io/en/0.9.15/start_quickstart/) → máy 4 GB **dưới mức tối thiểu**. Thử khả thi ở **M1.8** (ngay sau gate ROS 2, timebox 6 h): `-RenderOffScreen -quality-level=Low`, synchronous + fixed timestep, ít cảm biến | Remote GPU (đích đầy đủ) hoặc **đích dự phòng**: perception trên replay + closed-loop sim 2D, báo cáo riêng. Replay **không** thay thế closed-loop |
| CARLA 0.10 / UE5 | **Không dùng** — yêu cầu GPU lớn hơn nhiều | — |

## 4. Ổ đĩa (~63 GB trống)

Ngân sách gợi ý: CARLA ~20 GB sau giải nén ⚠️ · dataset giữ lại ≤ 15 GB · venv + model ≤ 10 GB · còn lại làm đệm.
Dataset và split cụ thể: `learning/ROADMAP.md` §1 (KITTI Raw 1 drive; KITTI Object split 3712/3769 tập con; KITTI Tracking tách theo sequence; KITTI Semantic 160/40).
**Dung lượng đỉnh:** zip KITTI rất lớn (Object ảnh ~12 GB, velodyne ~29 GB, Tracking lớn hơn ⚠️) — phải tính zip + phần giải nén + cache, không chỉ subset giữ lại. Quy trình: `df -h` → tải → giải nén chỉ tập con → xóa zip. Không giữ đồng thời zip lớn và CARLA.
Ghi mọi dataset vào `drivestack/data/MANIFEST.md` (nguồn, phiên bản, danh sách file split, checksum, license — KITTI: CC BY-NC-SA 3.0).

## 5. ROS 2

- ROS 2 **Humble** trên Ubuntu 22.04 — lựa chọn đúng cho máy này. Humble EOL **5/2027** ⚠️: lịch có thể kéo qua mốc này (xem ngân sách giờ trong ROADMAP) → vẫn dùng được nhưng hết bản vá; ghi chú trong portfolio rằng bản kế tiếp (Jazzy) cần Ubuntu 24.04.
- Đo thời gian: data/ROS time cho stamp; steady clock cho thời lượng xử lý và pipeline latency (https://design.ros2.org/articles/clock_and_time.html) — chi tiết `learning/PROJECT_ARCHITECTURE.md` §3.1.
- Cài bản `ros-humble-desktop` (có RViz). Không cài từ source.

## 6. Quy tắc trước mỗi lần đề xuất cài đặt

1. Kiểm tra lại: `lsb_release -ds`, `nvidia-smi`, `free -h`, `df -h ~`, `python3 --version`.
2. Đối chiếu tài liệu chính thức về phiên bản (PyTorch "Get Started", ROS 2 Humble docs, CARLA release notes, repo của model).
3. Đề xuất phương án nhẹ nhất; nêu dung lượng tải và VRAM ước tính.
4. Có fallback.
5. Người học tự chạy lệnh cài và ghi lại vào `LEARNING_LOG.md` (lệnh + phiên bản thực tế cài được).
