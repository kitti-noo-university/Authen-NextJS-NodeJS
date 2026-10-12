# Authen-NextJS-NodeJS

ตัวอย่างเว็บข้อมูลนักศึกษาที่สาธิตการสมัครสมาชิกและการยืนยันตัวตนด้วย JWT ประกอบด้วย frontend ที่พัฒนาด้วย Next.js และ backend API ที่ใช้ Express, Passport Local และ Passport JWT

> โปรเจกต์นี้เป็นตัวอย่างเพื่อการศึกษา ไม่ควรนำไปใช้กับบัญชีหรือข้อมูลจริง หรือถือว่าเป็นระบบยืนยันตัวตนที่พร้อมใช้งานจริง ข้อมูลผู้ใช้และข้อมูลนักศึกษาเก็บไว้ในหน่วยความจำ และข้อมูลจะหายเมื่อ backend เริ่มทำงานใหม่

## ความสามารถ

- แสดงรายการและจัดการข้อมูลนักศึกษาตัวอย่าง
- สมัครสมาชิกและเข้าสู่ระบบ
- เปิดดูหน้าโปรไฟล์ที่ต้องมี JWT และออกจากระบบ

## โครงสร้างและการทำงาน

```text
.
├── backend/                 # Express API, Passport strategies และข้อมูลในหน่วยความจำ
└── frontend/                # Next.js pages, components และการเรียก API
```

Frontend เรียก API ที่ `http://localhost/api` โดยค่าเริ่มต้น ส่วน backend ฟังที่ port `80` (หรือค่าจาก `PORT`) และกำหนด CORS ให้ origin `http://localhost:3000` เท่านั้น ในการพัฒนาให้เปิด frontend และ backend แยกกันคนละ terminal

| ส่วน | เทคโนโลยีและคำสั่ง |
| --- | --- |
| Frontend | Next.js 10, React 17, Axios — `npm run dev` (port 3000) |
| Backend | Node.js, Express, Passport Local/JWT, bcrypt — `node index.js` (port 80 โดยปริยาย) |
| Storage | ข้อมูลผู้ใช้และนักศึกษาอยู่ในหน่วยความจำ ไม่มีการเชื่อมต่อฐานข้อมูล |

## สิ่งที่ต้องมี

- Node.js และ npm (repository ไม่ได้ระบุเวอร์ชันขั้นต่ำ)
- Port `80` และ `3000` ว่างสำหรับค่าเริ่มต้น

## ติดตั้งและเริ่มใช้งาน

Clone repository แล้วติดตั้ง dependencies แยกในแต่ละโฟลเดอร์:

```bash
git clone https://github.com/kitti-noo-university/Authen-NextJS-NodeJS.git
cd Authen-NextJS-NodeJS/backend
npm install
```

เปิด terminal แรกเพื่อเริ่ม backend ในโหมด development:

```bash
cd Authen-NextJS-NodeJS/backend
NODE_ENV=development node index.js
```

บน PowerShell ให้ตั้งค่าและเริ่ม backend ด้วย:

```powershell
cd Authen-NextJS-NodeJS/backend
$env:NODE_ENV = "development"
node index.js
```

เปิด terminal ที่สองเพื่อติดตั้งและเริ่ม frontend:

```bash
cd Authen-NextJS-NodeJS/frontend
npm install
npm run dev
```

เปิดเว็บที่ [http://localhost:3000](http://localhost:3000) หากเปลี่ยน backend port ด้วย `PORT` ต้องปรับ URL ใน `frontend/config/config.js` ให้ตรงกันด้วย ค่า API URL ฝั่ง frontend ไม่ได้อ่านจาก environment variable; config จะเลือก URL สำหรับ production เมื่อ `NODE_ENV` เป็น `production` เท่านั้น

## สมัครสมาชิกและยืนยันตัวตน

1. ไปที่ **Register** แล้วสร้างผู้ใช้สำหรับการทดลองในเครื่อง จากนั้นไปที่ **Login** เพื่อเข้าสู่ระบบ
2. Backend ตรวจสอบ username/password ด้วย Passport Local และตอบกลับด้วย JWT พร้อมตั้ง cookie ชื่อ `token` ให้ browser
3. หน้า **Profile** ใช้ JWT ในการเรียก endpoint ที่มีการป้องกันด้วย Passport JWT
4. ไปที่ **Logout** เพื่อเรียก API ซึ่งล้าง cookie `token`

การสมัครสมาชิกและข้อมูลนักศึกษาถูกเก็บในหน่วยความจำของ backend เท่านั้น เมื่อ restart backend ข้อมูลที่เพิ่มระหว่างใช้งานจะหายไป อย่าใช้บัญชีหรือข้อมูลส่วนบุคคลจริง

## การตั้งค่าและ scripts

- `PORT` — backend ใช้กำหนด port ที่ Express ฟัง โดยค่าเริ่มต้นคือ `80`; หากเปลี่ยนค่านี้ URL ใน `frontend/config/config.js` ต้องตรงกัน
- `NODE_ENV` — frontend ใช้เลือก API URL ระหว่าง config สำหรับ development และ production; backend ใช้กำหนด `Secure` cookie โดยการพัฒนาแบบ HTTP ในเครื่องต้องตั้งเป็น `development`
- Backend ไม่มี `start` script ใน `backend/package.json`; เริ่มด้วย `node index.js`
- Frontend มี `dev`, `build` และ `start` scripts ใน `frontend/package.json`; สำหรับการทดลองในเครื่องใช้ `npm run dev`

ไม่มี environment variable สำหรับ JWT secret หรือการตั้งค่า database ที่กำหนดไว้ใน repository โปรเจกต์นี้มีค่าความลับอยู่ใน source code จึงไม่ควรนำไป deploy หรือใช้กับข้อมูลจริง

## แก้ปัญหาเบื้องต้น

- **เริ่ม backend ไม่ได้เพราะ port ถูกใช้งานหรือไม่มีสิทธิ์**: ตรวจสอบว่า port `80` ว่างและระบบอนุญาตให้ process ใช้งาน หากเลือก port อื่น ต้องปรับ URL ใน `frontend/config/config.js` ให้ตรงกัน
- **Frontend ติดต่อ backend ไม่ได้**: ตรวจสอบว่า backend ทำงานอยู่, URL ชี้ไปยัง backend ที่ถูกต้อง และเปิด frontend ที่ `http://localhost:3000` เพราะ CORS อนุญาต origin นี้เท่านั้น
- **เข้าสู่ระบบแล้ว cookie ไม่ทำงานบน HTTP ในเครื่อง**: ตรวจสอบว่าเริ่ม backend ด้วย `NODE_ENV=development` ตามตัวอย่างด้านบน
- **ข้อมูลที่สมัครหรือแก้ไขหายไป**: เป็นพฤติกรรมปกติของตัวอย่างนี้ เพราะ backend เก็บข้อมูลในหน่วยความจำและจะเริ่มข้อมูลใหม่เมื่อ restart

## README เพิ่มเติม

- [คู่มือ Backend](backend/README.md)
- [คู่มือ Frontend](frontend/README.md)
