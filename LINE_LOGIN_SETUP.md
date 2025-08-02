# การตั้งค่า LINE Login

## ✅ สถานะปัจจุบัน
- ✅ Environment variables ถูกตั้งค่าแล้ว
- ✅ Docker configuration พร้อมใช้งาน
- ✅ NextAuth configuration เรียบง่าย
- ✅ เพิ่ม error handling สำหรับ ECONNRESET

## 🔧 การตั้งค่าใน LINE Developers Console

### 1. ไปที่ LINE Developers Console
- URL: https://developers.line.biz/console/
- เข้าสู่ระบบด้วย LINE account

### 2. สร้าง Channel ใหม่ (ถ้ายังไม่มี)
1. คลิก "Create Channel"
2. เลือก "LINE Login"
3. กรอกข้อมูลพื้นฐาน:
   - Channel name: `ThaiHand Login`
   - Channel description: `Login for ThaiHand platform`

### 3. ตั้งค่า Callback URL
ใน Channel ที่สร้างไว้:
- **Callback URL:** `https://thaihand.shop/api/auth/callback/line`
- **Scope:** `profile openid`

### 4. เก็บข้อมูลสำคัญ
- **Channel ID:** `2007700233` (ใช้เป็น LINE_CLIENT_ID)
- **Channel Secret:** `e035e453d938989b8277dfe7c885dad6` (ใช้เป็น LINE_CLIENT_SECRET)

## 📋 Environment Variables ที่ตั้งค่าแล้ว

```env
# LINE OAuth
LINE_CLIENT_ID=2007700233
LINE_CLIENT_SECRET=e035e453d938989b8277dfe7c885dad6
NEXT_PUBLIC_LINE_CLIENT_ID=2007700233

# NextAuth
NEXTAUTH_URL=https://thaihand.shop
NEXTAUTH_SECRET=z9y8x7w6v5u4t3s2r1q0p9o8n7m6l5k4j3i2h1g0f9e8d7c6b5a4
```

## 🚀 การทดสอบ

### 1. รีสตาร์ทเซิร์ฟเวอร์
```bash
docker-compose down
docker-compose up -d
```

### 2. ตรวจสอบ logs
```bash
# ดู logs ของ frontend
docker-compose logs frontend

# ดู logs ของ backend
docker-compose logs backend
```

### 3. ทดสอบการเข้าสู่ระบบ
1. ไปที่ `https://thaihand.shop/login`
2. คลิกปุ่ม "เข้าสู่ระบบด้วย LINE"
3. ตรวจสอบว่าสามารถเข้าสู่ระบบได้

## 🔍 การแก้ไขปัญหา

### ปัญหาที่พบบ่อย:

#### 1. "ECONNRESET" Error
**สาเหตุ:** การเชื่อมต่อกับ LINE API ถูกตัด
**วิธีแก้ไข:**
- ตรวจสอบการเชื่อมต่ออินเทอร์เน็ต
- ลองใหม่อีกครั้ง (ระบบมี retry button)
- ตรวจสอบว่า LINE API ไม่มีปัญหา

#### 2. "Invalid client_id"
**สาเหตุ:** LINE_CLIENT_ID ไม่ถูกต้อง
**วิธีแก้ไข:**
- ตรวจสอบ Channel ID ใน LINE Developers Console
- ตรวจสอบว่า Channel เปิดใช้งานอยู่

#### 3. "Invalid redirect_uri"
**สาเหตุ:** Callback URL ไม่ตรงกัน
**วิธีแก้ไข:**
- ตรวจสอบ Callback URL ใน LINE Developers Console
- ต้องเป็น: `https://thaihand.shop/api/auth/callback/line`

#### 4. "Access denied"
**สาเหตุ:** ผู้ใช้ยกเลิกการเข้าสู่ระบบ
**วิธีแก้ไข:**
- ลองใหม่อีกครั้ง
- ตรวจสอบว่า LINE app ทำงานปกติ

#### 5. "Configuration error"
**สาเหตุ:** Environment variables ไม่ถูกต้อง
**วิธีแก้ไข:**
- ตรวจสอบไฟล์ .env
- รีสตาร์ท Docker containers

#### 6. "Timeout Error"
**สาเหตุ:** การเชื่อมต่อใช้เวลานานเกินไป
**วิธีแก้ไข:**
- ตรวจสอบความเร็วอินเทอร์เน็ต
- ลองใหม่อีกครั้ง
- ตรวจสอบว่า LINE API ไม่มีปัญหา

## 🛠️ การปรับปรุงล่าสุด

### เพิ่ม Error Handling:
- ✅ จัดการ ECONNRESET error
- ✅ เพิ่ม timeout 30 วินาที
- ✅ เพิ่ม retry button
- ✅ แสดงข้อความ error ที่ชัดเจน

### เพิ่ม Debug Logs:
- ✅ ดู logs ได้ใน real-time
- ✅ แสดง attempt number
- ✅ แสดง error details

## 📁 โครงสร้างไฟล์

```
frontend/
├── app/
│   ├── api/
│   │   └── auth/
│   │       └── [...nextauth]/
│   │           └── route.ts          # NextAuth configuration
│   └── login/
│       └── page.tsx                  # Login page with retry logic
```

## 🔒 ความปลอดภัย

### ข้อควรระวัง:
- ✅ LINE_CLIENT_SECRET ไม่ถูกเปิดเผยในโค้ด
- ✅ ใช้ environment variables
- ✅ ใช้ HTTPS ใน production
- ✅ Callback URL ถูกตั้งค่าถูกต้อง

### การตรวจสอบ:
```bash
# ตรวจสอบ environment variables
docker-compose exec frontend env | grep LINE

# ตรวจสอบ logs
docker-compose logs frontend | grep -i line

# ตรวจสอบ error logs
docker-compose logs frontend | grep -i error
```

## 📊 การตรวจสอบสถานะ

### ตรวจสอบการทำงาน:
1. **Frontend:** `https://thaihand.shop/login`
2. **API:** `https://thaihand.shop/api/auth/callback/line`
3. **Dashboard:** `https://thaihand.shop/dashboard`

### ตรวจสอบ logs:
```bash
# Real-time logs
docker-compose logs -f frontend

# Error logs
docker-compose logs frontend | grep -i error

# LINE specific logs
docker-compose logs frontend | grep -i line
```

## 🎯 สรุป

✅ **การตั้งค่าสำเร็จ:**
- LINE OAuth ถูกตั้งค่าแล้ว
- Environment variables ครบถ้วน
- Docker configuration พร้อมใช้งาน
- NextAuth configuration เรียบง่าย
- เพิ่ม error handling สำหรับ ECONNRESET

🚀 **พร้อมใช้งาน:**
- ผู้ใช้สามารถเข้าสู่ระบบด้วย LINE ได้
- ระบบจะ redirect ไปยัง dashboard หลังเข้าสู่ระบบสำเร็จ
- Error handling เรียบง่ายและเข้าใจง่าย
- มี retry button สำหรับปัญหาการเชื่อมต่อ 