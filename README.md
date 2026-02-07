# Todo New

Ứng dụng web MVP — Thiết lập bộ tiêu chuẩn vận hành (static HTML + Tailwind CDN).

## Cấu trúc các màn hình chính

- **`index.html`**: Danh mục các **bộ tiêu chuẩn** (trang chính, nên share link này).
  - Nút **“Tạo bộ tiêu chuẩn mới”** → `standard-builder.html`.
  - Click từng hàng → `standard-detail.html` (Checklist + Daily work của 1 bộ).
- **`standard-detail.html`**: Checklist + Daily work cho một bộ tiêu chuẩn.
  - Mũi tên quay lại → `index.html`.
  - Navbar trái: chuyển giữa `my-tasks.html`, `index.html`, `audit-list.html`.
- **`my-tasks.html`**: Màn “Công việc của tôi”.
- **`audit-list.html`**: Bảng chấm điểm tổng hợp, từ đây đi vào chi tiết.
- **`audit-detail.html`**: Chi tiết chấm điểm 1 tiêu chí (video, hình, text).
- **`standard-builder.html`**: Màn thiết lập chi tiết **1 bộ tiêu chuẩn vận hành**.

Các file dạng `* copy.html` hoặc `screen_*.html` chỉ là prototype, không bắt buộc dùng khi demo.

---

## Chạy localhost (tạm xem trên máy)

Dự án dùng file tĩnh (HTML/CSS/JS), không cần build. Chạy server đơn giản bằng Python:

```bash
# Port 8080 (mặc định)
python3 -m http.server 8080
```

Mở trình duyệt: **http://localhost:8080**

---

Dùng port khác (ví dụ 3000):

```bash
python3 -m http.server 3000
```

Mở trình duyệt: **http://localhost:3000**

---

Dừng server: nhấn `Ctrl + C` trong terminal.

### Lỗi "Address already in use"

Port đang bị chiếm (server cũ vẫn chạy). Có 2 cách:

1. **Đổi port** — chạy port khác, ví dụ:
   ```bash
   python3 -m http.server 3000
   ```
   Rồi mở http://localhost:3000

2. **Giải phóng port** — tìm và tắt process đang dùng port (ví dụ 8080):
   ```bash
   lsof -i :8080
   kill -9 <PID>
   ```
   Thay `8080` và `<PID>` bằng port và số PID hiển thị.

---

## Triển khai miễn phí lên Netlify

Ứng dụng là **static site** nên deploy rất đơn giản, chỉ cần các file `.html`.

- **Bước 1 – Chuẩn bị thư mục để upload**
  - Tạo 1 thư mục mới, ví dụ `deploy/`.
  - Copy vào `deploy/` các file:
    - `index.html`
    - `standard-detail.html`
    - `my-tasks.html`
    - `audit-list.html`
    - `audit-detail.html`
    - `standard-builder.html`
  - Không cần copy `node_modules/`, `dist/`, `tsconfig.tsbuildinfo`.

- **Bước 2 – Dùng Netlify Drop**
  - Truy cập `https://app.netlify.com/drop`.
  - Kéo thả cả thư mục `deploy/` (hoặc file `deploy.zip` nếu bạn nén lại).
  - Đợi build xong, Netlify sẽ trả về 1 URL dạng:  
    `https://<ten-site>.netlify.app`

- **Bước 3 – Link nên share cho mọi người**
  - Dùng link:  
    `https://<ten-site>.netlify.app/`
  - Từ đây người dùng đi theo flow:
    - → `index.html` → `standard-detail.html` → `my-tasks.html` / `audit-list.html` → `audit-detail.html`
    - Khi cần thiết lập bộ tiêu chuẩn: từ `index.html` bấm **“Tạo bộ tiêu chuẩn mới”** → `standard-builder.html`.

Bạn có thể vào phần **Site settings** của Netlify để đổi `<ten-site>` cho dễ nhớ (ví dụ: `retail-standards-demo.netlify.app`).

---

## Triển khai miễn phí lên Vercel

Dự án đã có sẵn file **`vercel.json`** để Vercel nhận diện đây là static site (không build). Deploy rất nhanh.

### Cách 1: Deploy qua GitLab (nên dùng)

1. **Đẩy code lên GitLab**
   - Tạo project mới trên GitLab (ví dụ: `todo-standards`).
   - Trong thư mục dự án, chạy:
     ```bash
     git init
     git add index.html create-page.html ui-mobile.html my-tasks.html audit-list.html audit-detail.html list-person.html standard-builder.html standard-detail.html shared-nav.html README.md vercel.json .gitignore
     git commit -m "Prepare for Vercel deploy"
     git branch -M main
     git remote add origin https://gitlab.com/<username>/<project>.git
     git push -u origin main
     ```
   - Thay `<username>` và `<project>` bằng tên nhóm/username và tên project trên GitLab của bạn.  
   - Nếu dùng SSH: `git remote add origin git@gitlab.com:<username>/<project>.git`

2. **Kết nối Vercel với GitLab**
   - Vào [vercel.com](https://vercel.com), đăng nhập (có thể dùng tài khoản GitLab).
   - Bấm **Add New…** → **Project**.
   - Chọn **Import Git Repository** → nếu chưa có GitLab, bấm **Configure Git Integrations** và kết nối GitLab, sau đó chọn repo vừa đẩy.
   - **Framework Preset**: để **Other** (hoặc Vercel tự nhận).
   - **Root Directory**: để trống.
   - Bấm **Deploy**. Vercel sẽ không chạy build (đã cấu hình trong `vercel.json`).

3. **Link sau khi deploy**
   - Vercel cho URL dạng: `https://<ten-project>.vercel.app`
   - Trang chủ: `https://<ten-project>.vercel.app/` hoặc `https://<ten-project>.vercel.app/index.html`

### Cách 2: Deploy bằng Vercel CLI

1. Cài Vercel CLI (chỉ cần 1 lần):
   ```bash
   npm i -g vercel
   ```

2. Trong thư mục dự án (chứa `vercel.json` và `index.html`):
   ```bash
   vercel
   ```

3. Làm theo hướng dẫn: đăng nhập nếu cần, chọn/ tạo project. Sau khi xong bạn sẽ có link dạng `https://...vercel.app`.

**Lưu ý:** Khi deploy qua Git, mỗi lần bạn `git push` lên branch đã kết nối, Vercel sẽ tự deploy lại (miễn phí). Không cần đẩy `node_modules/` hay `dist/` — đã có trong `.gitignore`.
