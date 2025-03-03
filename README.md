---

# Mini E-Commerce Flutter App - คู่มือเริ่มต้นจากศูนย์

ยินดีต้อนรับสู่คู่มือการสร้างแอปพลิเคชันร้านค้าออนไลน์ขนาดเล็กด้วย **Flutter**, **GetX**, และ **Firebase**! คู่มือนี้จะพาคุณเริ่มต้นจากศูนย์ ตั้งแต่การติดตั้งเครื่องมือ ไปจนถึงการสร้างและรันแอป โดยอธิบายทุกอย่างแบบ step-by-step เหมาะสำหรับผู้เริ่มต้นที่ยังไม่มีพื้นฐาน

---

## ภาพรวม

แอปนี้เป็นร้านค้าออนไลน์ที่ให้:
- **ผู้ใช้ทั่วไป (User):** ดูสินค้า, เพิ่มลงตะกร้า, สั่งซื้อ, และดูประวัติ
- **ผู้ดูแลระบบ (Admin):** เพิ่ม, แก้ไข, และลบสินค้า

เราจะใช้เครื่องมือดังต่อไปนี้:
- **Flutter:** Framework สำหรับสร้างแอป
- **GetX:** ช่วยจัดการสถานะและการนำทางในแอป
- **Firebase:** ระบบ backend สำหรับล็อกอิน, ฐานข้อมูล, และเก็บรูปภาพ

---

## การติดตั้งเครื่องมือ

### 1. ติดตั้ง Flutter
#### Flutter คืออะไร?
Flutter เป็นเครื่องมือจาก Google ใช้สร้างแอปที่รันได้ทั้ง Android และ iOS จากโค้ดเดียว

