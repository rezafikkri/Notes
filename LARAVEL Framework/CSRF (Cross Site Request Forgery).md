CSRF merupakan salah satu security exploit yang biasanya dilakukan untuk mengirim action ke aplikasi web kita dari web orang lain, alias cross site.

Hal ini sangat berbahaya, terutama misal ketika action yang dipanggil adalah action yang sangat berpotensi merugikan, misalnya mengirim uang atau mengganti email dari suatu akun.

Adapun cara kerja dari CSRF attack adalah, misalnya di website kita ada route `/user/email` yang menerima POST request untuk mengubah alamat email dari user website kita yang telah login dan route tersebut menerima input email pada body request nya.

Tanpa proteksi CSRF, sebuah website berbahaya akan bisa membuat HTML Form dengan target action-nya adalah route `/user/email` yang ada di website kita dan mengirim alamat email dari user jahat yang memiliki website berbahaya tersebut:

```html
<form action="https://your-application.com/user/email" method="POST">
	<input type="email" value="malicious-email@example.com">
</form>

<script>
	document.forms[0].submit();
</script>
```

Jika website berbahanya tersebut secara otomatis mengirim form ketika halaman di load, maka user jahat tersebut hanya perlu memancing user dari website kita yang telah login yang tidak curiga untuk mengunjungi website mereka dan email address dari user jahat tersebut akan menggantikan email address dari user website kita.

Ini sangat berbahaya, karena dengan mengganti email address dari user website kita, user jahat tersebut nantinya akan bisa mengganti password dari akun yang telah di ganti email addressnya dan login dengan akun tersebut, akhirnya nanti si user jahat bisa melakukan berbagai hal, misalnya mengirim uang, dsb.

## Cara Melindungi dari CSRF attack

Salah satu cara untuk melindungi dari CSRF adalah dengan mewajibkan token ketika melakukan aksi seperti POST ke aplikasi kita misalnya, atau aksi-aksi lain yang melakukan perubahan. Jadi token tersebut digunakan untuk memverifikasi apakah yang melakukan aksi perubahan tersebut adalah benar user yang telah login atau ter-autentikasi.

Caranya sangat sederhana, kita cukup tambahkan input berupa token yang hanya diketahui oleh aplikasi kita dan ketika disubmit menggunakan post, token tersebut dikirim dari form HTML ke aplikasi kita. Jika token valid, maka kita tahu bahwa itu adalah aksi dari web kita sendiri dan yang melakukannya adalah user yang telah ter-autentikasi, jika tidak valid maka kita akan reject request tersebut.

## CSRF Token

Untuk membuat token, Laravel sudah menyediakan helper function bernama `csrf_token()`yang digunakan untuk mendapatkan token session user, jadi nanti tokennya akan berbeda tergantung user atau sessionnya apa.

Setiap kita mengakses website di Laravel, Laravel akan menjalankan session dan akan menyimpan CSRF token di dalam session dan token akan diubah setiap kali session di buat ulang, misalnya ketika melakukan login logout atau ketika sessionnya expired.

Jika kita ingin melakukan POST request, maka kita wajib mengirimkan token tersebut di input, kalau tidak maka request akan otomatis ditolak. Laravel akan mengecek token melalui input name `_token`.

```html
<form method="POST" action="/profile">
	<input type="hidden" name="_token" value="{{ csrf_token() }}" />
</form>
```

Dan yang mengecek csrf token apakah valid atau tidak, di kirim atau tidak adalah middleware `Illuminate\Foundation\Http\Middleware\ValidateCsrfToken`, yang mana middleware ini secara default sudah ada dalam [[Middleware#^f0264c|group middleware]] `web`.

## Ajax

Selain menggunakan input name `_token` untuk mengirimkan csrf tokennya, kita juga bisa menggunakan header `X-CSRF-TOKEN` untuk mengirimkan csrf tokennya jika menggunakan ajax.