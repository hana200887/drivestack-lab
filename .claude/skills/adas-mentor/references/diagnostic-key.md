# Diagnostic key — đáp án & hướng chấm

> Dùng để chấm `learning/DIAGNOSTIC_TEST.md`. **Không đưa cho người học trước khi họ nộp bài.** Sau khi chấm, chỉ giải thích những câu sai/thiếu, theo kiểu chỉ ra sai ở đâu → vì sao → cách đúng.

Thang phần A: 0 = sai/không biết · 1 = đúng một phần hoặc đúng kết quả nhưng không giải thích · 2 = đúng + giải thích bản chất. Đối chiếu với độ tự tin để đánh dấu misconception (sai + 3/3) và trúng may (đúng + 1/3).

## Phần A

**A1** Bộ nhớ liên tục cùng dtype (không phải mảng con trỏ tới object Python); vòng lặp chạy trong C/SIMD, không có overhead interpreter + kiểm tra kiểu từng phần tử; cache-friendly. 2 điểm cần ≥ 2 lý do đúng bản chất.

**A2** In `[1, 2] [1, 2] True`. Default argument được tạo **một lần** khi định nghĩa hàm, dùng chung giữa các lần gọi. Sửa: `cloud=None` rồi tạo list mới. Hậu quả ADAS: buffer điểm/detection của frame trước "rò" sang frame sau → vật cản ma, bộ nhớ tăng dần. Thiếu ví dụ ADAS → 1 điểm.

**A3** `git revert <sha>` tạo commit mới đảo ngược thay đổi, giữ lịch sử. Reset + force push viết lại lịch sử chung → clone của người khác lệch, có thể mất commit của họ.

**A4** `source` chạy script trong **shell hiện tại**, đặt biến môi trường (PATH, AMENT_PREFIX_PATH, PYTHONPATH, LD_LIBRARY_PATH...). Terminal mới là process mới, không kế thừa biến từ terminal khác → chạy lại hoặc thêm vào `~/.bashrc` (lưu ý xung đột khi có nhiều workspace/venv).

**A5** By value: copy (đắt với vector lớn), sửa không ảnh hưởng gốc. `&`: không copy, hàm được sửa gốc (dùng khi output-parameter). `const&`: không copy, không sửa — mặc định cho input lớn chỉ đọc. Dangling: tham chiếu tới biến cục bộ được trả về từ hàm; tham chiếu/iterator tới phần tử vector sau `push_back` gây reallocation.

**A6** `RᵀR = I`, `det R = +1`, các cột trực chuẩn (là các trục frame đã xoay biểu diễn trong frame gốc). `R⁻¹ = Rᵀ` do trực giao. `det = −1` → phép phản xạ (đổi handedness) — thường do nhầm quy ước trục (ví dụ đổi dấu một trục).

**A7** ★ Gộp xoay + tịnh tiến thành một phép nhân ma trận → ghép chuỗi transform bằng nhân liên tiếp, đồng nhất cho điểm. `T⁻¹ = [Rᵀ, −Rᵀt; 0 1]`. Viết `[Rᵀ, −t]` → 0 điểm cho phần công thức (lỗi rất phổ biến, ghi backlog).

**A8** ★ `p_map = T_map_base @ T_base_lidar @ p_lidar` (chỉ số trong khớp nhau). Sai thứ tự → kết quả sai vì nhân ma trận không giao hoán. Triệu chứng: khi xe chạy thẳng ở heading ≈ 0 lỗi có thể nhỏ/khó thấy (chỉ lệch đúng bằng offset lắp đặt bị xoay sai); khi xe rẽ/heading thay đổi, lỗi thay đổi theo heading → bản đồ bị "nhòe", tường bị nhân đôi/xoay lệch. 2 điểm cần: công thức đúng + nhận ra lỗi phụ thuộc pose/heading.

**A9** ★ `Σ = [[σx², σxy],[σxy, σy²]]`, đơn vị m². Ngoài đường chéo: tương quan giữa sai số x và y. = 0 → ellipse thẳng theo trục; ≠ 0 → ellipse nghiêng (ví dụ GNSS/odometry sai nhiều theo hướng di chuyển, đường đi chéo so với trục). Thiếu ý nghĩa hình học → 1 điểm.

**A10** `P = 0.9·0.01 / (0.9·0.01 + 0.05·0.99) = 0.009 / 0.0585 ≈ 0.154` (~15%). Ý nghĩa: base rate thấp làm một lần phát hiện đơn lẻ không đáng tin → cần xác nhận nhiều frame (tracking), fusion nhiều cảm biến, prior từ bản đồ/ngữ cảnh. Số đúng mà không có ý nghĩa thiết kế → 1 điểm.

