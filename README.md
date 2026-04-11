## G: TRIỂN KHAI ỨNG DỤNG ĐẾN END - USER

---

### 1. TẠO TUNNEL TRONG CLOUDFARE

Bước 1: Tạo thư mục 

`mkdir -p /home/duong/.cloudflared`

Bước 2: Set quyền 

`sudo chown -R 1000:1000 /home/duong/.cloudflared`

`sudo chmod -R 777 /home/duong/.cloudflared`

Bước 3: Đăng nhập Cloudflare trên Ubuntu.

- Chạy lệnh : `sudo docker run -it --rm -v /home/duong/.cloudflared:/home/nonroot/.cloudflared cloudflare/cloudflared tunnel login`

👉 Sau khi chạy:

- Nó sẽ đưa 1 link

<img width="890" height="521" alt="{0119D89D-0CE1-42F5-AF13-CA887639A196}" src="https://github.com/user-attachments/assets/9b73c798-91af-421c-9ebe-f293f425988e" /></p>

- Copy link → mở trên trình duyệt

<img width="1920" height="1023" alt="{AD1827A3-F637-4CE8-934A-4696B14CE465}" src="https://github.com/user-attachments/assets/4c51fe28-8157-4297-901f-084d3ba0f3d9" /></p>

- Đăng nhập tài khoản Cloudflare -> Chọn domain muốn sử dụng -> Authorize

<img width="1167" height="633" alt="{F8AB89D0-2408-4129-8754-AA13645FF881}" src="https://github.com/user-attachments/assets/54ec4cd7-89b2-4055-bb82-ab889c33760b" /></p>

✅ Thành công → Cloudflare sẽ trả về một file có tên là **cert.pem.** Đây là "chìa khóa" để máy của có quyền tạo tunnel trên tên miền.

<img width="1920" height="1029" alt="{8314B799-8454-4C7A-B84B-B2E38BB96136}" src="https://github.com/user-attachments/assets/8da7324a-2d77-4bd0-b6f7-984211d4b6c3" /></p>

<img width="1138" height="692" alt="{0B7D96BF-A9F0-4F5D-9E85-99FA59746EB6}" src="https://github.com/user-attachments/assets/0a15a060-c990-4016-9ab3-b1f62afadf04" /></p>

Bước 4: Tạo Tunnel

- Chạy lệnh: `docker run -it --rm \
-v ~/.cloudflared:/home/nonroot/.cloudflared \
cloudflare/cloudflared tunnel create mytunnel`

- Lệnh này sẽ tạo ra một file định dạng .json (chứa mã ID của tunnel).

👉 Kết quả:

- Nó tạo file dạng: `~/.cloudflared/xxxxx.json`

- - Sau khi chạy, nó sẽ hiện một dòng chữ có ID dạng: Created tunnel mytunnel with id 12345678-abcd-....

<img width="1137" height="687" alt="{D99CA8C1-F22B-4D35-A4FC-4A54899817EA}" src="https://github.com/user-attachments/assets/2ad520ef-c671-4e1d-9605-04a30dc09b3a" /></p>

Bước 5: Tạo File Config

- Chạy lệnh: `nano ~/.cloudflared/config.yml`

- Dán nội dung sau vào

`
tunnel: mytunnel
credentials-file: /home/duong/.cloudflared/b2bbf619-ed4e-483a-90bf-19e20f288d57.json

ingress:
  - hostname: app.ducduong.id.vn
    service: http://localhost:80
  - service: http_status:404
`

<img width="1386" height="902" alt="{0AF8D775-284D-41E4-BC88-143CE11B6F55}" src="https://github.com/user-attachments/assets/9d10618a-228a-4ff0-854c-bbaf252936c5" /></p>

Bước 6: Gắn Domain với Tunnel

- Chạy lệnh: `cloudflared tunnel route dns mytunnel app.ducduong.id.vn`

<img width="1404" height="754" alt="{1A1F7B11-5920-42BE-8283-DE23D3194818}" src="https://github.com/user-attachments/assets/9754d47a-e5ba-4d7b-b6e8-cc0f731da53a" /></p>

Bước 5: Hoàn tất Onboarding




