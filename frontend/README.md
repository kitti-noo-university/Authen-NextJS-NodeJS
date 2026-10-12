# Frontend

เว็บตัวอย่างข้อมูลนักศึกษาที่พัฒนาด้วย Next.js 10 และ React 17 ใช้ Axios เรียก backend API

## ติดตั้งและเริ่มใช้งาน

ต้องติดตั้ง Node.js และ npm ก่อน (repository ไม่ได้ระบุเวอร์ชันขั้นต่ำ) จาก root ของ repository:

```bash
cd frontend
npm install
npm run dev
```

เปิดเว็บที่ [http://localhost:3000](http://localhost:3000) และเปิด backend แยกอีก terminal ตามคำแนะนำใน [Backend README](../backend/README.md)

## หน้าเว็บ

- `/` — หน้าแรก
- `/showStudents` และ `/editStudents` — แสดงและจัดการข้อมูลนักศึกษาตัวอย่าง
- `/register` — สมัครสมาชิก
- `/login` — เข้าสู่ระบบ
- `/profile` — ดูโปรไฟล์โดยใช้ JWT
- `/logout` — ออกจากระบบและล้าง cookie ผ่าน backend
- `/getConfig` — แสดงค่า config ของ API

## การตั้งค่า API

ค่า URL อยู่ใน [`config/config.js`](config/config.js) โดย development เรียก `http://localhost/api` และเมื่อ `NODE_ENV` เป็น `production` จะเลือก URL สำหรับ production ที่กำหนดไว้ในไฟล์นี้ ไม่มี environment variable สำหรับกำหนด API URL

สำหรับการพัฒนาในเครื่อง ให้เปิด frontend ที่ `http://localhost:3000` และ backend ที่ port `80` ตามค่าเริ่มต้น; backend CORS อนุญาตเฉพาะ origin นี้ หากเปลี่ยน backend port ต้องปรับ URL ใน config ให้ตรงกับ port ใหม่

Scripts ที่กำหนดใน `package.json`:

- `npm run dev` — เริ่ม development server
- `npm run build` — build frontend
- `npm start` — เริ่ม Next.js server หลัง build

ดูภาพรวมการติดตั้งและวิธีใช้งานที่ [README หลัก](../README.md)
