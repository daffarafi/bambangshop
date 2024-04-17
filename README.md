# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

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
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
   
    Jawab: 

    Dalam Observer pattern, Subscriber biasanya berperan sebagai sebuah interface atau trait untuk mendefinisikan kontrak yang harus dipatuhi oleh implementasi dari subscriber. Namun, dalam kasus BambangShop, di mana fungsionalitas subscriber relatif sederhana dan seragam (hanya menyimpan URL dan nama), menggunakan sebuah struct Model tunggal untuk Subscriber mungkin sudah cukup. Interface atau trait menjadi lebih berguna ketika terdapat beberapa jenis observer dengan fungsionalitas yang berbeda-beda. Oleh karena itu, dalam kasus ini, sebuah struct single model untuk Subscriber mungkin sudah cukup tanpa perlu menggunakan interface atau trait.

1. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    Jawab:
    
    Baik id di Program maupun url di Subscriber dimaksudkan untuk menjadi identifier unik. Meskipun sebuah Vec (list) bisa digunakan untuk menyimpan subscriber, penggunaan DashMap (map/dictionary) seperti yang digunakan dalam BambangShop lebih efisien untuk operasi penambahan, penghapusan, dan pencarian berdasarkan id. DashMap memungkinkan akses langsung ke elemen berdasarkan id mereka, sehingga lebih baik dibandingkan dengan melakukan iterasi melalui sebuah list. Oleh karena itu, dalam kasus ini, penggunaan DashMap diperlukan untuk memastikan program yang efisien.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    Jawab:
    
    Dalam Rust, memastikan keamanan thread sangat penting, terutama saat berurusan dengan state yang dapat diakses secara bersamaan dan dapat diubah seperti variabel statis SUBSCRIBERS. Meskipun menerapkan pola Singleton bisa memastikan bahwa hanya satu instance dari SUBSCRIBERS yang ada, namun hal itu tidak secara otomatis menjamin keamanan thread. DashMap menyediakan dukungan bawaan untuk akses konkuren, memastikan keamanan thread tanpa usaha tambahan. Oleh karena itu, dalam kasus ini, menggunakan DashMap untuk SUBSCRIBERS diperlukan untuk menjaga keamanan thread dan lebih disukai daripada hanya mengandalkan pola Singleton saja.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    Jawab:

    Meskipun dalam pola Model-View-Controller (MVC) Model mencakup penyimpanan data dan logika bisnis, terpisahnya "Service" dan "Repository" dari Model tetap diperlukan untuk memisahkan tanggung jawab dan meningkatkan kohesi dan fleksibilitas sistem. Pemisahan tanggung jawab ini memungkinkan untuk lebih mudah mengelola dan mengubah setiap bagian dari sistem secara independen. "Service" bertanggung jawab untuk menangani logika bisnis, mengelola interaksi antara berbagai model, dan menjalankan operasi yang kompleks atau terkait dengan bisnis, sementara "Repository" bertanggung jawab untuk mengakses dan memanipulasi data secara langsung dari penyimpanan. Dengan memisahkan "Service" dan "Repository" dari Model, kita dapat mencapai prinsip Single Responsibility Principle (SRP) yang mendorong setiap komponen untuk memiliki satu dan hanya satu alasan untuk berubah.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    Jawab:

    Jika kita hanya menggunakan Model tanpa "Service" dan "Repository", kompleksitas kode akan meningkat karena semua logika bisnis dan operasi data akan ditempatkan di dalam Model itu sendiri. Hal ini dapat menyebabkan Model menjadi terlalu besar dan sulit untuk dipelihara dan diuji. Interaksi antara setiap model juga akan menjadi lebih kompleks karena semua logika bisnis dan akses data akan dikelola di dalam setiap model, menyebabkan redundansi dan potensial terjadinya cacat dalam desain.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    Jawab:

    Postman adalah alat untuk menguji dan memvalidasi API yang dibangun dalam proyek. Dengan Postman, Kita dapat dengan mudah membuat dan mengirimkan permintaan HTTP ke endpoint API dan memeriksa respons yang diterima. Fitur-fitur yang berguna dalam Postman termasuk:
    - Collections: Untuk mengatur permintaan HTTP ke dalam kelompok yang terkait, memudahkan pengujian dan dokumentasi.
    - Environment Variables: Untuk menyimpan variabel yang dapat digunakan di seluruh permintaan, memudahkan pengujian dengan berbagai lingkungan (seperti pengembangan, produksi, dll.).
    - Tests: Untuk menulis skrip tes dalam JavaScript untuk memeriksa respons yang diterima, memvalidasi data, dan mengambil tindakan berdasarkan hasil tes.
    Monitoring: Untuk memantau kesehatan API Anda dengan memantau kinerja dan latensi permintaan.
    - Documentation: Untuk membuat dokumentasi API yang lengkap dan mudah dimengerti.

#### Reflection Publisher-3
