
## ตั้งค่า Firebase สำหรับ Flutter

### **Firebase คืออะไร?**

Firebase เป็นแพลตฟอร์มของ Google ที่ช่วยจัดการ Authentication, Database และ Storage สำหรับแอป

### **ขั้นตอนการตั้งค่า Firebase**

#### 5.1 สร้างโปรเจคใน Firebase

1. ไปที่: [Firebase Console](https://console.firebase.google.com/)
2. คลิก `Add project` → ตั้งชื่อโปรเจค → `Continue`
3. เปิดใช้งาน Google Analytics (หรือข้ามได้)

#### 5.2 เพิ่มแอปใน Firebase

1. คลิก `Android` → กรอก Package Name (เช่น `com.example.myapp`)
2. ดาวน์โหลดไฟล์ `google-services.json`
3. วางไฟล์ในโฟลเดอร์ `android/app/`

#### 5.3 เปิดใช้งาน Authentication

1. ไปที่ `Authentication` → `Sign-in method`
2. เปิดใช้งาน Email/Password และ Google Sign-in

#### 5.4 ตั้งค่า Firestore Database

1. ไปที่ `Firestore Database` → `Create database`
2. เลือกโหมด `Test Mode`

#### 5.5 เปิดใช้งาน Storage

1. ไปที่ `Storage` → `Get Started`
2. ตั้งค่ากฎเป็น:
   ```
   allow read, write: if request.auth != null;
   ```

---

## 6. ทดสอบการเชื่อมต่อ Firebase กับ Flutter

1. เพิ่ม Firebase SDK ใน `pubspec.yaml`:
   ```yaml
   dependencies:
     firebase_core: latest_version
     firebase_auth: latest_version
   ```
2. รันคำสั่งติดตั้งแพ็กเกจ:
   ```sh
   flutter pub get
   ```
3. ทดสอบการเชื่อมต่อใน `main.dart`:

   ```dart
   import 'package:firebase_core/firebase_core.dart';
   import 'package:flutter/material.dart';

   void main() async {
     WidgetsFlutterBinding.ensureInitialized();
     await Firebase.initializeApp();
     runApp(MyApp());
   }
   ```

4. รันแอป:
   ```sh
   flutter run
   ```

---

✅ **สรุป**: หลังจากติดตั้งและตั้งค่าเสร็จแล้ว คุณสามารถเริ่มพัฒนาแอป Flutter ได้ทันที! 🚀
