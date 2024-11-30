---
cssclasses:
  - code-nowrap
---
Logging adalah aksi menambahkan informasi ke dalam log file, log file berisi informasi kejadian dari sebuah sistem, biasanya didalam log file terdapat waktu kejadian (mengikuti waktu ketika log dibuat) dan pesan kejadian. Jadi log adalah si informasinya, log file adalah tempat menyimpan data log, dan logging adalah aksi menyimpan datanya.

Logging tidak hanya untuk menampilkan informasi, tetapi juga berguna ketika melakukan debugging ketika terjadi masalah pada aplikasi kita.
![Diagram Logging](https://ik.imagekit.io/rezafikkri/logging%20diagram.png?updatedAt=1732933646490)
<small style="color: gray">Diagram logging</small>

## Monolog

Nama Logger best practicenya adalah nama class dimana Logger itu dibuat, karena dengan ini kita bisa tahu lokasi aktifitas loggingnya ada dimana, hanya dengan melihat nama loggernya. Contoh:

```php
<?php

use Monolog\Logger;
use PHPUnit\Framework\Attributes\Test;
use PHPUnit\Framework\TestCase;

class LoggerTest extends TestCase
{
    #[Test]
    public function logger(): void
    {
        // Logger adalah class yang digunakan untuk melakukan logging
        $log = new Logger(LoggerTest::class);
        $this->assertNotNull($log);
    }
}
```

### Logger

Adalah class yang digunakan untuk melakukan logging.

### Handler

Adalah object yang bertugas mengirim aktivitas _log event_ (log kejadian/peristiwa) yang kita kirim ke Logger ke target yang dituju (file, database, dll). Intinya handler ini menyimpan/mengirim log kita ke target yang dituju. Secara default tidak ada handler sama sekali ketika kita membuat Logger, yang berarti semua aktifitas logging yang kita kirimkan ke dalam Logger akan hilang, karena tidak akan disimpan/dikirim kemanapun.

Jika ingin membuat handler sendiri kita bisa meng-implementasikan handler interface. Tetapi di Monolog sebenarnya sudah ada handler yang bisa kita gunakan, sehingga mungkin kita tidak perlu lagi membuat handler sendiri.

Salah satu handler yang paling sering digunakan adalah **Stream Handler**, yang mana ini digunakan untuk mengirim _log event_ (log kejadian) ke dalam target _Stream_, contohnya adalah file dan console.

### Level

Saat mengirim log event, kita bisa menentukan level dari log event tersebut. Cara kerja level itu bertingkat dari yang paling rendah ke paling tinggi.

Biasanya level digunakan untuk menentukan jenis log event, misalnya jika log event berupa informasi, maka kita gunakan level info, jika berupa masalah kita gunakan level error (misalnya ketika terjadi error dan kita ingin melakukan log, maka gunakan level error), jika kita ingin log informasi yang sangat detail untuk membantu proses debugging, maka gunakan level debug.

Salah satu kelebihan **StreamHandler** adalah, dia bisa menentukan mulai dari level mana log event harus dikirim, ini berguna salah satunya jika kita ingin memisahkan lognya, misalnya untuk di Console itu kita tampilkan semua lognya dan untuk di file itu hanya log yang isinya error saja, contoh:

```php
...
#[Test]
public function level(): void
{
	$log = new Logger(self::class);
    $log->pushHandler(new StreamHandler('php://stdout'));// diconsole kirimkan semua level log
    $log->pushHandler(new StreamHandler(__DIR__ . '/../app.log'));// di file app.log kirimkan semua level log
    $log->pushHandler(new StreamHandler(__DIR__ . '/../error.log', Level::Error)); // sedangkan di file
    // error.log kirimkan hanya log mulai dari level Error ke atas, sedangkan
    // yang dibawah level Error tidak akan dikirimkan ke file error.log

    $log->debug('Debug Log');
    $log->info('Info Log');
    $log->notice('Notice Log');
    $log->warning('Warning Log');
    $log->error('Error Log');
    $log->critical('Critical Log');
    $log->alert('Alert Log');
    $log->emergency('Emergency Log');

    $this->assertNotNull($log);
}
...
```

Secara default **StreamHandler** mengirim mulai dari level Debug. Ini cocok banget jika di production, misalnya lagi ada masalah level yang dikeluarkan level Debug keatas, lagi enggak ada masalah level yang dikeluarkan level Error keatas saja.

> Dan perlu diingat bahwa cara kerja level itu bertingkat, kita tidak bisa memilih salah satu, ataupun membatasi, misalnya hanya ingin mengirim log level Debug sampai Notice saja, itu tidak bisa. Yang bisa itu, pilih satu level dan hanya level itu dan di atasnya saja yang akan di kirimkan. Misal kita pilih level Info, maka hanya level Info dan level diatasnya saja yang akan di tampilkan.

### Context

Selain mengirimkan log event berupa string, kita juga bisa manmbahkan informasi lainnya berupa array, ketika melakukan logging. Informasi tambahan ini dinamankan context. Biasanya data yang dimasukkan kedalam context ini digunakan untuk tujuan _tracking_. Contoh:

```php
<?php

...
$log->info('Success Login', ['username' => 'rezafikkri']);
...
```

### Processor

Processor adalah cara lain jika kita ingin menambahkan informasi tambahan ke log event. Jika informasi Context harus kita kirim setiap kali melakukan logging, pada Processor kita bisa membuat class Processor yang akan dieksekusi setiap kali log event dikirim. Atau dengan kata lain, dengan Processor kita hanya perlu membuat informasi tambahannya satu kali, dan nantinya bisa digunakan berkali-kali, berbeda dengan Context, yang mana setiap kali melakukan logging, kita perlu menambahkan berkali-kali secara manual informasi tambahannya.

Untuk membuat Processor kita bisa langsung menggunakan Callable atau membuat class turunan dari ProcessorInterface.

> Tips: Context digunakan jika kita perlu menambahkan informasi tambahan sesifik per-log event, sedangkan Processor digunakan jika kita butuh menambahkan informasi tambahan yang general.

Contoh:

```php
<?php
...
$log->pushProcessor(function ($record) {
    $record->extra['app_version'] = ['v1.0.0'];
    return $record;
});
...
```

Sama seperti Handler Processor juga sudah banyak menyediakan class implementasi dari ProcessorInterface, yang bisa kita gunakan sesuai kebutuhan kita. Processor juga sama seperti Handler, bisa ditambahkan lebih dari satu.

Jika seperti contoh di atas, maka artinya Processor akan didaftarkan pada Logger. Selain didaftarkan pada Logger, Processor juga bisa didaftarkan pada spesifik Handler, sehingga nantinya kita bisa menambahkan data extra yang berbeda pada setiap Handler. Berbeda dengan contoh diatas, yang mana Processor akan digunakan oleh semua Handler pada Logger tersebut.

Contoh:

```php
<?php
...
$handler = new StreamHandler('php://stderr');
$handler->pushProcessor(new GitProcessor());
$log->pushHandler($handler);
...
```

### Resettable Interface

Beberapa Handler dan Processor ada yang menyimpan datanya di memory. Jika proses object Logger mengikuti alur hidup Web Request, hal itu aman-aman saja, karena setelah Web Request selesai semua data akan dihapus dari memory.

Namun jika kita ternyata melakukan pekerjaan yang lama, misalnya long running job, atau kita me-logging banyak data, maka sangat disarankan secara _regular_ (teratur) melakukan reset Handler dan Processor, agar datanya dihapus dari memory. Ini dilakukan untuk menghindari _memory leaks_.

Contoh:

```php
<?php
...
for ($i = 0; $i < 10000; $i++) {
    $log->info("Loop $i");
    // setiap 100 kali mengirimkan log event
    if ($i % 100 == 0) {
        $log->reset();
    }
}
...
```

### Formatter

Saat Handler mengirim log event ke tujuan (misal file atau console), Handler akan melakukan proses format log event terlebih dahulu. Setiap Handler biasanya memiliki default Formatter, contohnya StreamHandler menggunakan LineFormatter. Jika ingin membuat Formatter sendiri kita bisa membuat class turunan dari FormatterInterface.

Monolog juga sudah menyediakan banyak implementasi FormatterInterface, kita bisa menggunakannya sesuai kebutuhan kita.

> Formatter tidak bisa lebih dari 1 seperti Processor, jika kita set dua kali, maka dia akan menimpa/mengganti Formatter sebelumnya.

Contoh:

```php
<?php
...
$handler = new StreamHandler('php://stderr');
$handler->setFormatter(new JsonFormatter());

$log->pushHandler($handler);
...
```

### Rotating File Handler

Adalah class turunan dari StreamHandler, tetapi khusus untuk mengirim log event ke file dan juga perbedaanya dengan StreamHandler adalah RotatingFileHandler bisa secara otomatis membuat file baru setiap hari, sehinga semua log tidak akan disimpan di dalam satu file. Ini sangat bermanfaat, karena kadang semakin lama, ukuran file akan semakin besar, sehingga nantinya akan susah untuk membaca file dan mengambil data yang ada di dalamnya. Serta juga dengan RoratingFileHandler memudahkan kita jika ingin menghapus log lama yang sudah tidak digunakan lagi.

Dengan menggunakan RotatingFileHandler kita bisa menyetel, seberapa banyak file yang mau kita _keep_ atau simpan, misalnya kita set 10, maka artinya hanya bisa menyimpan file 10 hari terakhir, jika nantinya sudah di hari yang ke-11, maka file yang paling lama itu akan dihapus. Intinya dia akan mempertahankan file 10 hari terakhir, sisanya akan otomatis dihapus.