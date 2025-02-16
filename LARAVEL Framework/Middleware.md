Middleware merupakan cara untuk melakukan inspecting dan filtering terhadap HTTP Request yang masuk ke aplikasi kita. Laravel banyak sekali menggunakan middleware, contohnya melakukan enkripsi cookie, verifikasi csrf, authentication dan lain-lain. Semua middleware yang kita buat biasanya terletak di direktori `app/Http/Middleware`.

![Diagram middleware](https://ik.imagekit.io/rezafikkri/middleware%20diagram.png?updatedAt=1739670310298)<small style="color:gray">Diagram Middleware</small>

**Penjelasan diagram:**
Request yang masuk akan melalui middleware terlebih dahulu, ketika lolos, maka akan diteruskan ke Controller atau routing closure-nya, kalau sudah sukses, maka ketika balik juga bisa memproses middleware lagi, jika memang butuh, kalau tidak butuh tinggal balikkan saja langsung ke usernya.

Selain itu middleware juga bisa lebih dari satu:

![Diagram multiple middleware](https://ik.imagekit.io/rezafikkri/diagram%20multiple%20middleware.png?updatedAt=1739670198503)<small style="color:gray">Diagram Multiple Middleware</small>

**Penjelasan diagram:**
Kegunaan middleware bisa lebih dari satu adalah jika kita ingin membedakan jenis-jenis middlewarenya, misalnya ada middleware khusus untuk enkripsi cookie, authentication dan validasi csrf, jadi middlewarenya dipisah-pisah supaya kodenya tidak terlalu *Spaghetti* (kode yang sulit dipahami karena tidak memiliki struktur yang jelas, kusut seperti *spaghetti*).

## Membuat Middleware

Untuk membuat middleware kita mnggunakan perintah artisan:

```bash
php artisan make:middleware MiddlewareName
```

Middleware mendukung Dependency Injection, jadi kita bisa menambahkan *type-hint* untuk dependency yang kita butuhkan di constructor jika memang mau.

Middleware sebenarnya hanya sebuah class sederhama yang hanya memiliki method `handle(request, closure)` yang akan dipanggil sebelum request masuk ke Controller kita. Jika kita ingin meneruskan request, kita bisa panggil function `closure`-nya, yang mana akan meneruskan request ke middleware selanjutnya, jika itu adalah middleware terakhir maka akan ke Controller atau route closure. Jika tidak, kita bisa mengembalikan Response di middlewarenya atau manipulasi apapun itu bisa kita lakukan.

## Global Middleware

Secara default middleware yang baru kita buat, tidak akan dieksekusi oleh Laravel, kita perlu meregistrasikan middlewarenya terlebih dahulu. Ada beberapa cara untuk meregistrasikan middleware, pertama kita bisa meregistrasikan middleware secara global ke *global middleware stack* (tumpukan middleware global). Global, artinya middleware akan dieksekusi di semua route, ini bisa kita registrasikan di dalam file `bootstrap/app.php`:

```php
use App\Http\Middleware\EnsureTokenIsValid;

->withMiddleware(function (Middleware $middleware) {
	$middleware->append(EnsureTokenIsValid::class);
})
```

Object `$middleware` yang tersedia di `withMiddleware` closure adalah sebuah intance dari `Illuminate\Foundation\Configuration\Middleware` dan bertanggung jawab untuk mengelola middleware yang ditugaskan ke route aplikasi kita.

Jadi intinya dengan menggunakan object tersebut, kita bisa menambahkan atau meregistrasikan middleware yang kita buat ke *global middleware stack*, sehingga nantinya middleware kita akan dieksekusi di semua route.

## Assigning Middleware to Routes

Selain global kita juga bisa registrasikan middleware untuk route tertentu, dimana kita bisa registrasikan satu persatu, atau bisa langsung buat middleware group.

Untuk registrasi satu persatu kita bisa langsung menggunakan middlewarenya pada route atau membuat alias di file `bootstrap/app.php`:

```php
use App\Http\Middleware\EnsureUserIsSubscribed;

->withMiddleware(function (Middleware $middleware) {
	$middleware->alias([
		'subscribed' => EnsureUserIsSubscribed::class
	]);
})
```

Jadi nanti ketika ingin menggunakan middleware, kita bisa menggunakan aliasnya saja, pada contoh diatas yaitu "subscribed".