**A11** `fx, fy`: tiêu cự tính bằng **pixel** (= f / kích thước pixel); `cx, cy`: principal point (pixel). `u = fx·X/Z + cx`. Đổi độ phân giải ảnh → K phải scale theo.

**A12** ★ Intrinsic: thuộc tính bên trong camera (K, distortion). Extrinsic: pose camera so với frame khác (R, t). Reprojection error: khoảng cách pixel giữa góc checkerboard quan sát và góc chiếu lại bằng tham số ước lượng (thường báo RMS). Thấp chưa chắc tốt: ảnh calib thiếu đa dạng (chỉ ở giữa ảnh, không nghiêng, không ra mép) → distortion ở mép sai; overfit; board không phẳng; số đo kích thước ô sai làm scale extrinsic sai dù reprojection vẫn thấp.

**A13** IoU = diện tích giao / diện tích hợp. NMS: sắp theo score, giữ box cao nhất, loại các box có IoU > ngưỡng với nó, lặp. Cảnh đông: hai người đứng sát/chồng → box đúng bị loại (FN). Giải pháp: Soft-NMS, tune ngưỡng, NMS theo class.

**A14** Precision = TP/(TP+FP), recall = TP/(TP+FN). FN → không phanh → va chạm. FP → phanh ma → nguy cơ bị tông đuôi, mất niềm tin, người dùng tắt tính năng. Cả hai đều nguy hiểm. Cân bằng: ngưỡng score theo vùng/khoảng cách, xác nhận nhiều frame, fusion LiDAR/radar, phân tầng cảnh báo → phanh nhẹ → phanh mạnh. Trả lời "recall quan trọng hơn" mà không nhắc tác hại FP → 1 điểm.

**A15** Overfitting. Xử lý: augmentation / thêm dữ liệu, regularization (weight decay, dropout), early stopping, model nhỏ hơn, giảm lr. Chia theo frame ngẫu nhiên: các frame liên tiếp gần như giống hệt → **data leakage**, val lạc quan giả tạo → phải chia theo sequence/scene/ngày/vị trí.

**A16** Detection mỗi frame độc lập: không có danh tính, nhiễu, có frame bị sót. Tracking: gán ID, làm mượt, ước lượng vận tốc (cần cho prediction), chịu được sót tạm thời. ID switch: một track bị gán sang vật thể khác (ví dụ hai người đi cắt nhau). SORT: KF dự đoán vị trí → cost IoU giữa dự đoán và detection → Hungarian + ngưỡng (gating); track birth/death theo số lần hit/miss.

**A17** x, y, z (từ range + góc), intensity/reflectivity, thường có ring/laser id, đôi khi timestamp từng điểm. Thưa theo khoảng cách: độ phân giải góc cố định → khoảng cách giữa các tia tăng tỉ lệ với khoảng cách → vật ở xa có rất ít điểm.

**A18** (x, y, z) tâm, (l, w, h), yaw quanh trục thẳng đứng. KITTI label: frame **camera rectified** (x phải, y xuống, z trước); `location` là **tâm đáy** box; `rotation_y` quanh trục y camera; thứ tự kích thước `h, w, l`. Đổi sang LiDAR cần `R0_rect` và `Tr_velo_to_cam` (nghịch đảo), dời tâm lên h/2, đổi yaw (≈ `−ry − π/2`). 2 điểm cần nêu frame camera + ít nhất một bẫy (tâm đáy, thứ tự kích thước, yaw).

**A19** Point cloud không có thứ tự, thưa, số điểm thay đổi → chuyển về lưới đều để dùng 2D CNN nhanh, trưởng thành; BEV ít che khuất, kích thước vật không đổi theo khoảng cách. Voxel nhỏ → chi tiết hơn (tốt cho người đi bộ) nhưng tốn bộ nhớ/tính toán; voxel lớn → nhanh nhưng mất vật nhỏ, sai số lượng tử hóa. Pillars bỏ chia theo z → nhanh nhưng mất chi tiết theo chiều cao.