#### ขั้นตอน:
1. **ดาวน์โหลด Flutter SDK**
   - ไปที่: [Flutter Install](https://flutter.dev/docs/get-started/install)
   - เลือกเวอร์ชันสำหรับระบบปฏิบัติการของคุณ (Windows, macOS, Linux)
   - แตกไฟล์ ZIP และวางโฟลเดอร์ `flutter` ในตำแหน่งที่ต้องการ (เช่น `C:\flutter`)

2. **ตั้งค่า Path**
   - เพิ่ม `flutter/bin` เข้าไปใน Environment Variables (Windows) หรือ `.bashrc` (Linux/macOS)
   - ตัวอย่างใน Windows:
     - Control Panel → System → Advanced system settings → Environment Variables → Path → เพิ่ม `C:\flutter\bin`

3. **ตรวจสอบการติดตั้ง**
   - เปิด Terminal หรือ Command Prompt
   - รัน: `flutter doctor`
   - ถ้าติดตั้งสำเร็จ จะเห็นรายการตรวจสอบ (อาจต้องติดตั้งเพิ่มเติมตามคำแนะนำ)

---

### 2. ติดตั้ง IDE
#### IDE คืออะไร?
IDE (Integrated Development Environment) คือโปรแกรมที่ใช้เขียนโค้ดและรันแอป

#### ตัวเลือกแนะนำ:
- **VS Code:** เล็ก, เร็ว, เหมาะกับ Flutter
  - ดาวน์โหลด: [VS Code](https://code.visualstudio.com/)
  - ติดตั้ง Extensions:
    - `Flutter`
    - `Dart`
- **Android Studio:** ครบเครื่อง, เหมาะกับ Android
  - ดาวน์โหลด: [Android Studio](https://developer.android.com/studio)

#### ขั้นตอน:
1. ดาวน์โหลดและติดตั้ง IDE ที่เลือก
2. เปิด IDE และตั้งค่า Flutter (ใน VS Code: กด F1 → พิมพ์ `Flutter: New Project`)

---

### 3. ติดตั้ง Android Emulator หรือ iOS Simulator
#### Emulator/Simulator คืออะไร?
เครื่องจำลองที่ใช้ทดสอบแอปบนคอมพิวเตอร์

#### ขั้นตอน:
- **Android Emulator (Windows/macOS/Linux):**
  - ติดตั้ง Android Studio (มี Android SDK มาให้)
  - เปิด AVD Manager → Create Virtual Device → เลือกอุปกรณ์ (เช่น Pixel 4) → ดาวน์โหลด Android Version (เช่น API 30)
- **iOS Simulator (macOS เท่านั้น):**
  - ติดตั้ง Xcode: [Mac App Store](https://apps.apple.com/us/app/xcode/id497799835)
  - เปิด Xcode → Preferences → Components → ดาวน์โหลด Simulator

---

### 4. ตั้งค่า Firebase
#### Firebase คืออะไร?
Firebase เป็นบริการจาก Google ช่วยจัดการล็อกอิน, ฐานข้อมูล, และไฟล์ในแอป

#### ขั้นตอน:
1. **สร้างบัญชี Firebase**
   - ไปที่: [Firebase Console](https://console.firebase.google.com/)
   - คลิก "Add project" → ตั้งชื่อ (เช่น "Mini E-Commerce") → สร้าง

2. **เพิ่มแอปใน Firebase**
   - คลิก "Android" → ใส่ Package Name (เช่น `com.example.mini_ecommerce`)
   - ดาวน์โหลด `google-services.json`
   - วางไฟล์ใน `android/app/`

3. **เปิดใช้งานบริการ**
   - **Authentication:** ไปที่ Authentication → Sign-in method → เปิด Email/Password และ Google
   - **Firestore:** ไปที่ Firestore Database → Create database (เลือก Test Mode)
   - **Storage:** ไปที่ Storage → Get Started

---

## เริ่มต้นสร้างโปรเจกต์

### 1. สร้างโปรเจกต์ Flutter ใหม่
#### ขั้นตอน:
1. เปิด Terminal หรือ IDE
2. รันคำสั่ง:
   ```bash
   flutter create mini_ecommerce
   cd mini_ecommerce
   ```
3. เปิดโปรเจกต์ใน IDE:
   - VS Code: `code .`
   - Android Studio: เปิดโฟลเดอร์ `mini_ecommerce`

---

### 2. เพิ่ม Dependencies
#### Dependency คืออะไร?
แพ็กเกจที่เพิ่มฟังก์ชันให้ Flutter

#### ขั้นตอน:
1. เปิดไฟล์ `pubspec.yaml`
2. เพิ่ม dependencies นี้:
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     firebase_core: ^2.4.0        # เชื่อมต่อ Firebase
     firebase_auth: ^4.2.0        # ล็อกอิน
     cloud_firestore: ^4.3.0      # ฐานข้อมูล
     firebase_storage: ^11.0.0    # เก็บรูปภาพ
     get: ^4.6.5                 # State management และ navigation
     image_picker: ^0.8.6        # เลือกรูปภาพ
   ```
3. รันใน Terminal:
   ```bash
   flutter pub get
   ```

---

### 3. สร้างโครงสร้างโฟลเดอร์
#### โครงสร้างคืออะไร?
การจัดระเบียบไฟล์เพื่อให้โค้ดอ่านง่ายและขยายได้

#### โครงสร้างแนะนำ:
```
lib/
├── core/                  # ส่วนกลาง
│   ├── constants/        # ค่าคงที่ (เช่น สี)
│   └── widgets/          # วิดเจ็ตที่ใช้ซ้ำ
├── data/                 # ข้อมูล
│   ├── models/           # โมเดลข้อมูล
│   └── services/         # การเชื่อมต่อ Firebase
├── logic/                # ตรรกะ
│   ├── controllers/      # GetX Controllers
│   └── bindings/         # Dependency Injection
├── presentation/         # UI
│   ├── screens/          # หน้าต่าง ๆ
│   └── widgets/          # วิดเจ็ตย่อย
└── main.dart             # จุดเริ่มต้น
```

#### ขั้นตอน:
1. เปิดโฟลเดอร์ `lib/`
2. สร้างโฟลเดอร์ตามโครงสร้างด้านบน (ใช้ IDE หรือ File Explorer)

---

### 4. เขียนโค้ดเริ่มต้น
#### main.dart
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:get/get.dart';
import 'logic/bindings/app_binding.dart';
import 'presentation/screens/login_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(); // เชื่อมต่อ Firebase
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GetMaterialApp(
      title: 'Mini E-Commerce',
      initialBinding: AppBinding(), // ตั้งค่า GetX
      home: LoginScreen(),
      theme: ThemeData(primarySwatch: Colors.blue),
    );
  }
}
```

#### อธิบาย:
- `Firebase.initializeApp()`: เริ่มต้น Firebase
- `GetMaterialApp`: ใช้ GetX แทน MaterialApp เพื่อให้ใช้ Navigation ได้

---

### 5. สร้างโมเดลพื้นฐาน
#### data/models/user_model.dart
```dart
class UserModel {
  final String uid;
  final String email;
  final String? name;
  final String role;

  UserModel({
    required this.uid,
    required this.email,
    this.name,
    required this.role,
  });

  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      uid: json['uid'],
      email: json['email'],
      name: json['name'],
      role: json['role'] ?? 'user',
    );
  }

  Map<String, dynamic> toJson() {
    return {
      'uid': uid,
      'email': email,
      'name': name,
      'role': role,
    };
  }
}
```

#### อธิบาย:
- `UserModel`: โมเดลสำหรับเก็บข้อมูลผู้ใช้ (uid, email, role)
- ใช้สำหรับแยกบทบาท User และ Admin

---

### 6. สร้างระบบล็อกอิน
#### data/services/auth_service.dart
```dart
import 'package:firebase_auth/firebase_auth.dart';
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/user_model.dart';

class AuthService {
  final FirebaseAuth _auth = FirebaseAuth.instance;
  final FirebaseFirestore _firestore = FirebaseFirestore.instance;

  Future<UserModel?> login(String email, String password) async {
    try {
      UserCredential userCredential = await _auth.signInWithEmailAndPassword(
        email: email,
        password: password,
      );
      DocumentSnapshot doc = await _firestore.collection('users').doc(userCredential.user!.uid).get();
      return UserModel.fromJson(doc.data() as Map<String, dynamic>);
    } catch (e) {
      throw e.toString();
    }
  }

  Future<void> logout() async {
    await _auth.signOut();
  }
}
```

#### logic/controllers/auth_controller.dart
```dart
import 'package:get/get.dart';
import '../../data/models/user_model.dart';
import '../../data/services/auth_service.dart';
import '../../presentation/screens/login_screen.dart';
import '../../presentation/screens/user/home_screen.dart';
import '../../presentation/screens/admin/admin_product_screen.dart';

class AuthController extends GetxController {
  var user = Rxn<UserModel>();
  final AuthService _authService = AuthService();

  Future<void> login(String email, String password) async {
    try {
      UserModel? loggedUser = await _authService.login(email, password);
      if (loggedUser != null) {
        user.value = loggedUser;
        if (loggedUser.role == 'admin') {
          Get.offAll(() => AdminProductScreen());
        } else {
          Get.offAll(() => HomeScreen());
        }
      }
    } catch (e) {
      Get.snackbar('Error', e.toString());
    }
  }

  Future<void> logout() async {
    await _authService.logout();
    user.value = null;
    Get.offAll(() => LoginScreen());
  }
}
```

#### presentation/screens/login_screen.dart
```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';
import '../../logic/controllers/auth_controller.dart';

class LoginScreen extends StatelessWidget {
  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();
  final AuthController authController = Get.find();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Login')),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: emailController,
              decoration: InputDecoration(labelText: 'Email'),
            ),
            TextField(
              controller: passwordController,
              decoration: InputDecoration(labelText: 'Password'),
              obscureText: true,
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                authController.login(emailController.text, passwordController.text);
              },
              child: Text('Login'),
            ),
          ],
        ),
      ),
    );
  }
}
```

#### อธิบาย:
- `AuthService`: เชื่อมต่อ Firebase Authentication และ Firestore
- `AuthController`: จัดการล็อกอินด้วย GetX
- `LoginScreen`: UI สำหรับกรอกข้อมูลล็อกอิน

---

### 7. รันแอปครั้งแรก
#### ขั้นตอน:
1. เปิด Emulator หรือเชื่อมต่อโทรศัพท์
2. รันคำสั่ง:
   ```bash
   flutter run
   ```
3. ถ้าไม่มีข้อผิดพลาด คุณจะเห็นหน้า Login

#### การทดสอบ:
- เพิ่มข้อมูลผู้ใช้ใน Firestore:
  ```json
  {
    "uid": "test_uid",
    "email": "test@example.com",
    "role": "user"
  }
  {
    "uid": "admin_uid",
    "email": "admin@example.com",
    "role": "admin"
  }
  ```
- ล็อกอินด้วย email/password ที่สร้างใน Firebase Authentication

---

## พัฒนาต่อ
เมื่อแอปพื้นฐานทำงานได้ คุณสามารถเพิ่มส่วนอื่น ๆ เช่น:
- **HomeScreen:** แสดงสินค้า
- **AdminProductScreen:** จัดการสินค้า
- **CartScreen:** ตะกร้าสินค้า
ดูโค้ดเต็มได้ในส่วนถัดไปของคู่มือนี้ (ถ้าต้องการ)

---

## ทรัพยากรเพิ่มเติม
- [Flutter Documentation](https://flutter.dev/)
- [GetX Documentation](https://pub.dev/packages/get)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Dart Language](https://dart.dev/)

---

## คำแนะนำ
- ถ้ามีปัญหา ให้รัน `flutter doctor` เพื่อตรวจสอบ
- เริ่มจากโค้ดพื้นฐาน แล้วค่อย ๆ เพิ่มฟีเจอร์
- ถ้าต้องการความช่วยเหลือ ถามในชุมชน Flutter (เช่น Stack Overflow)

---

นี่คือ `README.md` ที่ออกแบบสำหรับผู้เริ่มต้นจากศูนย์ อธิบายทุกอย่างตั้งแต่การติดตั้งจนถึงการสร้างโปรเจกต์ ถ้าคุณต้องการให้เพิ่มโค้ดเต็มทั้งหมด (ตามที่ให้ไปก่อนหน้า) หรือปรับส่วนใด เช่น แปลเป็นไทย บอกผมได้เลยครับ!
