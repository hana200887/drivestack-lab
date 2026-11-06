# DriveStack Lab — ADAS Learning Journey

Lộ trình tự học **ADAS / Autonomous Driving** tới mức L3 năng lực thực hành (định nghĩa trong `learning/SKILL_MATRIX.md`), xây dựng qua một dự án xuyên suốt: **DriveStack Lab — Modular ADAS & Autonomous Driving Learning System**.

Pipeline mục tiêu: Sensors/Datasets → Ingestion & Sync → Camera/LiDAR Perception → Fusion & Tracking → Localization → Prediction → Planning → Control → Simulation & Evaluation.

## Cấu trúc

| Đường dẫn | Nội dung |
|---|---|
| `learning/ROADMAP.md` | 6 phase, ngân sách giờ, kế hoạch dữ liệu & protocol đánh giá |
| `learning/SKILL_MATRIX.md` | Competency map L1–L3 + trạng thái có bằng chứng |
| `learning/DEPENDENCY_GRAPH.md` | Phụ thuộc giữa các module |
| `learning/PROJECT_ARCHITECTURE.md` | Kiến trúc, quy ước frame/đơn vị/clock, schema message |
| `learning/MODULE_GATES.md` | Tiêu chí qua từng module (≥ 85/100 + tiêu chí cốt lõi) |
| `learning/DIAGNOSTIC_TEST.md` | Bài kiểm tra đầu vào |
| `learning/LEARNING_LOG.md` | Nhật ký & checkpoint từng phiên |
| `learning/WEAKNESS_BACKLOG.md` | Lỗ hổng kiến thức & lịch ôn cách quãng |
| `learning/INTERVIEW_PREP.md` | Chuẩn bị phỏng vấn theo phase |
| `.claude/skills/adas-mentor/` | Skill Claude Code đóng vai mentor (dạy, chấm, gate) |
| `drivestack/` | Code dự án (tạo từ Module M1.1) |

## Trạng thái

Trước Phase 1 — đang làm Diagnostic Test. Xem `learning/LEARNING_LOG.md`.

## Môi trường

Ubuntu 22.04 · Python 3.10 · ROS 2 Humble (dự kiến) · RTX 3050 Laptop 4 GB VRAM.

## An toàn

Dự án học tập, chỉ chạy trên dataset, replay và simulator. Không dùng để điều khiển xe thật.