**A20** ★ Bước: (1) điểm LiDAR → homogeneous; (2) nhân `T_cam_lidar` (KITTI: `R0_rect · Tr_velo_to_cam`); (3) **lọc z_cam > 0** (hoặc > ngưỡng nhỏ); (4) chiếu bằng K: chia cho z; (5) distortion nếu ảnh chưa rectify; (6) lọc trong khung ảnh; (7) (tô màu theo depth). Nguyên nhân lệch khi code đúng: extrinsic sai/cũ; lệch thời gian giữa LiDAR quét và camera chụp khi xe/vật chuyển động (rolling shutter, motion distortion của LiDAR quay); parallax/che khuất (LiDAR thấy điểm mà camera bị che); dùng sai ảnh (rectified vs raw) hoặc sai K. 2 điểm cần bước lọc z > 0 + ≥ 2 nguyên nhân hợp lý.

**A21** Odometry tích phân chuyển động tương đối; mỗi bước có sai số (trượt bánh, sai bán kính, bias IMU) và không có tham chiếu tuyệt đối để sửa → tích lũy. Sai heading δθ → sai vị trí ngang ≈ d·δθ (ví dụ 1° ≈ 0.0175 rad → ~17 m sau 1 km), và nếu là bias liên tục thì sai số tăng nhanh hơn tuyến tính.

**A22** ★ Predict: đẩy trạng thái theo mô hình chuyển động, P tăng thêm Q. Update: so đo với dự đoán (innovation), Kalman gain cân hai nguồn theo độ bất định, P giảm. Q: bất định của mô hình (gia tốc không mô hình hóa...). R: nhiễu đo. R quá nhỏ → tin đo quá mức → estimate bám theo nhiễu, covariance quá tự tin (inconsistent), gating có thể loại đo hợp lệ sau đó.

**A23** Mô hình chuyển động phi tuyến (`x += v·cosθ·dt`), đo có thể phi tuyến (range/bearing, chuyển frame). KF chỉ đúng với hệ tuyến tính + Gaussian. EKF tuyến tính hóa bằng Jacobian quanh estimate hiện tại để lan truyền covariance. Kém khi phi tuyến mạnh trong một bước (dt lớn, quay nhanh), estimate ban đầu sai xa, nhiễu không Gaussian/đa mode → sai covariance, phân kỳ.

**A24** Dijkstra mở rộng theo chi phí đã đi g, lan đều mọi hướng. A* dùng f = g + h, h hướng về đích → mở rộng ít node hơn. Admissible: h không bao giờ ước lượng quá chi phí thật → đảm bảo tối ưu. Lưới 8 hướng với bước chéo √2: Manhattan **ước lượng quá** (đi chéo 1 bước tốn √2 < 2) → không admissible → có thể ra đường không tối ưu; dùng octile/Euclidean. h = 0 → Dijkstra.

**A25** ★ 20 × 0.12 = **2.4 m** (cộng thêm tuổi dữ liệu cảm biến: LiDAR quét 100 ms). Timestamp phải là thời điểm chụp vì mọi phép ghép dữ liệu (fusion, TF lookup pose xe, tính vận tốc) cần biết thế giới **lúc đó** ra sao; dùng thời điểm xử lý → ghép nhầm frame, vận tốc sai, phụ thuộc tải CPU. Đồng bộ: `message_filters` ApproximateTime theo stamp, chọn ảnh gần thời điểm quét nhất (hoặc trigger phần cứng), bù chuyển động (motion compensation) bằng pose xe nội suy theo stamp. 2 điểm cần: 2.4 m + lý do + một cách đồng bộ hợp lý.

## Phần B (7 điểm/bài: đúng 3 · test 2 · chất lượng 1 · giải thích 1)

Tự chạy test của người học, sau đó chạy kiểm tra ẩn (viết nhanh trong scratchpad, không ghi vào thư mục bài của họ).

**B1** ★ `points_b @ R.T + t` hoặc homogeneous; `invert = [Rᵀ, −Rᵀt]`. Kiểm tra ẩn: transform ngẫu nhiên (rotation hợp lệ qua QR) round-trip ≈ 1e-9; N=0 trả về `(0,3)`; không dùng vòng lặp. Lỗi cốt lõi: dùng `−t` thay `−Rᵀt`.

**B2** ★ Tính tay: `R·p = (2, 1, 10)`, cộng t → `(2, 0.8, 10)` → **u = 780, v = 416**. `(−5,0,0)` → z_cam = −5 → loại. `(0,1,0)` → z_cam = 0 → phải loại **trước khi chia** (không được ra inf/nan hoặc warning bị bỏ qua). Mask có kích thước N theo input gốc. Chia cho z trước khi lọc z > 0 = trượt tiêu chí cốt lõi.

**B3** IoU ví dụ: box0 vs box1 = 81 / 119 ≈ 0.68 > 0.5 → kết quả `[0, 2]`. Chạm cạnh → IoU 0; box diện tích 0 → không chia cho 0 (union = 0 phải xử lý). Box chứa nhau → IoU = diện tích nhỏ / diện tích lớn.

