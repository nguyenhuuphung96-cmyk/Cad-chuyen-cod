# Đọc DXF → G-code dập lưới

Trang web một file, chạy thẳng trong trình duyệt. Chọn bản vẽ AutoCAD `.dxf`, chọn lỗ cần dập, trang tự sinh mã G-code dập lưới và cho tải về file `.txt` (đổi sang `.tap`, `.nc`… được ở ô **Đuôi file** bước 3).

Bản vẽ **không được gửi đi đâu cả** — mọi tính toán chạy ngay trên máy hoặc điện thoại của bạn.

---

## Đưa lên GitHub (làm một lần, khoảng 5 phút)

1. Vào https://github.com → bấm **New** để tạo kho mới.
   - **Repository name**: ví dụ `dxf-gcode`
   - Chọn **Public** (bắt buộc, nếu để Private thì GitHub Pages không chạy với tài khoản miễn phí)
   - Bấm **Create repository**
2. Trong kho vừa tạo, bấm **Add file → Upload files**.
3. Kéo **toàn bộ các file trong thư mục này** vào (index.html, manifest.webmanifest, sw.js, các file .png, README.md, .nojekyll). Lưu ý: kéo từng file, **đừng kéo nguyên thư mục mẹ** vào, nếu không `index.html` sẽ nằm sâu một cấp.
4. Bấm **Commit changes**.
5. Vào tab **Settings** (của kho) → menu bên trái chọn **Pages**.
   - **Source**: Deploy from a branch
   - **Branch**: `main` và thư mục `/ (root)` → bấm **Save**
6. Chờ 1–2 phút, tải lại trang Pages, GitHub sẽ hiện đường dẫn dạng:

   ```
   https://<tên-tài-khoản>.github.io/dxf-gcode/
   ```

Đó là địa chỉ để mở trên điện thoại.

---

## Dùng trên điện thoại

Mở đường dẫn ở trên bằng trình duyệt điện thoại, rồi cài ra màn hình chính để lần sau mở nhanh và **dùng được cả khi không có mạng**:

- **Android (Chrome)**: bấm dấu **⋮** góc trên phải → *Thêm vào Màn hình chính* / *Cài đặt ứng dụng*.
- **iPhone (Safari)**: bấm nút **Chia sẻ** (ô vuông có mũi tên) → *Thêm vào MH chính*.

Cách lấy file mã ra khỏi điện thoại:

- **Tải file .tap** — file vào thư mục Tải về / Files.
- **Gửi / Chia sẻ** — đẩy thẳng sang Zalo, email, USB-OTG, máy tính…
- **Sao chép** — chép toàn bộ mã vào bộ nhớ tạm để dán sang nơi khác.

Trên iPhone, nếu máy không cho tải đuôi lạ, cứ tải về rồi đổi tên file sau, hoặc dùng nút **Gửi / Chia sẻ**.

---

## Tấm có nhiều ô lưới

Nếu một tấm chia thành nhiều ô (ví dụ 3 ô ngang × 2 ô dọc), app dập **trọn từng ô rồi mới sang ô kế tiếp**: hết lỗ trong ô thì ra `M109`, xong mới `G0` chạy sang ô mới — không bao giờ nhảy qua nhảy lại giữa hai ô.

- **Tự nhận theo khoảng hở**: chỗ nào hở hơn 2,5 lần bước lỗ thì coi là sang ô khác. Ô bị gộp → giảm số đó; ô bị cắt vụn → tăng lên.
- **Tự điền số ô ngang × dọc**: dùng khi các ô chia đều nhau, gõ thẳng 3 × 2.
- **Thứ tự chạy các ô**: mặc định là **ô nào gần nhất chạy trước**. Ô đầu tiên vẫn bắt đầu ở góc dưới trái
  (`G0 X0 Y0`, rồi Y đi lên); dập xong một ô thì nhảy tới ô có lỗ gần đầu dập nhất và vào ô đó từ phía gần nhất.
  Nếu muốn ô nào cũng bắt đầu từ góc dưới trái thì chọn kiểu cố định: dưới→trên hoặc trên→xuống, có zigzag hoặc không.
- **Quãng chạy** tổng cộng hiện ở bước 4 để so sánh giữa các kiểu.
- **Mã chèn giữa hai ô**: để trống nếu máy không cần; điền `M00` nếu muốn máy dừng cho thao tác giữa hai ô.

Khuôn chùm cũng chỉ xếp **trong từng ô**, không có chuyện một nhát dập vắt qua hai ô.

Ở bước 4, mỗi ô được khoanh khung nét đứt và đánh số “ô 1, ô 2…” đúng theo thứ tự máy sẽ chạy — nhìn hình đó là biết ngay có đúng ý không. Bấm nút **Mẫu nhiều ô** để xem thử.

## Chiều trục của máy

Bản vẽ CAD luôn có Y đi lên, nhưng nhiều máy dập tính **Y+ theo chiều đi xuống**. Ở bước 3 có hai ô chọn:

- **Trục Y của máy**: `Y+ đi lên` (giống bản vẽ) hoặc `Y+ đi xuống` — chọn cái sau nếu bạn dập từ trên xuống mà muốn số dương: `G0 X0 Y0` rồi `Y16`, `Y32`… thay vì `Y-16`, `Y-32`.
- **Trục X của máy**: `X+ sang phải` (giống bản vẽ) hoặc `X+ sang trái`.

Chọn thêm **Gốc toạ độ = Góc cụm lỗ** thì gốc tự rơi vào đúng góc máy bắt đầu, cả chương trình không bao giờ có số âm.

Lưu ý: ô **Cột đầu dập theo chiều** nói về chiều *trên bản vẽ* (đi lên hay đi xuống tờ giấy), không liên quan tới dấu của toạ độ — dấu do hai ô chọn trục ở trên quyết định.

## Chuẩn bị bản vẽ

- Trong AutoCAD dùng lệnh **SAVEAS**, chọn kiểu **AutoCAD DXF** (bản ASCII, **không** chọn Binary). File `.dwg` không đọc trực tiếp được.
- Lỗ tròn: vẽ bằng lệnh **CIRCLE**.
- Rãnh dài / chữ nhật: phải là đường **khép kín**. Nếu các cung và đường còn hở đầu nối thì dùng lệnh **JOIN** hoặc **PEDIT** trước khi lưu.
- Lỗ nằm trong block (**INSERT**) vẫn đọc được.
- Chưa có bản vẽ thì bấm **Mẫu lỗ tròn** hoặc **Mẫu rãnh dài** để thử trước.

## Ghi chú

- Các cài đặt ở bước 3 (mã M, thứ tự cột, tên file…) được nhớ lại trong trình duyệt, lần sau mở lên vẫn còn.
- Trên điện thoại, nút chọn file mở ra toàn bộ file để tránh bị ẩn mất file `.dxf` — chọn đúng file `.dxf` là được.

## Khi sửa lại trang

Sửa file rồi upload đè lên GitHub. Nếu điện thoại vẫn hiện bản cũ, mở file `sw.js` đổi dòng

```js
var CACHE = 'dxf-gcode-v1';
```

thành `v2`, `v3`… rồi upload lại. Máy sẽ tự lấy bản mới.
