# Port Report Manager (Báo cáo sản lượng cảng)

Ứng dụng desktop nhập liệu & quản lý báo cáo sản lượng cảng theo tháng. Dữ liệu được đọc từ file Excel và **chỉ ghi ngược lại Excel khi bấm “Save All”**.

## 1) Hướng dẫn cho người dùng (không IT)

### 1.1 Chuẩn bị trước khi dùng

1. Chuẩn bị 1 thư mục làm việc (ví dụ `PortReportManager/`).
2. Đặt các file sau vào cùng thư mục:
   - File chạy ứng dụng (ví dụ `uyen-bcsl2.exe` trên Windows, hoặc `uyen-bcsl2` trên Linux/macOS).
   - `data.xlsx` (file dữ liệu).
   - (Tuỳ chọn) `config.toml` để đổi đường dẫn Excel/log.
3. Khuyến nghị: sao lưu `data.xlsx` trước khi nhập liệu.

### 1.2 Mở ứng dụng

- Mở file chạy ứng dụng.
- Nếu Excel lỗi cấu trúc/sai đường dẫn, ứng dụng sẽ không vào được màn hình chính.

### 1.3 Nguyên tắc “Save”

- Mọi thao tác thêm/sửa/xoá **chưa** ghi vào Excel cho tới khi bấm `Save All`.
- Khi có thay đổi, thanh trên cùng sẽ hiện trạng thái “Unsaved changes”.
- Mỗi tab có nút `↩ Discard All`: bỏ toàn bộ thay đổi chưa lưu của tab đó và tải lại từ Excel.

### 1.4 Các tab và cách dùng

#### A. Tab `Port Reports` (nhập liệu chính)

Bạn sẽ nhập các cột:
- `Port` (cảng) – chọn từ danh sách
- `Reporter` (đơn vị báo cáo) – chọn từ danh sách
- `Report Month` – nhập theo `yyyy-mm` (ví dụ `2026-03`)
- `Value` – số TEU (chỉ nhập số; có thể dùng dấu phẩy ngăn cách như `12,345`)

Ghi chú:
- `Group` và `Parent Group` là **tự động** (không sửa trực tiếp). Khi đổi `Port` hoặc `Report Month` thì nhóm sẽ cập nhật theo dữ liệu quan hệ có hiệu lực tại thời điểm tháng đó.
- Không cho phép `Report Month` ở tương lai.
- Không cho phép trùng bộ khoá `(Port, Reporter, Report Month)`.

Các nút trên thanh công cụ:
- `+ Add Row`: thêm 1 dòng trống (mặc định tháng = tháng trước).
- `++ Add Multiples`: thêm nhiều dòng cho **tháng trước** (ưu tiên reporter tên `VPA`, nếu không có sẽ lấy reporter đầu tiên).
  - Các dòng thêm nhanh có thể có `Value = 0`; các dòng `Value <= 0` sẽ **không được ghi** xuống Excel khi bấm `Save All` (bạn cần nhập giá trị > 0 nếu muốn lưu).
- `↩ Discard All`: bỏ thay đổi chưa lưu của tab.
- `Search`: lọc theo chữ (nhấn Enter ở ô Search để chạy lọc).

Bộ lọc:
- Parent Group → Group → Port (lọc dạng “chọn dần”).
- From/To (nhập `yyyy-mm`, nhấn Enter để áp dụng).

#### B. Tab `Region Reports`

- Quản lý báo cáo theo Region (Group) theo tháng.
- `Region` trên toolbar dùng để lọc nhanh; bấm `Reset` để bỏ lọc.
- `Month` nhập theo `yyyy-mm`.
- `Plan TEU`, `Real TEU` có thể để trống; nếu nhập thì phải là số.
- `++ Add Multiples`: tạo nhanh dữ liệu cho tháng trước (một số region đang active).

#### C. Tab `Groups`

- Quản lý danh sách Group.
- Cột `Region` là tuỳ chọn (chọn 1 group làm region cha).
- `Created At`/`Expired At` chấp nhận định dạng `yyyy-mm-dd` hoặc `yyyy-mm` (để trống `Expired At` nếu chưa hết hiệu lực).

#### D. Tab `Ports`

- Quản lý danh sách Port.
- Cột `Group` là tuỳ chọn (gán port vào group).
- Cột `Province` là tuỳ chọn.
- `Created At`/`Expired At` chấp nhận định dạng `yyyy-mm-dd` hoặc `yyyy-mm`.

### 1.5 Xoá dữ liệu (Delete)

- Chọn 1 dòng trong bảng và bấm `Delete`.
- Dòng sẽ được “đánh dấu xoá” (chưa xoá thật trong Excel) cho tới khi bấm `Save All`.

