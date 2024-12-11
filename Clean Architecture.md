![Clean Architecture](https://ik.imagekit.io/rezafikkri/clean-architecture.png?updatedAt=1733885279600)

- **Entity**: Representasi dari table di database
- **Uses Cases**: layer Business Logic atau Application Logic (Kode program Aplikasi yang mengelola entity)
- **Interface Adapter**: atau disebut juga UI, ini tergantung aplikasinya, jika itu berbasis terminal, maka ini adalah implementasi terminalnya. Bagian ini istilahnya adalal layer view.
- **Framework & Drivers**: External framework and tools. Layer ini sebenarnya bukan urusan kita, karena sebenarnya kita cuman pakai saja, misalnya kalau di PHP ada seperti framework Laravel, atau PDO driver untuk terhubung ke database.

## Implementasi Clean Architecture

![Clean Architecture Implementation](https://ik.imagekit.io/rezafikkri/clean-architecture-implementation.png?updatedAt=1733886659757)

Ini adalah contoh implementasi clean architecture di pemrograman yang berorientasi objek.

Business Rule dipisah menjadi 2 bagian yaitu:
1. Service: berisi business rule yang berhubungan dengan logic aplikasi
2. Repository: berisi business rule yang berhubungan dengan logic ke database, semacam adapter ke database kita

Jadi penjelasan dari diagram diatas adalah jika pada kasus menampilkan yaitu:
Layer UI (Web, Desktop) akan memanggil layer Service, lalu layer Service akan memanggil layer Repository dan Repository akan mengambil data dari Database, kemudian Repository akan mengubah atau memasukkan data tersebut ke dalam Entity, selanjutnya Repository akan mengembalikan data dalam bentuk Entity ke layer Service, pada layer Service data dalam bentuk entity akan di proses (jika butuh di filter akan di filter, jika butuh di format akan di format, misalnya seperti tanggal dsb), setelah itu Service akan mengembalikan data yang sudah di proses atau di format, ke layer UI untuk ditampilkan kepada user.

> Data yang di kembalikan oleh Service itu tidak lagi dalam bentuk Entity

Untuk kasus menambahkan data yaitu:
Layer UI (Web, Desktop) akan memanggil dan mengirim data ke layer Service, lalu layer Service akan memasukkan atau mengubah data tersebut ke dalam bentuk Entity dan mengirimnya ke layer Repository, kemudian Repository akan membaca data yang dalam bentuk Entity dan memasukkannya ke dalam database.