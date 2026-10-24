# Đặc tả thiết kế — Trang công cụ BMI (TEC-73)

> Nguồn: task Linear TEC-73, project `m-software-bmi-thuc-don`.
> Repo hiện chưa có trang mẫu (chỉ có README), nên bản này mô tả trang BMI từ đầu,
> đủ chi tiết để Dev hiện thực một trang duy nhất, mobile-first.

## 1. Nguyên tắc tổng thể

- **Mobile-first:** thiết kế cho màn hình điện thoại trước (chiều rộng 360–430px),
  sau đó nới rộng dần lên tablet/desktop bằng cách tăng bề rộng tối đa, KHÔNG đổi
  thứ tự khối.
- **Một trang, cuộn dọc:** toàn bộ luồng nằm trên một trang, không chuyển trang.
- **Ngôn ngữ:** tiếng Việt, câu ngắn, dễ hiểu, không thuật ngữ y khoa khó.
- **Tông:** sạch, nhẹ, thân thiện — giống app sức khỏe, không giống bảng số liệu y tế.

## 2. Bảng màu và kiểu chữ (đề xuất)

| Vai trò | Màu | Ghi chú |
|---|---|---|
| Màu chính (nút, tiêu đề nhấn) | Xanh lá đậm `#1B7A4D` | Cảm giác khỏe mạnh, tươi |
| Màu phụ (điểm nhấn nhẹ) | Xanh lá nhạt `#E7F4EC` | Nền thẻ kết quả, thẻ thực đơn |
| Nền trang | Trắng `#FFFFFF` | |
| Chữ chính | Xám đậm `#1F2937` | |
| Chữ phụ, ghi chú | Xám vừa `#6B7280` | |
| Cảnh báo nhẹ (thiếu cân / thừa cân) | Cam `#D97706` | Chỉ dùng cho nhãn phân loại, không tô nền đỏ gắt |
| Lỗi nhập liệu | Đỏ `#DC2626` | Chỉ cho dòng báo lỗi dưới ô nhập |

- **Kiểu chữ:** font hệ thống, không cần tải font ngoài
  (ví dụ: `-apple-system, "Segoe UI", Roboto, sans-serif`).
- **Cỡ chữ mobile:** tiêu đề trang 22px đậm; tiêu đề khối 18px đậm;
  chữ thường 16px; ghi chú 14px. Chữ không nhỏ hơn 14px để đọc được trên điện thoại.

## 3. Bố cục tổng thể (thứ tự hiển thị từ trên xuống)

Trang gồm đúng 5 khối, theo thứ tự sau — Dev giữ nguyên thứ tự này ở mọi cỡ màn hình:

1. **Đầu trang (Header)** — tiêu đề + mô tả ngắn
2. **Khối A — Form nhập** (luôn hiển thị đầu tiên)
3. **Khối B — Kết quả** (ẩn cho đến khi bấm "Tính BMI"; sau khi tính thì hiện
   ngay dưới form và tự cuộn tới)
4. **Khối C — Thực đơn mẫu 1 ngày** (nằm trong vùng kết quả, hiện cùng lúc với khối B)
5. **Khối D — Giải thích BMI + lưu ý tham khảo** (luôn hiển thị ở cuối trang,
   kể cả khi chưa tính)

Chiều rộng nội dung tối đa: 480px trên mobile (padding hai bên 16px);
trên màn hình ≥ 768px thì căn giữa, tối đa 640px.

## 4. Khối 1 — Đầu trang

- Tiêu đề: **"Tính chỉ số BMI"** (22px, đậm, màu chính).
- Dòng mô tả ngay dưới, 1–2 câu, ví dụ:
  "Nhập chiều cao và cân nặng để biết chỉ số BMI, phân loại thể trạng
  và gợi ý thực đơn cho một ngày."
- Không dùng ảnh minh họa lớn ở đầu trang (tránh đẩy form xuống dưới màn hình đầu).

## 5. Khối A — Form nhập (quan trọng nhất)

### 5.1. Các trường nhập — thứ tự cố định

