Laravel menyediakan sebuah class exception yang bisa kita gunakan untuk mempermudah ketika kita ingin membuat error mengikuti HTTP Status Code
Class exception tersebut adalah `Symfony\Component\HttpKernel\Exception\HttpException`
Laravel juga menyediakan helper function untuk membuat exception tersebut hanya dengan menggunakan method `abort(statusCode)`, secara otomatis akan `throw HttpException` dengan status code tersebut.

## HTTP Error Page

Jika kita ingin membuat error page sendiri, kita tidak perlu manual meregistrasikan satu per satu

Laravel akan secara otomatis menggunakan view dengan nama sesuai status code nya, misal jika kita lakukan `abort(400)`, maka Laravel akan menggunakan view `resources/views/errors/400.blade.php`, jika tidak ada, maka akan menggunakan `4xx.blade.php`

Untuk mendapat object exception nya, kita bisa gunakan variable `$exception` di template nya
