Sangat tidak praktis jika kita mengembalikan HTML document secara langsung pada Route dan Controller. Maka dari itu disinilah View berperan untuk mempermudah dalam pembuatan tampilan halaman web HTML. Dengan View kita bisa membedakan lokasi logic aplikasi dan kode tampilan dalam file yang terpisah.

Semua View disimpan di dalam direktori `resources/views`.

**Note**: Jika ingin testing view menggunakan http test (dengan kata lain view tersebut memiliki routing), maka pastikan sebelum `assertSee` dan sejenisnya, lakukan dulu `assertOk`. Karena jika kita salah memasukkan nama viewnya, testingnya akan tetap lolos, padahal seharusnya gagal.

## Blade Templating

Laravel menggunakan template engine yang bernama Blade untuk membuat kode View-nya, jadi tidak seperti kode PHP biasanya.

Blade menggunakan ekstensi `blade.php` jadi misalnya `index.blade.php`.

### Blade Variable

Salah satu keuntungan template dibandingkan kode PHP langsung adalah, kita bisa memaksa programmer untuk memisahkan logic kode program dengan tampilan (template).

Di Blade walaupun kita bisa membuat kode PHP, tapi tidak disarankan menggunakan itu. Cara yang direkomendasikan adalah, kita hanya menampilkan variable di template Blade, lalu mengirim variable tersebut dari luar ketika akan menampilkan templatenya.

Untuk menampilkan variable di Blade template kita bisa gunakan `{{ $name }}`, dimana nanti variable `$name` bisa diambil secara otomatis dari data yang kita kirim ketika menampilkan view Blade nya.

## Nested View Directory

View juga bisa disimpan di dalam direktori lagi di dalam direktori `views`. Hal ini baik ketika kita sudah banyak membuat views dan ingin melakukan management file views nya. Cara untuk mengakses atau menampilkan nested view adalah dengan menggunakan "Dot" notation, misalnya kita punya view di dalam `resources/views/admin/profile.blade.php`, maka cara mengakses atau menampilkannya adalah seperti ini:

```php
return view('admin.profile', $data);
```

## Optimizing View

Secara default Blade template di compile menjadi kode PHP sesuai permintaan atau kebutuhan, misalnya ketika ada request, Laravel akan mengecek apakah hasil compile Blade tempat ada atau tidak, jika ada maka akan menggunakannya, jika tidak maka akan coba melakukan compile. Termasuk Laravel juga akan mendeteksi jika ada perubahan Blade template, maka akan di lakukan compile ulang.

Compile ketika ada request masuk akan ada dampak negatif, walaupun kecil, yaitu performanya menjadi lebih lambat karena harus melakukan compile terlebih dahulu. 

Menurut pemahaman saya pribadi sebenarnya kalau di development prilaku tersebut tidak ada masalah, tetapi ketika di production akan menjadi masalah, walaupun dampaknya kecil, kenapa disebut dampaknya itu kecil, karena ketika di production, compile hanya akan dijalankan 1 kali, ketika request pertama ke view tersebut. Karena pada pertama kali request pasti file view yang telah di compile belum ada. Namun ketika ada request berikutnya ke view yang sama, maka file view hasil compile akan digunakan.

Oleh karena itu ketika nanti menjalankan aplikasi di production, ada baiknya melakukan compile seluruh Blade template terlebih dahulu, agar tidak perlu melakukan compile lagi ketika request pertama kali masuk.

### Compiling View

Untuk meng compile view atau Blade template gunakan perintah:

```bash
php artisan view:cache
```

Perintah diatas cocok sekali dijalankan pada proses deployment, lebih tepatnya dalam proses CD (*Continous Delivery*). Semua hasil compile view akan berada di dalam direktori `storage/framework/views`.

Jika ingin menghapus seluruh file hasil compile, gunakan perintah:

```bash
php artisan view:clear
```

## Test View tanpa Routing

Kadang kita juga ingin membuat view tanpa routing, misal untuk mengirim email. Pada kasus ini kita bisa melakukan test view tanpa harus membuat route terlebih dahulu. Untuk melakukan kita bisa menggunakan method `view()`, contoh:

```php
$this->view('hello', [
	'name' => 'Dian',
])->assertSeeText('Hello Dian');
```