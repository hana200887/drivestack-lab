# Review lộ trình DriveStack Lab — 2026-11-04

> **Trạng thái 2026-11-04:** đã áp dụng toàn bộ phát hiện #1–#7 và các điều chỉnh nhỏ vào `ROADMAP.md`, `PROJECT_ARCHITECTURE.md`, `MODULE_GATES.md`, `DEPENDENCY_GRAPH.md`, `SKILL_MATRIX.md`, `DIAGNOSTIC_TEST.md` và skill (`assessment.md`, `environment.md`). Mục "Thứ tự xử lý đề xuất" bước 2–5 vẫn còn hiệu lực — thực hiện sau khi có bài Diagnostic.

**Kết luận:** giữ cấu trúc 6 phase, dependency nền tảng và cơ chế gate. Khung hiện tại phù hợp để định hướng, nhưng chưa đủ căn cứ để cam kết hoàn thành toàn bộ trong 18–24 tuần. Cần làm rõ ngân sách giờ, dữ liệu đánh giá và mức tích hợp thực sự của capstone.

Đây là review thiết kế, chưa phải chấm năng lực hay lịch cá nhân hóa. Chưa có `learning/diagnostic/submission/`, `drivestack/` hoặc repository Git tại thư mục gốc; không có code/bài nộp để chạy test. Trạng thái người học vẫn là L0 — chưa đánh giá. Thông tin phần cứng lấy từ checkpoint 2026-11-03, chưa đo lại trong phiên này.

## Phần nên giữ

- Một dự án xuyên suốt với interface, frame, timestamp và unit được quy định sớm.
- Học transforms trước projection/fusion; linear KF trước EKF; detection/tracking trước prediction.
- Logic thuật toán độc lập ROS, node là lớp bọc; có regression sau mỗi lần tích hợp.
- Gate ≥ 85/100 kèm tiêu chí cốt lõi, không tự chuyển phase khi hết lịch.
- Phân biệt output model, ground truth và mock; có báo cáo metric và failure cases.

## Các phát hiện theo mức ưu tiên

P1 = cần xử lý trước khi dùng lộ trình để cam kết tiến độ hoặc kết quả kỹ thuật. P2 = cần làm rõ trước module liên quan.

### 1. [P1] Ngân sách 18 tuần không khớp khối lượng; lịch 24 tuần chưa có dự phòng rõ ràng

**Vị trí:** `ROADMAP.md`, bảng tổng quan và số giờ ở tiêu đề mỗi phase; mục thời gian Phase 1.

Tổng hiện tại: **110 + 110 + 135 + 110 + 110 + 85 = 660 giờ**.

| Phương án | Quỹ giờ ở 25–30 h/tuần | Hệ quả |
|---|---|---|
| 18 tuần | 450–540 giờ | Phải rút được 120–210 giờ, vẫn tính giờ thi gate |
| 24 tuần | 600–720 giờ | Cần trung bình 27,5 h/tuần cho đúng 660 giờ |
| Giữ đủ 660 giờ, cộng 20% dự phòng | 792 giờ | Khoảng 27–32 tuần ở 25–30 h/tuần |

20% là giả định lập kế hoạch trong review, không phải số liệu về tốc độ học của người học. Tài liệu chưa phân bổ rõ giờ cài môi trường, ôn tập, làm lại bài, gate và xử lý lỗi tích hợp nằm trong hay ngoài từng module.

Riêng fast track Phase 1: bỏ phần dạy M1.1/M1.2/M1.4 chỉ giảm **38 giờ**, còn **72 giờ**, vượt quỹ **50–60 giờ của 2 tuần**, ngay cả trước khi tính thêm giờ gate nếu chưa được bao gồm.

**Đề xuất:** coi 24 tuần là mục tiêu có điều kiện; chỉ xác nhận fast track sau Diagnostic và ước lượng lại từng module. Mỗi tuần ghi riêng giờ học mới, đánh giá/tích hợp và dự phòng. Không rút nền tảng chỉ để khớp lịch.