### 1.6 Phím tắt (tổng hợp từ source code)

**Áp dụng trong tab `Port Reports`:**
- `Ctrl+F`: đưa con trỏ vào ô `Search`.
- `Ctrl+N`: thêm 1 dòng mới.

**Trong bảng `Port Reports` (bảng lớn ở giữa):**
- `Enter`: sửa ô đang chọn (ô chọn bằng mũi tên `Left/Right` hoặc double click).
- `Double click`: sửa đúng ô đang bấm.
- `Tab` / `Shift+Tab`: lưu ô hiện tại và chuyển sang ô kế/trước (chỉ các cột cho phép sửa).
- `Esc`: huỷ sửa ô đang nhập.
- `Delete`: đánh dấu xoá dòng đang chọn.
- `Ctrl+C`: copy dòng đang chọn (lưu vào bộ nhớ tạm của app).
- `Ctrl+V`: paste (tạo 1 dòng mới giống dòng đã copy và chèn xuống ngay dưới dòng đang chọn).
- `Ctrl+D`: Fill down (lấy `Value` của dòng phía trên xuống dòng hiện tại).
- `Ctrl+Shift+D`: Duplicate (nhân đôi dòng đang chọn và chèn xuống ngay dưới).
- Khi đang sửa ô số `Value`: dùng `Up/Down` để lưu ô và nhảy lên/xuống dòng kế tiếp cùng cột.

**Trong các bảng CRUD (`Groups`, `Ports`, `Region Reports`):**
- `Enter`: sửa ô đang chọn (cột chọn bằng `Left/Right`).
- `Double click`: sửa đúng ô đang bấm.
- `Tab` / `Shift+Tab`: lưu và chuyển ô kế/trước.
- `Esc`: huỷ sửa ô.
- `Delete`: đánh dấu xoá dòng.
- Khi đang sửa ô số (`Plan TEU`, `Real TEU`…): `Up/Down` để lưu ô và nhảy dòng kế tiếp cùng cột.

## 2) Hướng dẫn cho IT (test / run / build)

### 2.1 Yêu cầu

- Python `>= 3.13`
- Tkinter (Windows thường có sẵn; Linux có thể cần cài gói `python3-tk`)
- Khuyến nghị dùng `uv` để cài dependency và chạy lệnh (repo đã có `uv.lock`)

### 2.2 Cấu hình runtime

File `config.toml` (đặt cùng thư mục chạy app):

```toml
excel_path = "./data.xlsx"
log_path = "./logs/app.log"
```

Nếu không có `config.toml` thì app dùng mặc định như trên.

### 2.3 Cài dependency (dev)

```bash
uv sync --dev
```

### 2.4 Chạy app ở chế độ dev

```bash
uv run python main.py
```

### 2.5 Chạy test

```bash
uv run pytest tests/ -v
```

### 2.6 Build app (đóng gói)

Repo hiện chưa “pin” tool build; cách phổ biến là dùng PyInstaller.

1) Cài PyInstaller (dev):

```bash
uv add --dev pyinstaller
```

2) Build 1-file executable:

```bash
uv run pyinstaller --onefile --noconsole --name uyen-bcsl2 main.py
```

3) Lấy file ở `dist/` và tạo thư mục chạy dạng:
- `uyen-bcsl2(.exe)`
- `data.xlsx`
- `config.toml` (tuỳ chọn)
- thư mục `logs/` (app sẽ tự tạo nếu chưa có)

## 3) Log

Log mặc định ghi tại `./logs/app.log` (hoặc theo `log_path` trong `config.toml`).

## 4) Troubleshooting

- Lỗi `ModuleNotFoundError: tkinter`: cài Tkinter (ví dụ Ubuntu/Debian: `sudo apt install python3-tk`).
- Không thấy dữ liệu: kiểm tra `excel_path` và tên sheet trong `data.xlsx` (phải có: `groups`, `ports`, `reporters`, `relationship`, `port_reports`, `region_reports`, `provinces`).

**Linux: Lỗi `cannot open display` hoặc không hiển thị cửa sổ**
- Đảm bảo đang chạy trong môi trường desktop (X11/Wayland), không phải SSH thuần.
- Nếu qua SSH: dùng `ssh -X` hoặc `-Y`.

**Linux: Lỗi thiếu thư viện `libgtk-3`**
- Chạy lại lệnh cài thư viện hệ thống ở trên.

**Windows: Lỗi `LINK : fatal error` khi build**
- Cài **Visual Studio C++ Build Tools** và chọn workload "Desktop development with C++".

**File Excel không tải được**
- Kiểm tra `excel_path` trong `config.toml` trỏ đúng đến file.
- Đảm bảo file không đang được mở bởi Excel.
