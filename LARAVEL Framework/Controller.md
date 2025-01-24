Membuat Route memang mudah, tetapi jika kita harus menyimpan semua logic aplikasi kita di closure function Route, lama-lama akan sulit untuk dilakukan dan terutama sulit untuk dikelola.

Di Laravel kita bisa menggunakan Controller sebagai tempat menyimpan logic dari Route, sehingga tidak perlu kita lakukan lagi di Route.

Controller direpresentasikan sebagai class dan penamaan class nya selalu diakhiri dengan `Controller`, misal `UserController`, `ProductController` dan sebagainya.

Untuk membuat Controller kita bisa membuatnya di namespace `App\Http\Controllers`, agar lebih mudah dalam mebuat Controller kita bisa menggunakan perintah artisan:

```bash
php artisan make:controller NamaController
```

## Membuat Method di Controller

Sebagai pengganti close function di Route, kita bisa membuat function di Controller, atau lebih tepatnya disebut method dan meletakkan semua logic dari web kita di method Controller.

Selanjutnya kita bisa meregistrasikan method Controller tersebut ke Route atau dengan kata lain, membuat route yang mengarah ke method Controller, dengan cara mengganti parameter closure di Route dengan array yang berisi class Controller dan juga nama methodnya.

## Dependency Injection

Controller mendukung dependency injection, karena pembuatan object Controller sebenarnya dilakukan Service Container.

Dengan demikian kita bisa menambahkan dependency yang dibutuhkan di Constructor Controller dan secara otomatis Laravel akan mengambil dependency tersebut dari Service Container dan memasukkannya kedalam Controller.

> **Note:** Menurut pemahaman pribadi, sebenarnya ketika kita melakukan binding dependency kita ke container, itu kita bukan melakukan dependency injection, tetapi kita hanya sebatas memberi tahu Laravel bagaimana cara membuat object dependency tersebut, karena mungkin misalnya dependency tersebut cukup kompleks, lebih detail lihat [[Service Container]]. Dan kegiatan melakukan dependency injection itu di lakukan oleh Laravel sendiri menggunakan service container, kita tinggal menggunakannya saja.

Meskipun dependency injection best practicenya adalah menggunakan constructor, bukan berarti kita tidak bisa melakukannya di method lain, kita bisa melakukannya, tetapi perlu diingat, automatic dependency injection ini hanya bisa dilakukan di fitur-fitur bawaan Laravel saja, seperti di Controller, Middleware, dll. Contoh:

```php
public function goodMorning(GoodMorningService $goodMorningService, string $name): string
{
	return $goodMorningService->goodMorning($name);
}
```

Yang perlu menjadi catatan adalah, jika pada Controller method kita juga butuh menerima input dari route parameter, maka pastikan posisi dari parameter-nya adalah setelah dependency lain, seperti pada contoh di atas, walaupun tidak error, tapi sebaiknya kita mengikuti saran yang ada di dokumentasi Laravel.

