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

Bước 5: Cài cloudflared

`sudo apt update`

`sudo apt install cloudflared -y`

` cloudflared --version`

<img width="1370" height="301" alt="image" src="https://github.com/user-attachments/assets/93aab7e9-c332-478d-ab03-6880e59c39be" /></p>

Bước 6: Gắn Domain với Tunnel

- Chạy lệnh: `cloudflared tunnel route dns mytunnel app.ducduong.id.vn`
  
<img width="1368" height="321" alt="{D1F90F68-503F-480D-9DBA-73A1D156A76D}" src="https://github.com/user-attachments/assets/cd53c44d-0902-4fdc-912d-7523d676c045" /></p>

✔ Domain đã trỏ về tunnel

✔ DNS trên Cloudflare đã OK

✔ Tunnel đã “gắn” với subdomain

Bước 7: Chạy Tunnel

- Chạy lệnh: `cloudflared tunnel run mytunnel`

<img width="1381" height="884" alt="{9AB31EE1-E2AA-4C60-AB49-B1E695831473}" src="https://github.com/user-attachments/assets/f2c3cc06-930a-4b7a-a469-eb2855fceab8" /></P>

- Mở trình duyệt: Truy cập `http://app.ducduong.id.vn`

<img width="1920" height="1080" alt="{91322C59-9CF0-4F29-9307-9FA34FF9E4FB}" src="https://github.com/user-attachments/assets/72438a3f-8ffb-4847-b78b-4150291d00bd" /></p>

### 2. CONVERT LỆNH DOCKER RUN -> DOCKER COMPOSE

Bước 1: Mở file docker-compose.yml

<img width="1167" height="854" alt="{1BA67DE6-C99E-47C1-89A0-1642444E548F}" src="https://github.com/user-attachments/assets/919e8112-7e5e-4a7b-bf11-f02d32df2625" /></p>

Bước 2: Thêm service
`
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: always
    command: tunnel run mytunnel
    volumes:
      - /home/duong/.cloudflared:/home/nonroot/.cloudflared
    depends_on:
      - nginx
`

<img width="843" height="641" alt="{A94AB1FB-F0DC-4A9D-81EA-FB5FDD2859B2}" src="https://github.com/user-attachments/assets/d95891b9-c356-435f-b385-20acf7c355b2" /></p>

Bước 3: Sửa config tunnel để trỏ vào Docker

- Mở file: `nano ~/.cloudflared/config.yml`

- Sửa thành như sau:

<img width="852" height="364" alt="{3A9C0FB7-4534-4062-A42B-3858CE294E4C}" src="https://github.com/user-attachments/assets/9c0a3b57-badc-4763-a1bd-1867b093a5a6" /></p>

### 3: THÊM ROUTER

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

🧠 GIẢI THÍCH

- hostname: subdomain

- service: container đích

- ingress: router

### 4: CHẠY LẠI DOCKER COMPOSE

`cd ~/myapp`

`docker compose down`

`docker compose up -d`

` docker compose ps`

<img width="1103" height="529" alt="image" src="https://github.com/user-attachments/assets/de90a14e-c17f-42cb-86ba-55c858341f73" /></p>

Bước 5: Test lại 

1. WEB

<img width="1920" height="1035" alt="{47EE78F6-E314-498A-A5CB-C22E7EF5EACE}" src="https://github.com/user-attachments/assets/6c61f773-780c-4bdf-88fd-9fe246761e05" /></p>

2. Nodered

<img width="1920" height="1028" alt="{CF6EAA0E-E8EA-4813-ADB0-1650512E6137}" src="https://github.com/user-attachments/assets/4f785a86-1002-4ac7-baa8-aaf889d9b3f6" /></p>

- Test trên thiết bị khác không sử dụng wifi

![test](https://github.com/user-attachments/assets/35e9c529-0e4a-45c6-ae89-932da1e71024)</p>

---

## 📘 G. Câu hỏi về bài làm
---

### ❓ 1. Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?

- Sử dụng Nginx làm reverse proxy giúp:

- Tách biệt frontend (web) và backend (API)
  
- Định tuyến linh hoạt:
  
/ → web tĩnh (HTML)

/api → Node-RED hoặc Flask

- Tăng bảo mật (không expose trực tiếp Node-RED)
  
- Dễ mở rộng hệ thống (có thể thêm nhiều service phía sau Nginx)

👉 Nếu trỏ trực tiếp Tunnel vào Node-RED:

- Không phục vụ được web tĩnh
  
- Khó mở rộng
  
- Thiếu lớp trung gian để kiểm soát request
- 
---

### ❓ 2. Sự khác biệt giữa Mount file và Mount thư mục trong Docker

| Tiêu chí     | Mount file              | Mount thư mục           |
|--------------|------------------------|------------------------|
| Phạm vi      | 1 file cụ thể          | Toàn bộ thư mục        |
| Mục đích     | File cấu hình (nginx)  | Dữ liệu / source code  |
| Ví dụ        | /nginx.conf            | /myweb/                |

---

### ❓ 3. Nếu thay đổi index.html trên Ubuntu, web có thay đổi ngay không?

👉 Có, thay đổi ngay

- Vì sử dụng bind mount:

./myweb:/myweb

→ Container đọc trực tiếp file từ máy host

→ Không cần build lại image hay restart container

--- 

### ❓ 4. restart: always và restart: unless-stopped để làm gì?

- restart: always
  
→ Container luôn tự chạy lại khi bị lỗi hoặc reboot máy

- restart: unless-stopped
  
→ Giống always, nhưng nếu bạn chủ động stop thì nó sẽ không tự chạy lại

👉 Giúp hệ thống ổn định, tự phục hồi khi lỗi

---

### 5. Khai báo các service dùng chung network

📌 Cấu hình:

`
networks:
  mynetwork:

services:
  mynginx:
    networks:
      - mynetwork

  mynodered:
    networks:
      - mynetwork

  myapi:
    networks:
      - mynetwork

  cloudflared:
    networks:
      - mynetwork
`

🎯 Lợi ích:

- Các container gọi nhau bằng tên service (DNS nội bộ)
  
- Không cần IP

- Dễ quản lý và mở rộng hệ thống

---

### 6. Đưa Cloudflare Token vào .env và thêm .gitignore – tại sao quan trọng?

📌 Ví dụ .env:

`CLOUDFLARE_TOKEN=your_token_here`

📌 .gitignore:

`.env`

🎯 Lý do:

- Token là thông tin nhạy cảm

- Nếu bị lộ:

→ Có thể bị chiếm quyền domain

→ Bị phá tunnel hoặc redirect traffic

👉 Nguyên tắc: Không commit secrets lên GitHub

---

### ❓ 7. Tại sao nên thêm :ro khi mount file Nginx?

`./nginx/nginx.conf:/etc/nginx/nginx.conf:ro`

🎯 Ý nghĩa:

- ro = read-only (chỉ đọc)

🔥 Lợi ích:

- Tránh container sửa file config
  
- Tăng bảo mật
  
- Giảm rủi ro lỗi cấu hình

---

### ❓ 8. Khi dùng Cloudflare Tunnel có cần mở port không?

👉 Không cần

🧠 Vì:

- Tunnel tạo kết nối từ server → Cloudflare (outbound)

- Không cần mở port inbound

🎯 Lợi ích:

- Không cần cấu hình router / NAT

- An toàn hơn (không lộ port)

- Dễ triển khai