### 2. [P1] Chưa chọn dữ liệu và protocol đủ cụ thể cho các metric bắt buộc

**Vị trí:** `ROADMAP.md`, M1.7, M2.5–M2.8, M3.4–M3.5; `PROJECT_ARCHITECTURE.md`, module `io` và `eval`.

Các phần đang cùng ghi “KITTI/subset/replay” nhưng cần dữ liệu khác nhau. KITTI Raw có timestamp, OXTS và tracklets XML ở những drive được gán nhãn; Object Detection và Tracking có format/protocol riêng. Chọn vài ảnh Object Detection không tự tạo thành benchmark MOT có ID liên tục. Cần chọn adapter và nhãn đúng trước khi đặt gate HOTA/IDF1. Tham khảo [KITTI Raw](https://www.cvlibs.net/datasets/kitti/raw_data.php), [Object Detection](https://www.cvlibs.net/datasets/kitti/eval_object.php), [Tracking](https://www.cvlibs.net/datasets/kitti/eval_tracking.php).

**Đề xuất chốt trước từng phase:**

| Công việc | Dữ liệu / thông tin cần chốt |
|---|---|
| Replay + TF + đọc OXTS | Một drive KITTI Raw synced/rectified; calibration, timestamp và checksum |
| Detection 2D/3D | Subset có nhãn; danh sách train/val; class mapping, ignore rules và định nghĩa AP |
| Tracking 2D/3D | Sequence có track ID, như training split KITTI Tracking; converter và cấu hình evaluator |
| Segmentation | Subset có pixel masks và label mapping trước khi đặt gate mIoU |
| Báo cáo so sánh | Cố định split, checkpoint model, seed và phiên bản evaluator |

Kiểm tra overlap theo drive/sequence khi dùng nhiều nhánh dữ liệu; không chỉ kiểm tra tên file. Phân biệt AP kiểu KITTI và mAP kiểu COCO trong báo cáo. Ngân sách dung lượng cần tính cả file tải nén, dữ liệu giải nén và cache, không chỉ subset giữ lại.

### 3. [P1] Fallback Phase 6 có thể không chứng minh được closed-loop

**Vị trí:** `ROADMAP.md`, M6.1 và capstone; `PROJECT_ARCHITECTURE.md`, module `sim`.

M6.1 cho phép fallback bằng replay CARLA, trong khi mục tiêu Phase 6 là tích hợp toàn hệ thống trong simulator. Với replay cảm biến cố định, `ControlCommand` không làm thay đổi trạng thái xe hoặc quan sát kế tiếp, nên không kiểm chứng được phản hồi của planner/controller.

Máy được ghi nhận có 4 GB VRAM. Tài liệu **CARLA 0.9.15** nêu GPU tối thiểu 6 GB, khuyến nghị 8 GB. Điều này là rủi ro khả thi, chưa phải bằng chứng rằng mọi cấu hình nhẹ đều không chạy. Không nên để tới tuần 22 mới thử. [CARLA requirements](https://carla.readthedocs.io/en/0.9.15/start_quickstart/).

**Đề xuất:** sau khi qua gate ROS 2, dành một lần thử kỹ thuật có giới hạn thời gian để kiểm tra simulator, cảm biến và đường gửi lệnh. Chốt một trong hai deliverable:

- **Đích đầy đủ:** CARLA local hoặc remote, sensor → model → planning/control → xe → sensor, có đo riêng chất lượng closed-loop và tốc độ thực thi.
- **Đích dự phòng:** perception/fusion trên replay, cộng closed-loop 2D với stub ghi rõ nguồn gốc. Báo cáo hai kết quả riêng; không gọi đây là full sensor-to-control closed-loop.

Khi dùng CARLA, chốt synchronous mode/fixed timestep và ghép sensor theo frame. Đây là cấu hình phục vụ tái lập; việc simulator chờ client không tự chứng minh pipeline đạt real-time. [CARLA synchrony and time-step](https://carla.readthedocs.io/en/0.9.15/adv_synchrony_timestep/).

### 4. [P1] Thiếu bước chuyển từ tracks sang trạng thái vật thể dùng cho planning

**Vị trí:** `ROADMAP.md`, M3.5, M4.7, M5.4–M5.6; `PROJECT_ARCHITECTURE.md`, schema `Track`, `PredictedObject`, input `map` của planning.

`Track.state` hiện chỉ ghi `(n,)`. Lộ trình chưa chốt state 2D là pixel hay mét, vận tốc tương đối hay trong frame `map`, cách bù ego-motion, và nguồn route/map. Vì vậy câu “dùng tracks Phase 2/3” chưa đủ để nối tới prediction/planning theo mét.

Ví dụ cần kiểm thử: vật cản đứng yên có vị trí thay đổi trong frame xe khi ego chạy. Nếu lấy biến thiên đó làm vận tốc trong `map`, prediction sẽ sai dù tracker hoạt động đúng trong frame của nó.

**Đề xuất:** thêm deliverable tích hợp giữa M4.7 và M5.4: đưa 3D tracks và covariance về frame planning tại timestamp tương ứng; quy định rõ position/velocity/state layout, cách xử lý ego-motion và dữ liệu quá cũ. Không dùng trực tiếp pixel tracks để tính TTC theo mét. Với sim 2D, quy định map/route từ scenario; với CARLA, cần adapter tương ứng.

### 5. [P1] Công thức latency trộn các loại thời gian và các đại lượng khác nhau

**Vị trí:** `PROJECT_ARCHITECTURE.md`, mục 3 dòng quy ước thời gian; `MODULE_GATES.md`, M2.8 và M6.2.

Hiện quy định latency module = `publish_time − stamp_ns`. Nếu `publish_time` lấy từ system clock hiện tại còn `stamp_ns` thuộc dữ liệu KITTI năm 2011, kết quả không phải latency xử lý. Nếu cùng dùng ROS time khi replay pause/tăng tốc, phép trừ vẫn không đại diện cho thời gian tính toán thực. ROS phân biệt SystemTime, SteadyTime và ROSTime. [ROS 2 Clock and Time](https://design.ros2.org/articles/clock_and_time.html).

**Đề xuất phân biệt ba phép đo:**

- Processing duration: `end_steady − start_steady` tại cùng process/máy.
- Thời gian qua pipeline replay: đo từ lúc phát input đến lúc nhận output trên cùng clock đo; dùng trace ID. Nhiều máy phải quy định đồng bộ clock hoặc đo tại một đầu mối.
- Data age/sync: chênh lệch timestamp trong cùng clock domain ROS/simulation; báo rõ replay rate và cách ánh xạ thời gian.

Giữ capture stamp cho TF, tracking và fusion. Báo cáo latency p50/p95, throughput, tỷ lệ drop và real-time factor riêng khi có simulation.

### 6. [P2] Protocol EKF chưa phân biệt bài giả lập với đánh giá sensor fusion độc lập

**Vị trí:** `ROADMAP.md`, M4.3–M4.7; `MODULE_GATES.md`, M4.4.

Lộ trình cho phép thêm nhiễu nhân tạo vào OXTS, nhưng chưa ghi trường nào làm reference, trường nào làm measurement, hoặc nguồn wheel odometry. KITTI mô tả OXTS là hệ GPS/IMU kết hợp. Vì vậy không mặc định pose, velocity và các trường liên quan là những phép đo độc lập. [KITTI data paper](https://www.cvlibs.net/publications/Geiger2013IJRR.pdf).

**Đề xuất:** tách bài EKF có state thật/measurement sinh độc lập để kiểm tra tính đúng và consistency; bài OXTS có nhiễu ghi rõ là thí nghiệm synthetic quanh reference. Với dữ liệu thật, liệt kê input/reference và giả định tương quan. Odometry tự sinh phải mang nhãn mock. Gate NEES cần ghi số lần Monte Carlo, mức tin cậy và cách tổng hợp, thay vì chỉ “trong ngưỡng”.

### 7. [P2] Mục tiêu năng lực rộng hơn một số deliverable đang bắt buộc

**Vị trí:** `SKILL_MATRIX.md`, A8, B9, B10, D3; `ROADMAP.md`, M3.3–M3.4, Phase 6; `MODULE_GATES.md`, M5.3.

- B10 yêu cầu tối thiểu L2 có fine-tune nhỏ, còn M3.4 và môi trường cho phép chỉ inference/eval nếu thiếu VRAM.
- B9 ghi tự tính BEV/3D IoU, còn gate M3.3 chỉ bắt buộc BEV IoU.
- Docker đặt Learn Later nhưng matrix vẫn đặt tối thiểu L2 cho các dòng không có ★; Phase 6 chưa có deliverable tương ứng.
- M5.3 gate yêu cầu tự viết A*/RRT, nhưng phần thực hành roadmap chỉ nêu A*.

**Đề xuất:** lập bảng năng lực → artifact → gate để xác nhận phạm vi trước khi cá nhân hóa. Với mục tiêu nghề nghiệp trong master prompt, nên ưu tiên chiều sâu Perception và khả năng tích hợp; phần mở rộng như thêm tracker/controller hoặc tự viết thêm thuật toán chỉ giảm khi không làm mất mục tiêu đã chọn. Nếu đưa một năng lực sang Learn Later thì matrix và gate phải phản ánh quyết định đó.

## Các điều chỉnh nhỏ nên đi cùng lần sửa roadmap

- Đổi tiêu đề thành “L3 năng lực thực hành theo Skill Matrix” để định nghĩa mục tiêu ngay từ đầu; thang hiện tại được định nghĩa nội bộ trong `SKILL_MATRIX.md`.
- Bootstrap package ở M1.1, mở rộng skeleton ở M1.7: gate M1.1 đã yêu cầu `pip install -e .`, trong khi kiến trúc ghi tạo code ở M1.7.
- Quy định ranh giới fusion/tracking nhất quán: roadmap Phase 3 đi qua 3D MOT rồi fusion, kiến trúc lại có nhánh fusion → tracking. Chọn baseline để không ghép lại ID ngoài ý muốn.
- Chuẩn hóa cách chấm câu ★: phần A tối đa 2 điểm, phần B tối đa 7 điểm; không dùng chung cách diễn đạt “mọi câu cốt lõi đạt 2/2”. Diagnostic chỉ dẫn tới gate-only, không tự xác nhận đã qua gate.
- Giữ Humble cho môi trường Ubuntu 22.04 hiện tại, nhưng xem lại vòng đời hỗ trợ nếu lịch kéo dài: mốc hỗ trợ công bố là tháng 5/2027. [Thông báo ROS 2 Humble](https://discourse.ros.org/t/ros-2-humble-hawksbill-released/25729).

## Thứ tự xử lý đề xuất

1. Người học làm Diagnostic theo đề hiện tại; chưa bỏ/rút module khi chưa có bài nộp.
2. Khi chấm: xác định module học đủ, rút gọn hoặc gate-only; tính lại giờ, có giờ gate và dự phòng.
3. Cập nhật đồng bộ roadmap, matrix, gates và kiến trúc theo các quyết định về phạm vi; ưu tiên sửa contract thời gian và dữ liệu trước bài tích hợp đầu tiên.
4. Trước mỗi phase, chốt dataset/GT/evaluator và deliverable; thử khả thi simulator sớm sau gate ROS 2.
5. Sau hai tuần học thực tế, dùng giờ học và kết quả gate để ước lượng lại ngày hoàn thành.

**Kiểm chứng trong phiên:** đọc toàn bộ tài liệu `learning/` và skill vận hành; script Python cộng số giờ, tính quỹ 18/24 tuần và kiểm tra sự tồn tại bài nộp/code; đối chiếu các nguồn chính thức được dẫn ở từng phát hiện. Chưa chạy benchmark GPU, chưa cài phần mềm, chưa chạy test năng lực. `ROADMAP.md` được giữ nguyên; các đề xuất trên chưa được coi là lịch đã chốt.