| # | Trường | Kiểu nhập | Ghi chú hiển thị |
|---|---|---|---|
| 1 | Chiều cao | Ô số, đơn vị `cm` | Placeholder: `VD: 165`. Giới hạn hợp lệ: 100–220 cm |
| 2 | Cân nặng | Ô số, đơn vị `kg` | Placeholder: `VD: 60`. Giới hạn hợp lệ: 20–200 kg |
| 3 | Tuổi | Ô số, đơn vị `tuổi` | Placeholder: `VD: 28`. Giới hạn hợp lệ: 10–100 tuổi |
| 4 | Giới tính | 2 nút chọn: `Nam` / `Nữ` | Nút được chọn tô màu chính, nút còn lại viền xám; bắt buộc chọn |

- **Bố cục mobile:** mỗi trường chiếm trọn chiều rộng, xếp dọc (không chia 2 cột),
  để ngón tay bấm thoải mái. Nhãn trường đặt TRÊN ô nhập, không đặt bên trong ô.
- **Bàn phím:** ô số mở bàn phím số trên điện thoại (`inputmode="decimal"` cho
  chiều cao/cân nặng, `inputmode="numeric"` cho tuổi).
- **Đơn vị:** hiển thị đơn vị (cm / kg / tuổi) ở mép phải bên trong ô nhập, màu xám,
  không cho người dùng gõ đơn vị vào.

### 5.2. Nút tính

- Một nút duy nhất, trọn chiều rộng, cao tối thiểu 48px (dễ bấm bằng ngón tay),
  nền màu chính, chữ trắng đậm: **"Tính BMI"**.
- Trạng thái chưa đủ dữ liệu: nút vẫn hiển thị nhưng mờ (opacity 0,5) và không bấm được.
- Khi bấm mà có trường sai: KHÔNG tính, hiển thị dòng lỗi đỏ ngay dưới trường đó
  (ví dụ: "Chiều cao phải từ 100 đến 220 cm") và cuộn tới trường lỗi đầu tiên.

### 5.3. Hành vi sau khi tính

- Vùng kết quả (khối B + C) hiện ra ngay dưới form.
- Trang tự cuộn mềm tới đầu vùng kết quả (để người dùng thấy ngay, không phải tự tìm).
- Nút "Tính BMI" đổi thành **"Tính lại"**; bấm lại thì tính lại với số mới và cập nhật
  kết quả tại chỗ, không tải lại trang.
- Nút phụ "Nhập lại" (dạng chữ, không nền, đặt dưới nút chính) để xóa toàn bộ
  form và ẩn vùng kết quả.

## 6. Khối B — Vùng hiển thị kết quả

Hiện sau khi tính thành công, gồm các phần theo thứ tự:

### 6.1. Số BMI lớn

- Một thẻ nền xanh lá nhạt, bo góc 16px, padding 20px, căn giữa:
  - Dòng nhỏ trên: "Chỉ số BMI của bạn"
  - Số BMI lớn, đậm, cỡ 48px, hiển thị 1 chữ số thập phân (ví dụ: **22,4**).
  - Dòng dưới số: phân loại theo WHO, dạng nhãn (ví dụ: "Cân nặng bình thường"),
    màu chữ theo phân loại (bình thường = xanh lá đậm; thiếu cân/thừa cân = cam;
    béo phì = đỏ).

### 6.2. Thang phân loại WHO (dạng thanh trực quan)

- Một thanh ngang chia 4 đoạn màu, theo ngưỡng WHO:
  - `< 18,5` — Thiếu cân (cam nhạt)
  - `18,5 – 24,9` — Bình thường (xanh lá)
  - `25 – 29,9` — Thừa cân (cam)
  - `≥ 30` — Béo phì (đỏ)
- Một dấu mốc (chấm tròn + đường kẻ) đặt tại vị trí BMI của người dùng trên thang,
  để họ thấy mình đang ở đoạn nào. Dưới thang ghi tên 4 mức tương ứng.
- Trên mobile, thanh này tràn đủ chiều rộng thẻ, không cuộn ngang.

### 6.3. Calo gợi ý

- Một dòng/thẻ nhỏ ngay dưới thang phân loại:
  - "Nhu cầu calo gợi ý mỗi ngày: **~1.900 kcal**" (con số do Dev tính theo công thức
    đã thống nhất với BA; bản thiết kế chỉ quy định vị trí và cách hiển thị).
  - Dòng ghi chú nhỏ bên dưới: "Con số mang tính tham khảo, chưa tính mức vận động
    chi tiết."

## 7. Khối C — Thực đơn mẫu 1 ngày

- Tiêu đề khối: **"Thực đơn mẫu cho một ngày"** + dòng phụ:
  "Gợi ý theo mức calo trên — bạn có thể thay món tương đương."
