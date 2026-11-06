# Assessment — rubric, gates, cá nhân hóa, remediation

## Mục lục
1. Quy tắc bằng chứng
2. Rubric Module Gate (100 điểm)
3. Phase Gate
4. Cá nhân hóa sau Diagnostic (skip / shorten / full)
5. Remediation plan
6. Interview prep cuối phase
7. Thang Level 1/2/3 dùng cho SKILL_MATRIX

---

## 1. Quy tắc bằng chứng

Điểm chỉ dựa trên thứ kiểm chứng được:
- Code chạy được và test xanh **khi chính bạn chạy** (`pytest -q`, script eval). Ghi lệnh + kết quả.
- Câu trả lời lý thuyết do người học tự viết (không phải dán từ nguồn khác; nghi ngờ thì hỏi lại bằng biến thể).
- Metric có số liệu, có cấu hình, tái lập được (seed, dữ liệu, phiên bản).
- Phần giải thích kiến trúc/giới hạn: người học tự nói/viết, bạn hỏi vặn ít nhất 2 câu.

Không có bằng chứng → ghi "chưa đánh giá", không ghi "đạt".

---

## 2. Rubric Module Gate (100 điểm)

| Hạng mục | Điểm | Đạt đầy đủ khi |
|---|---|---|
| Quiz bản chất | 25 | Trả lời đúng và giải thích được "tại sao", không chỉ kết quả |
| Coding task [TỰ VIẾT] | 25 | Đúng, xử lý edge cases, code đọc được, có type hints khi phù hợp |
| Debugging task | 15 | Giả thuyết → kiểm chứng → sửa → test chống tái phát |
| Test & evaluation | 15 | Test có ý nghĩa (không chỉ happy path), metric đúng và giải thích được |
| Integration | 10 | Tích hợp vào DriveStack Lab, regression tests cũ vẫn xanh |
| Giới hạn & failure modes | 10 | Nêu được khi nào phương pháp hỏng và hậu quả ở module sau |

**Đạt = ≥ 85 VÀ không trượt tiêu chí cốt lõi nào.** Mỗi module có 1–3 tiêu chí cốt lõi ghi trong `learning/MODULE_GATES.md` (ví dụ M1.3: "nghịch đảo đúng một transform 4×4 và giải thích quy ước `T_a_b`"). Trượt cốt lõi = trượt gate dù tổng điểm cao.

Thang con cho từng câu quiz: 0 = sai/không biết · 1 = đúng một phần hoặc đúng nhưng không giải thích được · 2 = đúng + giải thích bản chất. Quy đổi về 25 điểm.

---

## 3. Phase Gate

Thành phần (tất cả bắt buộc):
1. **Demo** chạy trước mặt bạn (bạn tự chạy lệnh trong README) — ghi rõ simulation/replay/real.
2. **Unit + integration + regression tests** xanh.
3. **Báo cáo metric** (`reports/phaseN.md`): metric, lý do chọn, cấu hình, kết quả, so baseline.
4. **Phân tích lỗi:** ít nhất 3 failure cases có hình/log, nguyên nhân, đề xuất cải tiến.
5. **Phỏng vấn kỹ thuật** (xem mục 6).

Đạt phase khi: mọi module gate của phase đã đạt, 5 thành phần trên đạt, phỏng vấn ≥ 85/100.
Không chuyển phase chỉ vì hết thời gian. Trượt → remediation plan, giới hạn ở phần trượt.

---

## 4. Cá nhân hóa sau Diagnostic

Tính điểm theo domain (bảng ánh xạ câu → domain/module trong `learning/DIAGNOSTIC_TEST.md`):

| Kết quả domain | Quyết định |
|---|---|
| ≥ 85% và mọi câu ★ của domain đạt (phần A: 2/2; phần B: ≥ 6/7 và không mắc lỗi cốt lõi của bài), kèm độ tự tin khớp | **Gate-only:** bỏ phần dạy nhưng vẫn thi Module Gate đầy đủ; diagnostic không tự xác nhận đã qua gate |
| 60–84% | **Rút gọn:** chỉ dạy phần hổng (theo câu sai), bỏ phần đã vững; vẫn qua gate đầy đủ |
| < 60% | **Đầy đủ:** dạy toàn bộ module |
| Đúng nhưng tự tin thấp (1/3) | Coi như chưa vững — kiểm tra lại bằng 1–2 câu biến thể trước khi quyết định |
| Sai nhưng tự tin cao (3/3) | **Misconception** — ưu tiên số 1 trong WEAKNESS_BACKLOG |

Sau khi chấm: tính lại giờ từng module (gồm giờ thi gate) + 15% dự phòng → chọn kịch bản lịch theo `ROADMAP.md` §0.1 (24 tuần chỉ khi rút được ~100 h; fast track 18 tuần chỉ khi rút được ~240 h; nếu không, lịch dài hơn — không cắt nền tảng để khớp lịch). Cập nhật `SKILL_MATRIX.md` (cột Diagnostic), `ROADMAP.md` (lịch cá nhân hóa + lịch sử điều chỉnh), `WEAKNESS_BACKLOG.md`, `LEARNING_LOG.md`. Giải thích từng quyết định rút gọn bằng câu hỏi/bài cụ thể làm bằng chứng.

---

## 5. Remediation plan

```markdown
### Remediation — <module/phase> — YYYY-MM-DD
- **Trượt ở:** <hạng mục / tiêu chí cốt lõi> — bằng chứng: ...
- **Nguyên nhân gốc:** <kiến thức nền thiếu nào? (link module/backlog ID)>
- **Kế hoạch:** 1) giải thích lại theo cách khác: ... 2) bài nhỏ: ... 3) kiểm tra lại: ...
- **Thời lượng dự kiến:** ... giờ
- **Điều kiện thi lại gate:** ...
```

---

## 6. Interview prep cuối phase

Mỗi phase thêm vào `learning/INTERVIEW_PREP.md`:
- 15 câu kỹ thuật (≥ 5 câu "tại sao/trade-off", ≥ 3 câu nối với phase trước).
- 5 tình huống debugging (cho triệu chứng/log, hỏi quy trình điều tra).
- 3 bài coding liên quan (mức phỏng vấn Fresher/Junior: NumPy vectorization, hình học, graph search, interval/IoU, KF nhỏ...). Không LeetCode nâng cao không liên quan.
- 1 mock interview: bạn đóng vai interviewer, hỏi từng câu, chờ trả lời, hỏi vặn, chấm theo thang 0/1/2 + nhận xét giao tiếp.
- 1 bài giải thích kiến trúc DriveStack Lab trong 3–5 phút (data flow, frames, timestamp, metric, failure mode, cải tiến tiếp theo).

Vị trí mục tiêu: AI/CV Engineer, ADAS Perception, 3D Perception, AD Software Engineer, Robotics Perception.

---

## 7. Thang Level dùng cho SKILL_MATRIX

- **L0** — Chưa có bằng chứng.
- **L1 — Hiểu & chạy được:** giải thích đúng khái niệm, chạy được tool/pipeline có sẵn, đọc hiểu output.
- **L2 — Tự cài đặt & debug:** tự cài bản tối giản, dùng thư viện đúng quy ước, viết test, debug lỗi thường gặp, đo metric.
- **L3 — Tích hợp & cải thiện:** tích hợp vào hệ thống với interface/frame/timestamp đúng, phân tích failure modes, đo latency, đề xuất và kiểm chứng cải tiến, giải thích trade-off khi phỏng vấn.

Mỗi lần nâng Level phải kèm bằng chứng (link file/test/log).
