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

Object `$middleware` yang tersedia di `withMiddleware` closure adalah sebuah intance dari `Illuminate\Foundation\Configuration\Middleware` dan bertanggung jawab untuk mengelola middleware yang ditugaskan ke route aplikasi kita. Jadi intinya dengan menggunakan object tersebut, kita bisa menambahkan atau meregistrasikan middleware yang kita buat ke *global middleware stack*, sehingga nantinya middleware kita akan dieksekusi di semua route.

Pada contoh diatas digunakan method `append`, yang berarti middleware akan ditambahkan pada bagian akhir dari daftar global middleware, namun jika kita ingin menambahkan middleware pada bagian awal dari daftar global middleware kita bisa menggunakan method `prepend`. Hal ini berkaitan dengan urutan eksekusi dari si middleware tersebut, selain itu, urutan kita meregistrasikan middleware-nya juga menentukan urutan eksekusi middleware.

## Assigning Middleware to Routes

Selain global kita juga bisa registrasikan middleware untuk route tertentu, dimana kita bisa registrasikan satu persatu, atau bisa langsung buat middleware group.

Untuk registrasi satu persatu kita bisa langsung menggunakan middlewarenya pada route:

```php
use App\Http\Middleware\EnsureTokenIsValid;

Route::get('/profile', function () {
	// ...
})->middleware(EnsureTokenIsValid::class);
```

atau membuat alias di file `bootstrap/app.php`, lalu ketika ingin menggunakan middlewarenya kita hanya perlu memasukkan alias nya saja, tanpa perlu memasukkan nama classnya (*ini bermanfaat jika nama class dari middleware kita cukup panjang*):

```php
use App\Http\Middleware\EnsureUserIsSubscribed;

->withMiddleware(function (Middleware $middleware) {
	$middleware->alias([
		'subscribed' => EnsureUserIsSubscribed::class
	]);
})
```

```php
Route::get('/profile', function () {
	// ...
})->middleware('subscribed');
```

## Middleware Group

Kadang kita ingin menggabungkan beberapa middleware dalam satu group, sehingga ketika membutuhkannya, kita cukup sebutkan nama groupnya saja. Kita bisa melakukan ini dengan membuat middleware group menggunakan method `appendToGroup` (*menambahkan group pada bagian akhir dari daftar group*) atau method `prependToGroup` (*menambahkan group pada bagian awal dari daftar group*) di dalam file `bootstrap/app.php`:

```php
use App\Http\Middleware\First;
use App\Http\Middleware\Second;

->withMiddleware(function (Middleware $middleware) {
	$middleware->appendToGroup('group-name', [
		First::class,
		Second::class,
	]);

	$middleware->prependToGroup('group-name', [
		First::class,
		Second::class,
	]);
})
```

Secara default di Laravel terdapat 2 middleware group, yaitu `web` dan `api`. Group middleware `web` akan secara otomatis diterapkan ke file `routes/web.php`, sedangkan group `api` diterapkan ke `routes/api.php` dan penerapakan tersebut dilakukan oleh file `bootstrap/app.php`. ^f0264c
## Middleware Parameter

Middleware bisa juga menerima parameter tambahan. Sebagai contoh misalnya pada aplikasi kita ada dua macam role: admin dan operator, kita ingin mengecek apakah user yang sudah login memiliki role yang diperbolehkan untuk menggunakan atau mengakses fitur tertentu, misalnya ada beberapa fitur di aplikasi kita yang hanya user dengan role "admin" yang bisa menggunakannya, maka dari itu kita bisa membuat middleware kita menerima role name sebagai argument tambahan. Untuk membuat itu kita bisa mendefinisikan paremeter tambahan setelah parameter `$next` di method `handle`.

Nilai dari Middleware parameter ditentukan atau dimasukkan ketika mendefinisikan route (*atau lebih tepatnya ketika kita menggunakan atau assign si middleware-nya*) dengan memisahkan antara nama middleware dan parameter dengan `:` (titik dua), jika ada lebih dari satu parameter, gunakan koma sebagai pemisah antar prameter:

```php
Route::put('/post/{id}', function (string $id) {
	// ...
})->middleware(EnsureUserHasRole::class.':admin');
```

## Exclude Middleware

Ketika assign atau menerapkan middleware ke sebuah routes group, kita mungkin kadang-kadang butuh untuk mencegah middleware diterapkan ke salah satu route yang ada di dalam group. Untuk melakukan itu di Laravel kita bisa menggunakan method `withoutMiddleware`:

```php
use App\Http\Middleware\EnsureTokenIsValid;

Route::middleware([EnsureTokenIsValid::class])->group(function () {
	Route::get('/', function () {
		// ...
	});
	
	Route::get('/profile', function () {
		// ...
	})->withoutMiddleware([EnsureTokenIsValid::class]);
});
```