- Hiển thị dạng danh sách dọc 4 bữa, mỗi bữa một thẻ riêng (bo góc 12px, viền xám nhạt):
  1. **Sáng** — tên món + ước lượng kcal (ví dụ: "Phở gà — ~450 kcal")
  2. **Trưa** — tên món + kcal
  3. **Chiều (ăn nhẹ)** — tên món + kcal
  4. **Tối** — tên món + kcal
- Mỗi thẻ bữa: tên bữa đậm ở trái, tổng kcal của bữa ở phải, danh sách món bên dưới
  dạng gạch đầu dòng ngắn (mỗi món 1 dòng, không mô tả dài).
- Cuối khối: dòng tổng "Tổng cả ngày: ~X kcal" (khớp với calo gợi ý ở khối B,
  chênh lệch cho phép do làm tròn món).
- **Quy tắc hiển thị:** thực đơn thay đổi theo phân loại BMI (thiếu cân / bình thường /
  thừa cân / béo phì) — Dev lấy nội dung món từ dữ liệu đã thống nhất với BA;
  bản thiết kế chỉ quy định khung hiển thị, không chốt tên món cụ thể.

## 8. Khối D — Giải thích BMI và lưu ý tham khảo

Luôn hiển thị ở cuối trang (kể cả trước khi tính), gồm:

- **"BMI là gì?"** — 2–3 câu ngắn: BMI là chỉ số tính từ chiều cao và cân nặng,
  dùng để đánh giá nhanh thể trạng, không đo trực tiếp lượng mỡ cơ thể.
- **Bảng ngưỡng WHO** — bảng nhỏ 4 dòng (mức + khoảng BMI), khớp với thang ở 6.2.
- **Lưu ý tham khảo** (dạng danh sách, có biểu tượng ⚠️ ở đầu khối):
  - Kết quả chỉ mang tính tham khảo, không thay cho tư vấn y tế.
  - BMI chưa phản ánh cơ bắp, tuổi tác, tình trạng mang thai hay bệnh lý —
    người tập gym, phụ nữ mang thai, trẻ em nên hỏi bác sĩ/chuyên gia dinh dưỡng.
  - Nếu BMI ngoài khoảng bình thường và kéo dài, nên đi khám để được tư vấn.
- Khối này dùng chữ nhỏ hơn một chút (14–15px), nền xám rất nhạt hoặc không nền,
  để phân biệt với vùng kết quả chính.

## 9. Hành vi responsive (từ mobile lên desktop)

| Cỡ màn hình | Thay đổi duy nhất |
|---|---|
| Mobile (< 768px) | Mọi khối xếp dọc, form 1 cột, nút trọn chiều rộng (mô tả ở trên) |
| Tablet/desktop (≥ 768px) | Nội dung căn giữa, tối đa 640px; form vẫn 1 cột (không chia 2 cột) để giữ luồng nhập đơn giản; các thẻ kết quả giữ nguyên bố cục, chỉ rộng hơn |

- Không dùng menu, không dùng tab, không dùng popup cho luồng chính.
- Mọi nút bấm cao tối thiểu 48px; khoảng cách giữa các khối 24px; giữa các trường
  trong form 16px.

## 10. Trạng thái và trường hợp biên

- **Chưa nhập đủ:** nút "Tính BMI" mờ, không bấm được.
- **Nhập sai:** báo lỗi đỏ dưới trường sai, không tính, cuộn tới lỗi đầu tiên.
- **Kết quả cực biên:** BMI rất thấp (< 14) hoặc rất cao (> 40) vẫn hiển thị bình thường
  trên thang, kèm dòng lưu ý ở khối D (không chặn, không báo lỗi).
- **Mất kết quả khi tải lại trang:** chấp nhận được ở v1 — không yêu cầu lưu lịch sử
  hay lưu vào bộ nhớ trình duyệt.

## 11. Ghi chú cho Dev

- Đây là bản thiết kế giao diện (TEC-73). Công thức tính BMI/calo và nội dung món ăn
  cụ thể do BA chốt trong task dữ liệu tương ứng; Dev chỉ cần hiện thực khung,
  thứ tự khối và hành vi mô tả ở trên, cắm công thức/dữ liệu khi có.
- Nếu khi hiện thực phát hiện một khối không vừa màn hình 360px, ưu tiên thu gọn
  padding/chữ trong khối đó, không đổi thứ tự khối.
