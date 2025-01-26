Laravel mendukung abstraction untuk management File Storage menggunakan library [Flysystem](https://github.com/thephpleague/flysystem). Dengan menggunakan fitur File Storage ini kita bisa melakukan misalnya, diawal kita simpan file ke Local tempat terinstall aplikasi Laravel kita, tetapi suatu saat misalnya kita ingin ganti, dari yang awalnya ke Local kita ingin ganti ke Amazon S3, itu kita tidak perlu khawatir, kodenya tidak perlu berubah, kita cukup ganti konfigurasinya saja, karena abstraction-nya sudah dibuat oleh Laravel menggunakan library Flysystem.

## Konfigurasi File Storage

Konfigurasi File Storage di Laravel ada di file `config/filesystems.php`. Kita bisa menambahkan banyak konfigurasi file Storage dan nanti ketika kita akan menyimpan file, kita bisa menentukan File Storage mana yang akan digunakan, apakah itu Local, Amazon S3, Google Cloud Storage dan lain-lain.

Di dalam file tersebut kita bisa mengkonfigurasi semua filesystem "disks", setiap disk driver dan lokasi penyimpanan tertentu. Kita bisa meng-konfigurasikan banyak sebanyak yang kita mau dan bahkan kita juga bisa mengkonfigurasikan banyak disk yang menggunakan driver yang sama, sebagai contoh di dalam file konfigurasi tersebut sudah ada beberapa disk, dua diantaranya adalah `local` dan `public`, dimana kedua disk ini menggunakan driver yang sama yaitu `local`, driver `local` ini digunakan untuk berinteraksi dengan file yang disimpan secara local di server di mana aplikasi Laravel berjalan.

## FileSystem

Implementasi setiap File Storage di Laravel adalah menggunakan sebuah interface bernama `FileSystem`. Untuk class implementasinya itu tergantung, apakah drivernya local, S3 dan lain-lain. Dan kita sebenarnya juga tidak perlu membuat class impelmentasinya.

Dan untuk mendapatkan storage atau disk atau lebih tepatnya object dari class implementasi dari interace `FileSystem`, kita bisa menggunakan Facade `Storage::disk(namaDisk)`, namaDisk disini adalah `local` atau `s3` atau `public`.

## Storage Link

Secara default karena file di simpan di directory `storage/app/`, maka file tidak akan bisa oleh publik via web. Karena file dan directory yang bisa diakses oleh publik itu hanya yang terdapat dalam directory `public`, maka dari itu kita harus membuat symbolic link dari `public/storage` ke `storage/app/public`, untuk melakukan ini, kita bisa menggunakan fitur di Laravel bernama Storage Link. Dengan ini file ataupun directory yang terdapat di `storage/app/public` bisa diakses oleh publik via web. Untuk membuat symbolic link kita bisa gunakan perintah:

```bash
php artisan storage:link
```

Kita juga bisa membuat symbolic link sesuai kebutuhan kita di file konfigurasi `filesystems`. Setiap link yang kita tambahkan pada file konfigurasi akan dibuat ketika kita menjalankan perintah `storage:link`, contoh:

```php
'links' => [
	public_path('storage') => storage_path('app/public'),
	public_path('images') => storage_path('app/images'),
],
```

Key dari array `links` adalah lokasi dari link dan valuenya adalah target dari link tersebut. 

Jadi misal pada contoh diatas, directory `public` yang ada di storage_path >> app akan dibuatkan symbolic link di public_path dengan nama `storage`, storage ini adalah sebuah directory karena public yang ada di storage_path >> app juga sebuah directory, sehingga dengan ini semua file yang ada di directory storage_path >> app >> public akan bisa diakses oleh publik via web, begitu juga dengan link `images`.

Lebih lengkap mengenai symbolic link di linux bisa baca https://www.freecodecamp.org/news/symlink-tutorial-in-linux-how-to-create-and-remove-a-symbolic-link/