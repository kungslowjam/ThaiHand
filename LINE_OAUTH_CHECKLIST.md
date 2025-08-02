# LINE OAuth Setup Checklist

## ✅ Environment Variables (ตรวจสอบแล้ว)
```bash
NEXT_PUBLIC_LINE_CLIENT_ID=2007700233
LINE_CLIENT_SECRET=e035e453d938989b8277dfe7c885dad6
LINE_CLIENT_ID=2007700233
```

## 🔧 การตรวจสอบใน LINE Developers Console

### 1. เข้าไปที่ LINE Developers Console
- URL: https://developers.line.biz/console/
- เข้าสู่ระบบด้วย LINE account

### 2. เลือก Channel
- ✅ เลือก Channel ที่มี Channel ID: `2007700233`
- ✅ ตรวจสอบว่า Channel Status เป็น "เปิดใช้งาน"

### 3. ตรวจสอบ LINE Login Settings
ไปที่ **LINE Login** tab และตรวจสอบ:

#### Callback URL:
- ✅ ต้องเป็น: `https://thaihand.shop/api/auth/callback/line`
- ✅ ไม่มี trailing slash
- ✅ ใช้ HTTPS เท่านั้น

#### Scope:
- ✅ ต้องเป็น: `profile openid`
- ✅ ไม่ต้องเพิ่ม `email` scope

#### Bot link option:
- ✅ ตั้งเป็น "ปิด"

#### Add friends:
- ✅ ตั้งเป็น "ปิด"

### 4. ตรวจสอบ Basic Settings
ไปที่ **Basic settings** tab และตรวจสอบ:

#### Channel name:
- ✅ ตั้งชื่อที่เหมาะสม เช่น "ThaiHand Login"

#### Channel description:
- ✅ ตั้งคำอธิบายที่เหมาะสม

## 🚀 การทดสอบ

### 1. ทดสอบ Callback URL
```bash
curl -I "https://thaihand.shop/api/auth/callback/line"
# ควรได้ 404 (ปกติสำหรับ callback URL)
```

### 2. ทดสอบ LINE API
```bash
curl -I "https://access.line.me/oauth2/v2.1/authorize"
# ควรได้ 200 OK
```

### 3. ทดสอบการเข้าสู่ระบบ
1. ไปที่ `https://thaihand.shop/login`
2. คลิกปุ่ม "เข้าสู่ระบบด้วย LINE"
3. ตรวจสอบ logs

## 🔍 การ Debug เพิ่มเติม

### หากยังมีปัญหา 400 Bad Request:

#### 1. ตรวจสอบ Channel Status
- ✅ ไปที่ LINE Developers Console
- ✅ ตรวจสอบว่า Channel เปิดใช้งานอยู่
- ✅ ตรวจสอบว่า Channel type เป็น "LINE Login"

#### 2. ตรวจสอบ Callback URL
- ✅ ตรวจสอบว่า Callback URL ตรงกับที่ตั้งไว้ใน LINE Developers Console
- ✅ ต้องเป็น: `https://thaihand.shop/api/auth/callback/line`
- ✅ ไม่มี trailing slash

#### 3. ตรวจสอบ Scope
- ✅ ตรวจสอบว่า Scope ตั้งเป็น `profile openid`
- ✅ ไม่ต้องเพิ่ม `email` scope (ต้องขอสิทธิ์ก่อน)

#### 4. ตรวจสอบ Bot Settings
- ✅ ตรวจสอบว่า Bot link option ปิด
- ✅ ตรวจสอบว่า Add friends ปิด

#### 5. ตรวจสอบ Channel Secret
- ✅ ตรวจสอบว่า Channel Secret ตรงกับที่ตั้งไว้
- ✅ ต้องเป็น: `e035e453d938989b8277dfe7c885dad6`

## 📊 การตรวจสอบสถานะ

### ตรวจสอบการทำงาน:
```bash
# ดู container status
docker-compose ps

# ดู logs ของ frontend
docker-compose logs frontend

# ดู environment variables
docker-compose exec frontend env | grep LINE
```

### ตรวจสอบ Network:
```bash
# ทดสอบการเชื่อมต่อกับ LINE API
curl -I https://access.line.me/oauth2/v2.1/authorize

# ทดสอบ DNS resolution
nslookup access.line.me
```

## 🎯 สรุปการแก้ไข

✅ **Environment Variables ถูกต้อง:**
- LINE_CLIENT_ID=2007700233
- LINE_CLIENT_SECRET=e035e453d938989b8277dfe7c885dad6
- NEXT_PUBLIC_LINE_CLIENT_ID=2007700233

🔧 **สิ่งที่ต้องตรวจสอบ:**
- การตั้งค่าใน LINE Developers Console
- Callback URL ต้องตรงกัน
- Scope ต้องถูกต้อง
- Channel Status ต้องเปิดใช้งาน

## 🔍 การ Debug เพิ่มเติม

### หากยังมีปัญหา:
1. ตรวจสอบ Channel settings ใน LINE Developers Console
2. ตรวจสอบ Callback URL ว่าตรงกัน
3. ตรวจสอบ Scope ว่าถูกต้อง
4. ตรวจสอบ Channel Status
5. ตรวจสอบ Bot Settings 