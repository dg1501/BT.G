## G: TRIỂN KHAI ỨNG DỤNG ĐẾN END - USER

---

### 1. TẠO TUNNEL TRONG CLOUDFARE

Bước 1: Đăng nhập Cloudflare trên Ubuntu.

- Chạy lệnh : `docker run -it --rm cloudflare/cloudflared tunnel login`

👉 Sau khi chạy:

Nó sẽ đưa 1 link

Copy link → mở trên trình duyệt

Đăng nhập tài khoản Cloudflare

Chọn domain

✅ Thành công → nó sẽ tạo file credentials

Bước 2: Đặt tên tunnel, chọn Cloudflared

- Nó sẽ tạo ra một domain quản lý là ***ducduong.cloudflareaccess.com***

<img width="1920" height="1022" alt="{D84D4CE8-6516-45A7-80AA-4ECEDC7FB62B}" src="https://github.com/user-attachments/assets/2d098538-5814-48cc-88e2-28b819d8b938" /></p>

Bước 3: Chọn gói dịch vụ (Plan): Cloudflare sẽ yêu cầu chọn gói. Hãy chọn gói Free ($0/month).

<img width="1920" height="1017" alt="{ECF6C082-A152-43AD-9F2F-BDE13C6005F1}" src="https://github.com/user-attachments/assets/545b5d9b-fd0f-4cf5-a723-bef758be35ea" /></p>

Bước 4: Xác nhận thanh toán (Add Payment Method)

<img width="1404" height="754" alt="{1A1F7B11-5920-42BE-8283-DE23D3194818}" src="https://github.com/user-attachments/assets/9754d47a-e5ba-4d7b-b6e8-cc0f731da53a" /></p>

Bước 5: Hoàn tất Onboarding




