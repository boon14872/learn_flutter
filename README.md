---

# Mini E-Commerce Flutter App - คู่มือเริ่มต้นจากศูนย์

ยินดีต้อนรับสู่คู่มือการสร้างแอปพลิเคชันร้านค้าออนไลน์ขนาดเล็กด้วย **Flutter**, **GetX**, และ **Firebase**! คู่มือนี้จะพาคุณเริ่มต้นจากศูนย์ ตั้งแต่การติดตั้งเครื่องมือ ไปจนถึงการสร้างและรันแอป โดยอธิบายทุกอย่างแบบ step-by-step เหมาะสำหรับผู้เริ่มต้นที่ยังไม่มีพื้นฐาน

---

## สารบัญ
1. [ภาพรวม](#ภาพรวม)
2. [การติดตั้งเครื่องมือ](2-flutter-install.md)
3. [setup firebase](3-setup-firebase.md)
4. [เริ่มต้นสร้างโปรเจกต์](#เริ่มต้นสร้างโปรเจกต์)
  - [สร้างโปรเจกต์ Flutter ใหม่](#สร้างโปรเจกต์-flutter-ใหม่)
  - [เพิ่ม Dependencies](#เพิ่ม-dependencies)
  - [สร้างโครงสร้างโฟลเดอร์](#สร้างโครงสร้างโฟลเดอร์)
  - [เขียนโค้ดเริ่มต้น](#เขียนโค้ดเริ่มต้น)
  - [สร้างโมเดลพื้นฐาน](#สร้างโมเดลพื้นฐาน)
  - [สร้างระบบล็อกอิน](#สร้างระบบล็อกอิน)
  - [รันแอปครั้งแรก](#รันแอปครั้งแรก)
5. [พัฒนาต่อ](#พัฒนาต่อ)
6. [ทรัพยากรเพิ่มเติม](#ทรัพยากรเพิ่มเติม)
7. [คำแนะนำ](#คำแนะนำ)

---

## ภาพรวม

แอปนี้เป็นร้านค้าออนไลน์ที่ให้:
- **ผู้ใช้ทั่วไป (User):** ดูสินค้า, เพิ่มลงตะกร้า, สั่งซื้อ, และดูประวัติ
- **ผู้ดูแลระบบ (Admin):** เพิ่ม, แก้ไข, และลบสินค้า

เราจะใช้เครื่องมือดังต่อไปนี้:
- **Flutter:** Framework สำหรับสร้างแอป
- **GetX:** ช่วยจัดการสถานะและการนำทางในแอป
- **Firebase:** ระบบ backend สำหรับล็อกอิน, ฐานข้อมูล, และเก็บรูปภาพ



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
