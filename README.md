[OOP #3.txt](https://github.com/user-attachments/files/21956642/OOP.3.txt)
Object-Oriented Programming
Basic concept Abstraction, Inheritance & Polymorphism dalam OOP Javascript


1. Abstraction
Sebelumnya kita sudah mempelajari konsep encapsulation, yang secara garis besar menyatakan tentang bagaimana pembungkusan sebuah class yang aman dan datanya terlindungi.
Pada konsep abstraction, berfokus ke cara menyembunyikan detail kompleks dari sebuah sistem dan hanya menampilkan fitur penting pada interface.

Banyak kesamaan antara encapsulation dengan abstraction, yaitu:
* Sama-sama menyembunyikan sesuatu dari user, encapsulation menyembunyikan data user, sementara abstraction menyembunyikan kompleksitas cara kerja interface dari user.
* Dan sama-sama membuat sistem lebih aman, rapi & mudah digunakan.

Misalnya seperti, di dalam sebuah interface (website) memiliki fitur login. 
Maka, user hanya perlu tau bahwa fitur login pada interface adalah untuk memasukkan akun user dan user tidak perlu tau bagaimana sistem internal memproses fitur tersebut.


2. Inheritance
Inheritance (Pewarisan) adalah konsep dimana sebuah class (child/subclass) bisa mewarisi property dan method dari class (parent/superclass).
Dengan menerapkan konsep  inheritance, kita tidak perlu menulis ulang class yang sama untuk membuat banyak class sejenis,
melainkan hanya perlu menurunkan perilaku class utama ke class turunan dengan extends.
Subclass juga bisa menambahkan atau memodifikasi perilaku dari superclass tanpa merusak pewarisan dari superclass-nya.

Tujuan dari Inheritance:
* Reusable → menghindari duplikasi class dan mudah digunakan ulang.
* Strukturnya Jelas → ada hubungan antara class umum (superclass) dengan class khusus (subclass)
* Flexible  → class bisa dipakai dalam berbagai konteks berbeda karena desainnya tidak kaku.
* Extensible →  Subclass bisa ditambahkan property dan method sesuai kebutuhan tanpa banyak perubahan besar.

3. Polymorphism
Polymorphism adalah kemampuan object dari class yang berbeda untuk menyediakan method dengan nama yang sama, tetapi dengan perilaku yang berbeda sesuai dengan class-nya masing masing.

Tujuan Polymorphism:
* Menyederhanakan pemanggilan object, kita hanya perlu satu nama method yang sama pada setiap class, lalu sistem akan menentukan versi dari class mana yang dijalankan.
* Menghilangkan kebutuhan conditional seperti if/else atau switch untuk membedakan role/tipe object.

4. Praktek Konsep OOP
Pada kali ini kita akan mempraktekkan semua konsep OOP yang telah dipelajari, mulai dari encapsulation, abstraction, inheritance hingga polymorphism.


Langkah Pertama: Mendefinisikan Class Induk (Account)
Tujuan: menyiapkan data privat, API publik, dan kontrak umum. (Encapsulation + Abstraction)


* Encapsulation: menyimpan password di private field #password.
  ``` javascript
        class Account {
        #password;

        constructor(username, password) {
          this.username = username;
          this.#password = password;
        }
  ```

* Abstraction: sediakan method publik yang rapi: login(), changePassword(), role(), getNotification(), canAccess().
  ``` javascript
          login(inputPassword) {
          return inputPassword === this.#password
            ? "Login berhasil!"
            : "Password salah!";
        }

        changePassword(oldPw, newPw) {
          if (oldPw !== this.#password) return "Password lama salah!";
          this.#password = newPw;
          return "Password berhasil diganti!";
        }
          role() {
          return "account";
        }
        getNotification() {
          return "🔔 Notifikasi umum";
        }
        canAccess(_feature) {
          return false;
        }
      }
  ```
* Kontrak umum: role/getNotification/canAccess akan di override di subclass.

Langkah Kedua: Turunkan ke Subclass User & Admin
Tujuan: Inheritance (pewarisan perilaku dasar) + Polymorphism (override method yang sama dengan hasil berbeda).

* User override role, getNotification, canAccess (hanya “profile”).
  ``` javascript
        class User extends Account {
        role() {
          return "user";
        }
        getNotification() {
          return `🔔 Selamat Datang User ${this.username}`;
        }
        canAccess(feature) {
          return feature === "profile";
        }
      }
  ```
* Admin override role, getNotification, canAccess (semua fitur).
  ``` javascript
        class Admin extends Account {
        role() {
          return "admin";
        }
        getNotification() {
          return `🔔 Selamat Datang Admin ${this.username}`;
        }
        canAccess(_feature) {
          return true;
        } // admin bisa semua
      }
  ```
  
Langkah Ketiga: Membuat Data atau Instance
Tujuan: menyiapkan 1 admin + 2 user seperti permintaanmu (Syahrul, Hisyam, Syahid).
Mereka disimpan dalam satu variabel array agar pemanggilan seragam (memanfaatkan polymorphism):
  ``` javascript
      const accounts = [
        new Admin("Syahrul", "admin123"),
        new User("Hisyam", "user123"),
        new User("Syahid", "user456"),
      ];
  ```

Langkah Keempat: Uji Notifikasi & Akses
Tujuan: memanggil method yang sama pada objek berbeda tanpa if/else role.
  ``` javascript
      for (const acc of accounts) {
        console.log(`[${acc.role()}] ${acc.username}`);
        console.log("  Notification :", acc.getNotification());
        console.log("  Akses profile?  ", acc.canAccess("profile"));
        console.log("  Akses admin-panel?", acc.canAccess("admin-panel"));
      }
  ```

Ekspektasi di Console (garis besar):
* Admin Syahrul → notifikasi admin; akses profile true; admin-panel true
* User Hisyam/Syahid → notifikasi user; akses profile true; admin-panel false
Hal ini menegaskan “user hanya bisa menjalankan fitur yang diizinkan pemilik sistem (public method/aturan akses), bukan menyentuh sistem internal”.
