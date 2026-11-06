# Session template & nhịp đánh giá

## Mục lục
1. Format buổi học A–J
2. Daily Check
3. Weekly Review
4. Template checkpoint cuối phiên
5. Template ghi LEARNING_LOG

---

## 1. Format buổi học A–J

Viết gọn; mỗi phần chỉ dài khi nội dung cần. Phần D–G là bài tập — giao xong thì dừng chờ, không giảng tiếp.

**A. Learning Context**
- Hôm nay học gì (module ID, ví dụ `M1.3 — Homogeneous transforms`).
- Vì sao cần; nằm ở đâu trong pipeline ADAS.
- Kiến thức cũ nào được dùng lại (dẫn module ID).
- Ai dùng output của bài này, sai thì hỏng gì phía sau.

**B. Concept** — trực giác trước, kỹ thuật sau; ít nhất một ví dụ thực tế trên xe tự lái.

**C. Mathematics** — công thức, ý nghĩa từng biến, đơn vị, giả định, một ví dụ số làm tay được.

**D. Code From Scratch** [TỰ VIẾT]
- Đề bài + signature + docstring + test (bạn viết test, người học viết thân hàm).
- Nêu rõ input/output shape, dtype, đơn vị, frame.
- Không đưa lời giải. Ghi sẵn "Hint có sẵn: H1/H2/H3 — hỏi khi cần".

**E. Engineering Implementation** [THƯ VIỆN]
- Làm lại bằng thư viện (NumPy/OpenCV/Open3D/PyTorch/ROS 2...).
- Câu hỏi so sánh: thư viện xử lý thêm gì (edge cases, hiệu năng, quy ước) mà bản tự viết chưa có?

**F. Debugging** [TỰ DEBUG]
- Ít nhất một lỗi cố ý (sai frame, sai đơn vị, sai thứ tự nhân ma trận, off-by-one, timestamp, dtype, shape broadcast...).
- Cho triệu chứng hoặc log, không nói lỗi ở đâu. Yêu cầu: giả thuyết → cách kiểm chứng → sửa → test chống tái phát.

**G. Evaluation** — test case + metric phù hợp, giải thích vì sao chọn metric đó.

**H. Integration** — nối vào DriveStack Lab: file nào, interface nào, schema nào; regression test nào phải vẫn xanh.

**I. Knowledge Check** — 5–10 câu, từ cơ bản → tình huống thực tế. Ít nhất một câu "điều gì xảy ra nếu…", một câu về failure mode.

**J. Learning Record** — checkpoint (mục 4) + cập nhật file.

Ghi chú khi mở buổi học: nếu `WEAKNESS_BACKLOG.md` có mục đến hạn ôn, mở đầu bằng 2–3 câu active recall cho mục đó (interleaving) trước phần A.

---

## 2. Daily Check (đầu hoặc cuối mỗi ngày học, ~20–30 phút)

- 5 câu hỏi ngắn: 3 câu bài hôm trước, 2 câu từ backlog/bài cũ (spaced repetition).
- 1 bài code nhỏ hoặc bài tính tay.
- 1 câu ứng dụng thực tế ("trên xe thật, nếu… thì…").
- Chấm nhanh: đúng / đúng một phần / sai; câu sai → backlog nếu là lỗi bản chất (không phải lỗi bất cẩn).

---

## 3. Weekly Review (cuối tuần, ~2–3 giờ)

1. Tổng kết: module đã qua, gate đã đạt, bằng chứng.
2. Ôn cách quãng: các mục backlog đến hạn.
3. Một bài tổng hợp nối kiến thức cũ và mới (ví dụ: transform + projection + đo latency).
4. Tiến độ thực vs kế hoạch (giờ học thực tế, module trễ, lý do).
5. Điều chỉnh lịch tuần sau trong `ROADMAP.md` nếu cần — trễ thì cắt Optional/Learn Later trước, không cắt Must Learn Now.

---

## 4. Template checkpoint cuối phiên

```markdown
### Checkpoint — YYYY-MM-DD — <module ID>
- **Đã học:** ...
- **Đã làm được:** ...
- **Bằng chứng:** <lệnh đã chạy> → <kết quả>; file: <đường dẫn>
- **Điểm (nếu có chấm):** xx/100 — tiêu chí cốt lõi: đạt/trượt (<tiêu chí>)
- **Chưa chắc:** ...
- **Lời giải AI đã dùng:** không / có (<phần nào>, đã giao biến thể: ...)
- **Module đang học:** ...
- **Việc tiếp theo:** ...
- **File đã cập nhật:** ...
```

---

## 5. Template ghi LEARNING_LOG

Thêm checkpoint mới lên **đầu** mục "Nhật ký" (mới nhất trên cùng) để phiên sau đọc nhanh. Giữ bảng "Trạng thái hiện tại" ở đầu file luôn đúng: module đang học, bài đang chờ nộp, gate gần nhất, ngày cập nhật.
