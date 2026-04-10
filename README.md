## G: TRIỂN KHAI ỨNG DỤNG ĐẾN END - USER

---

### 1. TẠO TUNNEL TRONG CLOUDFARE

Bước 1: Đăng nhập Cloudflare trên Ubuntu.

- Chạy lệnh : `docker run -it --rm cloudflare/cloudflared tunnel login`

👉 Sau khi chạy:

Nó sẽ đưa 1 link

<img width="890" height="521" alt="{0119D89D-0CE1-42F5-AF13-CA887639A196}" src="https://github.com/user-attachments/assets/9b73c798-91af-421c-9ebe-f293f425988e" /></p>

Copy link → mở trên trình duyệt

<img width="1920" height="1023" alt="{AD1827A3-F637-4CE8-934A-4696B14CE465}" src="https://github.com/user-attachments/assets/4c51fe28-8157-4297-901f-084d3ba0f3d9" /></p>

Đăng nhập tài khoản Cloudflare -> Chọn domain muốn sử dụng -> Authorize

<img width="1167" height="633" alt="{F8AB89D0-2408-4129-8754-AA13645FF881}" src="https://github.com/user-attachments/assets/54ec4cd7-89b2-4055-bb82-ab889c33760b" /></p>

✅ Thành công → nó sẽ tạo file credentials

<img width="1920" height="1029" alt="{8314B799-8454-4C7A-B84B-B2E38BB96136}" src="https://github.com/user-attachments/assets/8da7324a-2d77-4bd0-b6f7-984211d4b6c3" /></p>

Bước 2: Đặt tên tunnel, chọn Cloudflared

- Nó sẽ tạo ra một domain quản lý là ***ducduong.cloudflareaccess.com***

<img width="1920" height="1022" alt="{D84D4CE8-6516-45A7-80AA-4ECEDC7FB62B}" src="https://github.com/user-attachments/assets/2d098538-5814-48cc-88e2-28b819d8b938" /></p>

Bước 3: Chọn gói dịch vụ (Plan): Cloudflare sẽ yêu cầu chọn gói. Hãy chọn gói Free ($0/month).

<img width="1920" height="1017" alt="{ECF6C082-A152-43AD-9F2F-BDE13C6005F1}" src="https://github.com/user-attachments/assets/545b5d9b-fd0f-4cf5-a723-bef758be35ea" /></p>

Bước 4: Xác nhận thanh toán (Add Payment Method)

<img width="1404" height="754" alt="{1A1F7B11-5920-42BE-8283-DE23D3194818}" src="https://github.com/user-attachments/assets/9754d47a-e5ba-4d7b-b6e8-cc0f731da53a" /></p>

Bước 5: Hoàn tất Onboarding




