# DIAGNOSTIC TEST — kiểm tra đầu vào

> Bước 5 của master prompt. Mục đích **không phải** lấy điểm cao mà để biết chính xác phần nào có thể bỏ qua, rút gọn hay phải học đầy đủ. Một câu "Không biết" trung thực có giá trị hơn một câu đoán trúng.

## Cấu trúc & điểm

| Phần | Nội dung | Số lượng | Điểm | Thời gian gợi ý |
|---|---|---|---|---|
| A | Lý thuyết | 25 câu | 50 (2 điểm/câu) | 90–120 phút |
| B | Bài toán / code | 5 bài | 35 (7 điểm/bài) | 3–4 giờ |
| C | Tình huống debugging | 3 tình huống | 15 (5 điểm/tình huống) | 45–60 phút |
| | **Tổng** | | **100** | ~6 giờ, có thể chia 2 ngày |

**Phân bố độ khó** (33 mục): Dễ 9 · Trung bình 16 · Khó 8. Phần A: Dễ 8 · TB 11 · Khó 6. Phần B: B1 Dễ, B4 Khó, còn lại TB. Phần C: C1, C2 TB · C3 Khó.

## Luật làm bài

- **Phần A và C:** đóng sách, không AI, không tra cứu.
- **Phần B:** được xem tài liệu chính thức (docs.python.org, numpy.org, cppreference.com); **không AI, không tìm lời giải có sẵn**.
- Với mỗi câu phần A, ghi **độ tự tin 1–3** (1 = đoán, 2 = khá chắc, 3 = chắc chắn). Không tính điểm nhưng dùng để phát hiện hiểu sai (sai + tự tin 3) và trúng may (đúng + tự tin 1).
- Ghi thời gian thực tế cho từng phần.
- Viết "Không biết" khi không biết. Không bị trừ điểm, và giúp lộ trình chính xác hơn.

## Cách nộp

```
learning/diagnostic/submission/
├── NOTES.md        # thời gian từng phần, lệnh tạo môi trường đã dùng, chỗ bị kẹt
├── answers_A.md    # A1..A25: câu trả lời + (tự tin: x/3)
├── answers_C.md    # C1..C3
└── B/              # code + test của phần B
```

**Bước 0 (thuộc bài kiểm tra):** tự tạo một Python virtual environment trong project và cài `numpy`, `pytest`. Ghi lại các lệnh đã dùng vào `NOTES.md`. Nếu kẹt quá 20 phút, ghi lỗi gặp phải rồi hỏi hint — việc kẹt ở đây cũng là dữ liệu chẩn đoán.

Làm xong, gõ: **"nộp diagnostic"**. Mentor sẽ tự chạy test của bạn, chấm theo rubric và cá nhân hóa `ROADMAP.md`.

---

## PHẦN A — Lý thuyết (25 câu × 2 điểm)

★ = câu cốt lõi (ảnh hưởng trực tiếp quyết định bỏ qua module).

### Engineering

**A1** (Dễ) Vì sao phép tính trên `numpy.ndarray` thường nhanh hơn nhiều so với vòng lặp Python trên `list` cùng dữ liệu? Nêu ít nhất hai lý do.

**A2** (TB) Đoạn code sau in ra gì? Giải thích, và nêu một tình huống trong pipeline xử lý frame cảm biến mà lỗi này gây hậu quả.

```python
def add_point(p, cloud=[]):
    cloud.append(p)
    return cloud

a = add_point(1)
b = add_point(2)
print(a, b, a is b)
```

**A3** (Dễ) Bạn đã `push` lên nhánh chung một commit làm hỏng test. Dùng lệnh nào để hoàn tác một cách an toàn? Vì sao không nên `git reset --hard` rồi `push --force`?

**A4** (Dễ) Lệnh `source /opt/ros/humble/setup.bash` làm gì? Vì sao khi mở terminal mới lại phải chạy lại?

**A5** (TB) Trong C++, khác nhau giữa truyền tham số `std::vector<Point> pts`, `std::vector<Point>& pts` và `const std::vector<Point>& pts`? Khi nào dùng cái nào? "Dangling reference" là gì — cho một ví dụ.

### Toán nền

**A6** (Dễ) Một ma trận xoay 3×3 hợp lệ `R` có những tính chất gì? Nghịch đảo của nó bằng gì, vì sao? Nếu `det(R) = −1` thì chuyện gì đang xảy ra?

**A7** ★ (TB) Vì sao dùng ma trận đồng nhất (homogeneous) 4×4 `T = [R t; 0 1]` để biểu diễn pose? Viết công thức `T⁻¹` theo `R` và `t` (không dùng `np.linalg.inv`).

