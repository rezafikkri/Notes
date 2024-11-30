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