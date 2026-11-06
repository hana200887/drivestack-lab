# LEARNING LOG

> Checkpoint mới nhất đặt **trên cùng** mục Nhật ký. Mentor đọc file này đầu mỗi phiên — giữ bảng Trạng thái luôn đúng.

## Trạng thái hiện tại

| Mục | Giá trị |
|---|---|
| Phase | Trước Phase 1 — chờ Diagnostic |
| Module đang học | – |
| Bài đang chờ nộp | `DIAGNOSTIC_TEST.md` → `learning/diagnostic/submission/` |
| Gate gần nhất | – |
| Giờ học tích lũy | 0 |
| Cập nhật | 2026-11-04 (đã áp dụng review) |

## Nhật ký

### Checkpoint — 2026-11-04 — Áp dụng review lộ trình
- **Đã học:** chưa mở buổi học; sửa thiết kế lộ trình theo `ROADMAP_REVIEW.md` theo yêu cầu người học.
- **Đã làm được:**
  - Ngân sách giờ có điều kiện: 674 h nội dung, cộng 15% dự phòng. Lịch 24 tuần chỉ đạt được nếu Diagnostic rút được khoảng 100 h; fast track 18 tuần cần rút khoảng 240 h. Thêm sổ giờ 4 loại và danh sách phạm vi có thể cắt.
  - Kế hoạch dữ liệu & protocol cho từng loại metric: KITTI Raw / Object 3712/3769 / Tracking tách theo sequence / Semantic 160/40; evaluator; quy tắc dung lượng.
  - Thêm **M1.8**: thử khả thi CARLA ngay sau gate ROS 2, chốt đích capstone (đầy đủ hoặc dự phòng).
  - Thêm **M5.0**: contract `ObjectState` (frame `map`, bù ego-motion, dữ liệu cũ, nguồn map/route).
  - Đo thời gian tách 3 clock domain và 3 đại lượng: processing duration, pipeline latency, data age.
  - Protocol EKF: dữ liệu tổng hợp + NEES Monte Carlo; KITTI có bảng giả định.
  - Fusion ở mức detection rồi đưa vào một 3D tracker.
  - Bổ sung RRT, 3D IoU, fine-tune 3D (local hoặc Colab); A8 Docker mục tiêu L1; package khởi tạo từ M1.1.
  - Thống nhất cách chấm câu ★ và nguyên tắc gate-only vẫn phải thi gate.
- **Bằng chứng:** tổng giờ theo bảng phase 116+110+135+110+118+85 = 674 (kiểm tra bằng script trong phiên).
- **Chưa chắc:** tốc độ học thực tế; CARLA có chạy được trên 4 GB VRAM không (sẽ biết ở M1.8); ứng viên drive KITTI Raw cụ thể (chốt ở M1.7).
- **Lời giải AI đã dùng:** không.
- **Module đang học:** – (trước Phase 1).
- **Việc tiếp theo:** người học làm Diagnostic (gồm Bước 0); sau khi chấm mới tính lại giờ và chọn kịch bản lịch.
- **File đã cập nhật:** `ROADMAP.md` (viết lại), `PROJECT_ARCHITECTURE.md` (viết lại), `MODULE_GATES.md`, `DEPENDENCY_GRAPH.md`, `SKILL_MATRIX.md`, `DIAGNOSTIC_TEST.md` (luật quyết định), `ROADMAP_REVIEW.md` (trạng thái), skill `references/assessment.md` + `references/environment.md`, `LEARNING_LOG.md`.

### Checkpoint — 2026-11-04 — Review lộ trình
- **Đã học:** chưa mở buổi học; review thiết kế lộ trình theo yêu cầu người học.
- **Đã làm được:** đối chiếu roadmap với dependency graph, skill matrix, gates, kiến trúc và môi trường; lưu phát hiện và đề xuất trong `learning/ROADMAP_REVIEW.md`.
- **Bằng chứng:** đọc các tài liệu `learning/` và skill `adas-mentor`; script `python3` cộng giờ từ tiêu đề phase → `[110, 110, 135, 110, 110, 85]`, tổng **660 giờ**; quỹ 18 tuần **450–540 giờ**, 24 tuần **600–720 giờ**; kiểm tra `learning/diagnostic/submission/` và `drivestack/` → đều chưa tồn tại. Đối chiếu tài liệu chính thức KITTI, ROS 2 clocks, CARLA requirements/synchrony; nguồn ghi trong bản review.
- **Phát hiện chính:** fast track chưa khớp giờ; chưa chốt dữ liệu/protocol đánh giá; replay không thay thế kiểm chứng closed-loop; thiếu contract tracks → planning; công thức latency chưa phân biệt clock domain; cần làm rõ protocol EKF và độ khớp matrix/gates.
- **Chưa chắc:** năng lực và tốc độ học thực tế; khả năng chạy simulator/model trên máy chưa được benchmark. Giữ mọi mức năng lực/gate như cũ; chưa cá nhân hóa lịch.
- **Lời giải AI đã dùng:** không — chỉ review tài liệu, không làm bài Diagnostic.
- **Module đang học:** – (trước Phase 1).
- **Việc tiếp theo:** người học làm Diagnostic, gồm Bước 0; sau khi nộp mới quyết định rút/bỏ phần dạy và cập nhật lịch. Các sửa đổi thiết kế được đề xuất trong bản review; chưa áp dụng vào `ROADMAP.md`.
- **File đã cập nhật:** `learning/ROADMAP_REVIEW.md` (mới), `learning/LEARNING_LOG.md`.

### Checkpoint — 2026-11-03 — Onboarding (Session 0)
- **Đã học:** chưa — phiên thiết lập.
- **Đã làm được:** tạo skill `adas-mentor` (`.claude/skills/adas-mentor/`) từ master prompt; soạn Competency Map, Dependency Graph, Roadmap 24 tuần, kiến trúc DriveStack Lab, Module Gates, Diagnostic Test.
- **Bằng chứng (đo môi trường, lệnh read-only):** Ubuntu 22.04.5; i5-11400H 12 threads; RAM 15 GiB; RTX 3050 Laptop 4096 MiB, driver 580.178.04 (CUDA driver 13.0); ổ `/` trống 63 GB; Python 3.10.12; g++ 11.4; git có sẵn. Chưa có: cmake, docker, nvcc, ROS 2, numpy, torch, opencv, open3d.
- **Chưa chắc:** toàn bộ năng lực — chưa có đánh giá.
- **Lời giải AI đã dùng:** không.
- **Module đang học:** –
- **Việc tiếp theo:** làm Diagnostic Test (kể cả Bước 0: tự tạo venv + cài numpy, pytest); nộp bằng câu "nộp diagnostic".
- **File đã cập nhật:** tất cả file trong `learning/` (khởi tạo).