**A8** ★ (Khó) Biết `T_map_base` (pose xe trong frame map) và `T_base_lidar` (vị trí lắp LiDAR trên xe). Viết công thức đưa một điểm `p_lidar` về frame `map`. Nếu nhân sai thứ tự hai ma trận, triệu chứng trên bản đồ sẽ trông như thế nào khi xe chạy thẳng so với khi xe rẽ?

**A9** ★ (TB) Ma trận covariance 2×2 của vị trí (x, y) là gì? Phần tử ngoài đường chéo khác 0 nói lên điều gì? Vẽ (mô tả) hình dạng vùng bất định trong hai trường hợp: ngoài đường chéo = 0 và ≠ 0.

**A10** (Khó) Một detector người đi bộ có recall 90% và tỉ lệ báo nhầm (false positive rate) 5% trên mỗi ô lưới. Xác suất tiên nghiệm có người đi bộ trong một ô là 1%. Khi detector báo "có người" ở một ô, xác suất thực sự có người là bao nhiêu? Kết quả này nói gì về thiết kế hệ thống?

### Camera & Deep Learning

**A11** (Dễ) Trong mô hình pinhole, `fx, fy, cx, cy` là gì và đơn vị gì? Viết công thức tính `u` từ điểm `(X, Y, Z)` trong frame camera.

**A12** ★ (TB) Phân biệt intrinsic và extrinsic parameters. Reprojection error là gì? Vì sao reprojection error thấp **chưa chắc** đã là calibration tốt?

**A13** (Dễ) IoU là gì? Mô tả thuật toán NMS. NMS có thể gây lỗi gì trong cảnh đông người đi bộ?

**A14** (TB) Định nghĩa precision và recall. Với hệ thống phanh khẩn cấp tự động (AEB) phát hiện người đi bộ, false negative và false positive mỗi loại gây hậu quả gì? Bạn cân bằng hai loại lỗi bằng cách nào?

**A15** (TB) Khi train detector, train loss giảm đều nhưng validation loss tăng từ epoch 15. Hiện tượng gì? Nêu 3 cách xử lý. Nếu bạn chia train/val ngẫu nhiên theo **frame** từ các video lái xe, có vấn đề gì?

**A16** (TB) Vì sao cần tracking sau detection? "ID switch" là gì? Một tracker kiểu SORT ghép detection với track dựa trên những gì?

### LiDAR & 3D

**A17** (Dễ) Một điểm LiDAR thường chứa những thông tin gì? Vì sao point cloud càng xa càng thưa?

**A18** (TB) Một 3D bounding box 7-DoF gồm những tham số nào? Theo kinh nghiệm annotation của bạn: label 3D của KITTI được biểu diễn trong frame nào, và cần lưu ý gì khi đổi sang frame LiDAR?

