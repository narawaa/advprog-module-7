# BambangShop Publisher App

Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

- Nama : Nashwa Ghania
- NPM : 2306241770
- Kelas : Pemrograman Lanjut - A

---

## About this Project

In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:

1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment

1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable | type | description |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)

- [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
- **STAGE 1: Implement models and repositories**
  - [x] Commit: `Create Subscriber model struct.`
  - [x] Commit: `Create Notification model struct.`
  - [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - [x] Commit: `Implement add function in Subscriber repository.`
  - [x] Commit: `Implement list_all function in Subscriber repository.`
  - [x] Commit: `Implement delete function in Subscriber repository.`
  - [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - [x] Commit: `Create Notification service struct skeleton.`
  - [x] Commit: `Implement subscribe function in Notification service.`
  - [x] Commit: `Implement subscribe function in Notification controller.`
  - [x] Commit: `Implement unsubscribe function in Notification service.`
  - [x] Commit: `Implement unsubscribe function in Notification controller.`
  - [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - [x] Commit: `Implement publish function in Program service and Program controller.`
  - [x] Commit: `Edit Product service methods to call notify after create/delete.`
  - [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. **Apakah kita masih perlu trait dalam kasus BambangShop ini?**

   Dalam Observer, Subscriber biasanya berupa interface (trait dalam Rust) agar berbagai jenis subscriber bisa diimplementasikan dengan cara yang berbeda. Namun, dalam kasus ini, hanya ada satu model Subscriber sehingga penggunaan trait tidak diperlukan. Jika nantinya ada berbagai jenis subscriber dengan perilaku berbeda, maka trait bisa digunakan untuk fleksibilitas yang lebih baik.

2. **Apakah menggunakan Vec cukup atau DashMap diperlukan?**

   Id dalam Program dan url dalam Subscriber harus unik sehingga menggunakan Vec kurang efisien karena perlu pencarian berulang (O(n)) untuk memastikan keunikan. DashMap, yang berbasis HashMap, memungkinkan akses langsung (O(1)) sehingga lebih cepat dalam pencarian, penambahan, dan penghapusan subscriber, terutama jika jumlah data besar.

3. **Apakah kita masih perlu DashMap atau bisa pakai Singleton?**

   Rust memerlukan thread safety, dan DashMap sudah menyediakan concurrent HashMap yang aman diakses dari banyak thread. Jika menggunakan Singleton biasa dengan HashMap, kita perlu menambahkan mekanisme thread safety seperti Mutex<HashMap>, yang bisa menyebabkan bottleneck. Oleh karena itu, penggunaan DashMap tetap lebih baik untuk performa yang optimal.

#### Reflection Publisher-2

1. **Mengapa perlu memisahkan Service dan Repository dari Model?**

   Memisahkan Service dan Repository dari Model agar sesuai dengan prinsip Single Responsibility. Repository bertanggung jawab untuk mengelola akses data (misalnya membaca dan menulis ke database), sedangkan Service menangani logika bisnis. Ini membuat kode lebih terstruktur, mudah dipelihara, dan memungkinkan perubahan dalam penyimpanan data tanpa mengubah logika bisnis.

2. **Apa yang terjadi jika hanya menggunakan Model?**

   Jika semua logika bisnis dan akses data langsung diletakkan dalam Model, maka Model akan menjadi sulit dikelola. Interaksi antar Model seperti Program, Subscriber, dan Notification akan bercampur sehingga menyebabkan kode menjadi kompleks dan sulit diuji. Setiap perubahan dapat berdampak besar pada seluruh sistem, meningkatkan risiko bug dan kesalahan.

3. **Bagaimana Postman membantu pengujian?**

   Postman sangat membantu saya dalam menguji API karena memungkinkan pengiriman request dengan mudah. Fitur seperti Collections untuk menyimpan request dan Environment Variables untuk konfigurasi sangat berguna.

#### Reflection Publisher-3

1. **Observer Pattern yang digunakan.**

   Pola yang digunakan adalah Push Model dimana NotificationService secara otomatis mengirimkan notifikasi ke setiap subscriber segera setelah ada perubahan status produk (misalnya, "CREATED", "DELETED", "PROMOTION"). Data yang dikirim mencakup detail produk, status, dan informasi subscriber.

2. **Keuntungan dan kerugian menggunakan Pull Model.**

   Jika menggunakan Pull Model, subscriber akan secara aktif mengambil data dari publisher saat dibutuhkan, bukan menerima notifikasi langsung. Keuntungannya, subscriber memiliki kontrol lebih besar atas kapan dan bagaimana mereka mengambil data sehingga mengurangi beban sistem saat banyak subscriber. Kelemahannya, subscriber harus terus melakukan polling yang dapat meningkatkan latensi dan beban jaringan karena adanya request yang tidak perlu. Dalam kasus ini, Pull Model akan membuat sistem kurang responsif terhadap perubahan produk.

3. **Dampak jika tidak menggunakan multi-threading dalam proses notifikasi.**

   Tanpa multi-threading, notifikasi akan dikirim secara berurutan kepada setiap subscriber sehingga menyebabkan pemrosesan menjadi lambat terutama jika ada banyak subscriber. Ini dapat menyebabkan bottle-neck di server karena harus menunggu setiap notifikasi selesai sebelum melanjutkan ke yang berikutnya.