**B4** ★ `F = [[1, dt],[0, 1]]`, `H = [[1, 0]]`, `R = 0.25`, Q dạng white-noise acceleration (σ_a nhỏ, ví dụ 0.1–1) có giải thích; P0 lớn khi chưa biết v. RMSE KF phải **nhỏ hơn rõ rệt** RMSE đo thô (~0.5). R nhỏ 100× → bám nhiễu, RMSE gần đo thô; R lớn 100× → mượt nhưng phản ứng chậm (ở đây chuyển động đều nên có thể vẫn tốt — người học giải thích được vì sao mới đủ điểm giải thích). Kiểm tra seed được dùng đúng để tái lập.

**B5** Lưới mẫu: đường ngắn nhất **16 bước (17 ô)**. Bao kín → None; start == goal → `[start]`; goal là vật cản → None. Độ phức tạp O(V log V) với heap (V = số ô). Kiểm tra: không mở rộng lại node đã đóng sai cách; tie-breaking không làm sai tối ưu. Nếu viết C++: build được bằng lệnh nêu trong NOTES (g++/cmake), const-correctness, dùng `std::priority_queue`.

## Phần C (5 điểm: nguyên nhân 2 · kiểm chứng 1 · sửa 1 · test 1)

**C1** Nguyên nhân chính: (a) **sai chiều transform** — `T_lidar_cam` biến camera→LiDAR; cần `T_cam_lidar = inv(T_lidar_cam)` → giải thích lệch và "trượt ngược khi rẽ" (rotation bị đảo); (b) **không lọc z_cam > 0** trước khi chia → điểm phía sau bị lật dấu và rơi vào ảnh; (phụ) `W, H` biến toàn cục; z = 0 chia cho 0. 2 điểm cần cả (a) và (b). Kiểm chứng: một điểm đã biết (ví dụ góc bảng/xe gần) tính tay; in phân bố `z_cam`; tô màu điểm theo dấu z. Test: điểm phía sau phải bị loại; round-trip extrinsic; điểm đã biết → pixel đã biết.

**C2** `dt` lấy theo **wall clock lúc nhận** thay vì `stamp`. Bag phát ×2 → dt wall = 0.05 s thay vì 0.1 s → v_est = 20 (gấp đôi). Dòng 1000: hai message đến gần như cùng lúc (queue/callback dồn, CPU bận) → dt wall = 0.001 s. Dòng 10.1: dt wall = 0.099 s trùng gần đúng. Kiểm chứng: log cả stamp và wall, tính dt theo từng cái; phát lại ở `--rate 1.0` và 0.5 → v_est đổi theo rate = bằng chứng. Sửa: dùng `stamp` (header), xử lý dt ≤ 0 / message đến sai thứ tự, dùng `use_sim_time` khi replay. Test: cùng stamp, wall time ngẫu nhiên → v = 10 m/s.

**C3** Theo mức độ: (1) **`max_steer: 35` là độ nhưng dùng như radian** → clamp vô tác dụng (35 rad), cho phép 0.9 rad (~52°); (2) **lookahead cố định 2 m ở 25 m/s** → gain lái quá cao → dao động; nên `ld = clip(k·v, ld_min, ld_max)`; (3) **trễ actuator 0.2 s + control 10 Hz** → lệch pha → dao động tăng dần ở tốc độ cao; (4) không giới hạn tốc độ đổi góc lái (rate limit). 2 điểm cần (1) và (2) + nhắc tới delay. Thí nghiệm tách: sửa đơn vị riêng → đo lại; quét ld theo v; tắt delay trong sim; tăng control rate; vẽ CTE theo thời gian ở nhiều tốc độ. Test: unit test clamp ≤ 0.61 rad; quy ước config `_deg` / `_rad`; test closed-loop nhiều tốc độ yêu cầu CTE bị chặn và không tăng dần.

## Sau khi chấm

1. Tính điểm theo domain bằng bảng ánh xạ trong đề.
2. Áp luật quyết định trong `assessment.md` §4; ghi lý do từng quyết định kèm câu làm bằng chứng.
3. Cập nhật `SKILL_MATRIX.md` (cột Diagnostic), `ROADMAP.md` (lịch cá nhân hóa + bảng lịch sử điều chỉnh), `WEAKNESS_BACKLOG.md` (misconception trước), `LEARNING_LOG.md`.
4. Mở buổi học đầu tiên của module ưu tiên cao nhất — chỉ một buổi.
