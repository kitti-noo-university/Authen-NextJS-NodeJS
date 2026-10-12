# Backend

บริการ API สำหรับตัวอย่างเว็บข้อมูลนักศึกษา ใช้ Node.js, Express, Passport Local/JWT และ bcrypt ข้อมูลผู้ใช้กับข้อมูลนักศึกษาถูกเก็บในหน่วยความจำ ไม่มีการเชื่อมต่อฐานข้อมูล และข้อมูลที่เพิ่มระหว่างใช้งานจะหายเมื่อ backend เริ่มใหม่

> ใช้เพื่อการศึกษาเท่านั้น อย่าใช้บัญชีหรือข้อมูลจริง และอย่านำระบบนี้ไปใช้งานจริงโดยไม่ออกแบบและตรวจสอบความปลอดภัยเพิ่มเติม

## ติดตั้งและเริ่มใช้งาน

ต้องติดตั้ง Node.js และ npm ก่อน (repository ไม่ได้ระบุเวอร์ชันขั้นต่ำ) จาก root ของ repository:

```bash
cd backend
npm install
NODE_ENV=development node index.js
```

Backend ฟังที่ port `80` โดยปริยาย ค่า `NODE_ENV=development` ทำให้ cookie ใช้ได้กับการพัฒนาแบบ HTTP ในเครื่อง หากใช้ PowerShell:

```powershell
cd backend
npm install
$env:NODE_ENV = "development"
node index.js
```

หาก port `80` ไม่ว่าง สามารถกำหนด `PORT` ให้ backend ได้ แต่ต้องปรับ URL ใน [`../frontend/config/config.js`](../frontend/config/config.js) ให้ตรงกันด้วย Frontend ตั้งค่า API URL ไว้เป็น `http://localhost/api` โดยค่าเริ่มต้น และ CORS อนุญาต origin `http://localhost:3000`

ไม่มี `start` script ใน `package.json`; ใช้ `node index.js` เพื่อเริ่มบริการ

## API ที่เกี่ยวกับการยืนยันตัวตน

ทุก path ด้านล่างมี prefix `/api`:

| Method | Path | การทำงาน |
| --- | --- | --- |
| `POST` | `/register` | สมัครสมาชิก; เพิ่มผู้ใช้ในหน่วยความจำ |
| `POST` | `/login` | ตรวจสอบข้อมูลด้วย Passport Local และตั้ง cookie `token` |
| `GET` | `/profile` | ตรวจสอบ JWT จากส่วนหัว Authorization แล้วส่งข้อมูลผู้ใช้กลับ |
| `GET` | `/logout` | ล้าง cookie `token` |

Frontend ใช้ cookie ที่ได้จากการเข้าสู่ระบบเพื่อส่ง JWT ไปยังหน้า profile ส่วนการออกจากระบบเรียก endpoint ที่ล้าง cookie

## การตั้งค่า

- `PORT` — port ของ Express; ค่าเริ่มต้น `80`
- `NODE_ENV` — cookie จะตั้ง `Secure` เมื่อค่าไม่ใช่ `development`; ใช้ `development` สำหรับการทดสอบผ่าน HTTP ในเครื่อง

ไม่มีการตั้งค่า database หรือ environment variable สำหรับ JWT secret ใน repository ค่าความลับปัจจุบันอยู่ใน source code จึงไม่ควรใช้ระบบนี้กับบัญชีหรือข้อมูลจริง

ดูภาพรวมและวิธีเริ่ม frontend ที่ [README หลัก](../README.md)