**A19** (Khó) Vì sao các model như PointPillars chuyển point cloud thành pillars / ảnh BEV (Bird's-Eye View) thay vì xử lý trực tiếp từng điểm? Kích thước voxel/pillar ảnh hưởng gì đến kết quả?

**A20** ★ (Khó) Liệt kê đầy đủ các bước để vẽ điểm LiDAR lên ảnh camera, kể cả các bước lọc/kiểm tra. Nêu ít nhất 2 nguyên nhân khiến điểm bị lệch khỏi vật thể dù code "đúng".

### Localization

**A21** (Dễ) Vì sao sai số odometry tích lũy theo thời gian? Sai số góc heading nhỏ ảnh hưởng thế nào tới sai số vị trí sau 1 km?

**A22** ★ (TB) Mô tả bước predict và update của Kalman Filter bằng lời. `Q` và `R` đại diện cho gì? Nếu đặt `R` quá nhỏ so với thực tế thì estimate sẽ ra sao?

**A23** (Khó) Vì sao localization cho xe (trạng thái có góc yaw) cần EKF thay vì KF tuyến tính? Jacobian đóng vai trò gì? Khi nào EKF hoạt động kém?

### Planning, Control & System

**A24** (TB) Khác nhau giữa Dijkstra và A*? Heuristic "admissible" là gì, vì sao quan trọng? Trên lưới cho phép đi chéo (8 hướng, chi phí chéo √2), dùng Manhattan distance có vấn đề gì?

**A25** ★ (Khó) Xe chạy 20 m/s, pipeline perception có độ trễ 120 ms. Trong thời gian trễ đó xe đi được bao xa? Vì sao timestamp của message phải là **thời điểm cảm biến chụp** chứ không phải thời điểm xử lý? Nêu một cách đồng bộ camera 30 Hz với LiDAR 10 Hz.

---

## PHẦN B — Bài toán / code (5 bài × 7 điểm)

Mỗi bài: code + test của **chính bạn** trong `submission/B/`. Mentor sẽ chạy thêm test ẩn.
Rubric mỗi bài (7 điểm): đúng (3) · test & edge cases (2) · chất lượng code — vectorization, type hints, dễ đọc (1) · giải thích / độ phức tạp (1).

### B1 ★ (Dễ–TB) — Transform points

```python
def transform_points(T_a_b: np.ndarray, points_b: np.ndarray) -> np.ndarray:
    """Đưa điểm từ frame b sang frame a. T_a_b: (4,4); points_b: (N,3) -> (N,3). Không dùng vòng lặp Python."""

def invert_transform(T_a_b: np.ndarray) -> np.ndarray:
    """Trả về T_b_a, dùng R^T, không dùng np.linalg.inv."""
```

Viết ít nhất 4 test: identity; round-trip `invert(T) @ T ≈ I`; một điểm cụ thể với phép xoay yaw 90° quanh z + tịnh tiến (tự tính đáp án bằng tay); input rỗng `(0, 3)`.

### B2 ★ (TB) — Project LiDAR lên ảnh

```python
def project_lidar_to_image(points_lidar, K, T_cam_lidar, image_size):
    """points_lidar (N,3) frame LiDAR (x trước, y trái, z lên).
    T_cam_lidar (4,4): LiDAR -> camera optical (x phải, y xuống, z trước).
    image_size = (W, H). Trả về (uv (M,2), depth (M,), mask (N,) bool)."""
```

Dữ liệu: `K = [[700,0,640],[0,700,360],[0,0,1]]`, ảnh 1280×720,
`R_cam_lidar = [[0,-1,0],[0,0,-1],[1,0,0]]`, `t_cam_lidar = [0, -0.2, 0]`.

1. **Tính tay trước** (ghi vào answers): điểm `p_lidar = (10, −2, −1)` rơi vào pixel nào?
2. Cài hàm; test gồm: điểm trên, một điểm phía sau xe `(−5, 0, 0)`, một điểm có `z_cam = 0`, một điểm nằm ngoài khung ảnh.

### B3 (TB) — IoU & NMS

```python
def iou_xyxy(a: np.ndarray, b: np.ndarray) -> float: ...
def nms(boxes: np.ndarray, scores: np.ndarray, iou_thresh: float) -> list[int]:
    """Greedy NMS. Trả về chỉ số box giữ lại, sắp theo score giảm dần."""
```

Quy ước tọa độ liên tục: diện tích = `(x2−x1)·(y2−y1)`. Test bắt buộc: không giao nhau; một box nằm trong box kia; hai box chỉ chạm cạnh; box diện tích 0; ví dụ `boxes=[[0,0,10,10],[1,1,11,11],[20,20,30,30]]`, `scores=[0.9,0.8,0.7]`, `iou_thresh=0.5` (tự tính kết quả mong đợi).

### B4 ★ (TB–Khó) — Kalman Filter 1D vận tốc không đổi

Sinh dữ liệu: `rng = np.random.default_rng(42)`, `dt = 0.1`, 50 bước, vị trí thật `x_k = 2.0 * k * dt` (vận tốc 2 m/s), đo `z_k = x_k + N(0, 0.5²)`.

1. Tự cài KF với trạng thái `[x, v]`: viết rõ `F, H, Q, R, x0, P0` và giải thích lựa chọn.
2. In RMSE (so với vị trí thật) của: đo thô, và estimate của KF.
3. Chạy lại với `R` nhỏ đi 100 lần và lớn lên 100 lần. Mô tả và giải thích kết quả.

### B5 (TB) — A* trên lưới

Viết bằng **Python hoặc C++** (chọn C++ nếu muốn được đánh giá C++ ngay; nếu chọn Python, năng lực C++ sẽ được kiểm tra ở M1.5).

`astar(grid, start, goal) -> list[(row, col)] | None` — 4 hướng, chi phí 1/bước, heuristic Manhattan, `grid` gồm 0 = trống, 1 = vật cản.

```
grid = [
  [0,0,0,0,0],
  [1,1,1,1,0],
  [0,0,0,0,0],
  [0,1,1,1,1],
  [0,0,0,0,0],
]
start = (0,0), goal = (4,4)
```

Test: lưới trên (đường đi ngắn nhất có bao nhiêu bước?); goal bị bao kín → `None`; `start == goal`; goal là ô vật cản. Nêu độ phức tạp thời gian.

---

## PHẦN C — Tình huống debugging (3 × 5 điểm)

Mỗi tình huống trả lời 4 ý: **(1) nguyên nhân** (2 điểm) · **(2) cách kiểm chứng giả thuyết** — chạy/log/vẽ gì (1) · **(3) cách sửa** (1) · **(4) test chống tái phát** (1).

### C1 (TB) — Điểm LiDAR không khớp ảnh

Một đồng nghiệp viết hàm sau. Khi overlay lên ảnh: các điểm không khớp vật thể, có một đám điểm xuất hiện trên ảnh dù đó là vùng **phía sau** xe, và khi xe rẽ trái thì các điểm trượt lệch theo hướng ngược lại.

```python
def project(points_lidar, K, T_lidar_cam):
    # T_lidar_cam: pose của camera trong frame LiDAR (p_lidar = T_lidar_cam @ p_cam)
    pts_h = np.hstack([points_lidar, np.ones((len(points_lidar), 1))])
    pts_cam = (T_lidar_cam @ pts_h.T).T[:, :3]
    uv = (K @ pts_cam.T).T
    uv = uv[:, :2] / uv[:, 2:3]
    mask = (uv[:, 0] >= 0) & (uv[:, 0] < W) & (uv[:, 1] >= 0) & (uv[:, 1] < H)
    return uv[mask]
```

### C2 (TB) — Vận tốc ước lượng vô lý

Tracker ước lượng vận tốc bằng `v = (x - x_prev) / (t - t_prev)`. Dữ liệu được phát lại bằng `ros2 bag play ... --rate 2.0`. Xe phía trước thực tế chạy đều khoảng 10 m/s. Log:

```
[tracker] recv_wall=1730000000.000 stamp=1317380000.000 track=7 x=20.00
[tracker] recv_wall=1730000000.050 stamp=1317380000.100 track=7 x=21.00 v_est=20.0
[tracker] recv_wall=1730000000.100 stamp=1317380000.200 track=7 x=22.00 v_est=20.0
[tracker] recv_wall=1730000000.101 stamp=1317380000.300 track=7 x=23.00 v_est=1000.0
[tracker] recv_wall=1730000000.200 stamp=1317380000.400 track=7 x=24.00 v_est=10.1
```

Giải thích **từng** giá trị `v_est` bất thường.

### C3 (Khó) — Xe lắc ngang ở tốc độ cao (trong simulator)

Controller Pure Pursuit bám làn tốt ở 5 m/s. Ở 25 m/s, xe lắc ngang với biên độ tăng dần rồi văng khỏi làn. Log cho thấy góc lái có lúc đạt 0.9 rad. Simulator có độ trễ actuator khoảng 0.2 s.

```yaml
controller: pure_pursuit
lookahead_m: 2.0
max_steer: 35
control_rate_hz: 10
target_speed_mps: 25
wheelbase_m: 2.7
```

```python
alpha = math.atan2(target_y, target_x)            # target trong frame base_link
delta = math.atan2(2 * cfg.wheelbase_m * math.sin(alpha), cfg.lookahead_m)
delta = float(np.clip(delta, -cfg.max_steer, cfg.max_steer))
```

Có **nhiều hơn một** nguyên nhân. Liệt kê theo mức độ ảnh hưởng và nêu thí nghiệm để tách riêng từng nguyên nhân.

---

## Bảng ánh xạ câu → domain → module (dùng khi cá nhân hóa)

| Domain | Câu | Module liên quan |
|---|---|---|
| Engineering (Python, Linux, Git, C++, testing) | A1–A5, B1 (chất lượng), B3, B5, Bước 0 | M1.1, M1.2, M1.5 |
| Toán: transforms & xác suất | A6–A10, B1, B4 | M1.3, M1.4 |
| Camera & Deep Learning | A11–A16, B2, B3, C1 | M2.1–M2.7 |
| LiDAR & 3D | A17–A20, B2, C1 | M3.1–M3.6 |
| Localization & estimation | A9, A21–A23, B4 | M2.7, M4.x |
| Planning, control & system | A24, A25, B5, C2, C3 | M5.x, M6.2 |

Câu cốt lõi ★: A7, A8, A9, A12, A20, A22, A25, B1, B2, B4.

**Luật quyết định** (áp dụng sau khi chấm):
- Câu ★ "đạt" = phần A: **2/2**; phần B: **≥ 6/7** và không mắc lỗi cốt lõi của bài (ví dụ B1 dùng `−t` thay `−Rᵀt`; B2 chia cho z trước khi lọc `z > 0`).
- Domain ≥ 85% và mọi câu ★ trong domain đạt → **gate-only**: bỏ phần dạy nhưng **vẫn phải thi Module Gate đầy đủ**. Diagnostic không bao giờ tự xác nhận đã qua gate.
- 60–84% → **rút gọn** (chỉ học phần hổng); < 60% → **học đầy đủ**.
- Đúng nhưng tự tin 1/3 → kiểm tra lại bằng câu biến thể. Sai nhưng tự tin 3/3 → ưu tiên số 1 trong `WEAKNESS_BACKLOG.md`.
- Sau khi áp luật: tính lại giờ từng module (gồm giờ thi gate) + 15% dự phòng, rồi mới chọn lịch 24 tuần / fast track / dài hơn (`ROADMAP.md` §0.1).
