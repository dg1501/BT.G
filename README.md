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

Bước 6: Cài cloudflared

`sudo apt update`

`sudo apt install cloudflared -y`

` cloudflared --version`

<img width="1370" height="301" alt="image" src="https://github.com/user-attachments/assets/93aab7e9-c332-478d-ab03-6880e59c39be" /></p>

Bước 7: Gắn Domain với Tunnel

- Chạy lệnh: `cloudflared tunnel route dns mytunnel app.ducduong.id.vn`
  
<img width="1368" height="321" alt="{D1F90F68-503F-480D-9DBA-73A1D156A76D}" src="https://github.com/user-attachments/assets/cd53c44d-0902-4fdc-912d-7523d676c045" /></p>

✔ Domain đã trỏ về tunnel

✔ DNS trên Cloudflare đã OK

✔ Tunnel đã “gắn” với subdomain

Bước 8: Chạy Tunnel

- Chạy lệnh: `cloudflared tunnel run mytunnel`

<img width="1381" height="884" alt="{9AB31EE1-E2AA-4C60-AB49-B1E695831473}" src="https://github.com/user-attachments/assets/f2c3cc06-930a-4b7a-a469-eb2855fceab8" /></P>

- Mở trình duyệt: Truy cập `http://app.ducduong.id.vn`

<img width="1920" height="1080" alt="{91322C59-9CF0-4F29-9307-9FA34FF9E4FB}" src="https://github.com/user-attachments/assets/72438a3f-8ffb-4847-b78b-4150291d00bd" /></p>



