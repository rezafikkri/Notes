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