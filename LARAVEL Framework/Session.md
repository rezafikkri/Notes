Seperti kita ketahui, bahwa HTTP itu stateless, artinya setiap request dilakukan secara independent, dan tidak ada hubungannya dengan request lain,

Session digunakan untuk menyimpan data yang bisa digunakan antar request, dan biasanya session disimpan di persistent storage.

Laravel menyediakan abstraction layer untuk kita mengelola session, sehingga kita tidak perlu menggunakan PHP session lagi.

Semua konfigurasi Laravel session terdapat di file config/session.php

## Mengambil Session

Untuk mendapatkan object Session, ada banyak caranya, kita bisa menggunakan method `session()` dari object Request, atau bisa menggunakan helper method `session()`, atau bisa menggunakan facade Session.
