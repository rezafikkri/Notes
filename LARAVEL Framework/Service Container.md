Sebelumnya kita sudah mencoba melakukan dependency injection secara manual di [[Dependency Injection]]. Laravel memiliki fitur dependency injection secara otomatis dan ini wajib dikuasasi agar lebih mudah membuat aplikasi menggunakan Laravel. Di Laravel fitur ini bernama **Service Container**, dimana Service Container ini merupakan fitur yang digunakan untuk management semua dependencies yang ada di Laravel dan juga dependency injection.

## Application Class

Service Container di Laravel direpresentasikan dalam class bernama Application. Application adalah Container atau Manager dari dependency injectionnya. Kita tidak perlu membuat class Application secara manual, karena semua sudah dilakukan secara otomatis oleh Laravel. Di semua project Laravel hampir semua bagian terdapat property `$app` yang merupakan instance dari Application.

## Membuat Dependency

Dengan menggunakan Service Container kita tidak perlu lagi membuat object secara manual menggunakan kata kunci `new`. Kita bisa menggunakan function `make(key)` yang terdapat di class Application untuk membuat dependency secara otomatis. Saat kita menggunakan `make(key)` , object akan selalu dibuat baru, jadi harap hati-hati ketika menggunakannya, karena dia bukan menggunakan object sama.

```php
$foo = $this->app->make(Foo::class); // new Foo
$foo2 = $this->app->make(Foo::class); // new Foo
```

## Mengubah Cara Membuat Dependency

Saat kita menggunakan function `make(key)`, secara otomatis Laravel akan membuat objectnya, namun kadang kita ingin menentukan cara pembuatan objectnya. Misalnya pada object yang kompleks, ada constructor nya ada banyak parameternya.

Pada kasus seperti ini kita bisa menggunakan method `bind(key, closure)`, kita cukup returnkan data yang kita inginkan pada function `closure`nya. Saat kita menggunakan `make(key)` untuk mengambil dependencynya, secara otomatis function `closure` akan dipanggil. Perlu diingat juga bahwa setiap kita memanggil `make(key)`, maka function `closure`nya akan selalu dipanggil, jadi bukan menggunakan object yang sama.

```php
$this->app->bind(Person::class, function () {
	return new Person('Reza', 'Sariful Fikri');
});

$person = $this->app->make(Person::class); // closure() // new Person('Reza', 'Sariful Fikri')
$person2 = $this->app->make(Person::class); // closure() // new Person('Reza', 'Sariful Fikri')
```

Dengan kita menggunakan `bind(key, closure)` seperti diatas, kita memberitahu Laravel bahwa jika ada yang ingin membuat object Person, maka jalankan function `closure` tersebut.

## Singleton

Sebelumnya ketika kita menggunakan `make(key)`, maka secara default Laravel akan membuat object baru, atau jika menggunakan `bind(key, closure)`, function closure akan selalu dipanggil.

Kadang ada kebutuhan kita membuat object singleton, yaitu satu object saja dan ketika butuh kita cukup menggunakan object yang sama.

Pada kasus ini kita bisa menggunakan function `singleton(key, closure)`, maka secara otomatis ketika kita menggunakan `make(key)`, maka object hanya dibuat di awal, selanjutnya object yang akan digunakan terus menerus ketika kita memanggil `make(key)`.

```php
$this->app->singleton(Person::class, function () {
	return new Person('Reza', 'Sariful Fikri');
});

$person = $this->app->make(Person::class); // new Person('Reza', 'Sariful Fikri') if not exist
$person2 = $this->app->make(Person::class); // return existing
```

## Instance

Selain menggunakan function `singleton(key, closure)`, untuk membuat singleton object, kita juga bisa menggunakan object yang sudah ada, dengan cara menggunakan function `instance(key, object)`. Ketika menggunakan `make(key)`, instance object tersebut akan selalu dikembalikan.

```php
$person = new Person('Reza', 'Sariful Fikri');
$this->app->instance(Person::class, $person);

$person1 = $this->app->make(Person::class); // $person
$person2 = $this->app->make(Person::class); // $person
```

## Dependency Injection

Sekarang kita tahu bagaimana cara membuat dependency dan mendapatkan dependency di Laravel. Lalu bagaimana cara melakukan dependency injection? secara default jika kita membuat object menggunakan `make(key)`, lalu Laravel mendeteksi terdapat Constructor, maka Laravel akan mencoba menggunakan dependency yang sesuai yang dibutuhkan di constructor tersebut.

Jika tipe yang ada di Constructor itu adalah class, maka Laravel akan secara otomatis meng-inject object dari class tersebut, tetapi jika tipe yang ada di Constructor adalah selain class, misalnya tipe data primitive seperti `string`, `int`, dll atau sebuah `interface`, maka akan terjadi error, karena Laravel tidak mengerti dependency yang dibutuhkan oleh Constructor. Karena itu kita butuh melakukan binding menggunakan `bind(key, closure)`, untuk memberi tahu Laravel bagaimana cara membuat object dari class tersebut.

```php
$this->app->singleton(Foo::class, function () {
	return new Foo;
});

$foo = $this->app->make(Foo::class);
$bar = $this->app->make(Bar::class);
```

## Dependency Injection di Closure

Pada function closure kita juga bisa menggunakan parameter `$app` untuk mengambil object yang sudah ada di Service Container. Kadang ini mempermudah kita ketika ingin membuat object yang kompleks.

```php
$this->app->singleton(Foo::class, function () {
	return new Foo;
});

$this->app->singleton(Bar::class, function (Application $app) {
	return new Bar($app->make(Foo::class));
});

$foo = $this->app->make(Foo::class);
$bar1 = $this->app->make(Bar::class);
$bar2 = $this->app->make(Bar::class);
```

## Binding Interface ke Class

Dalam prakter pengembangan perangkat lunak, **hal yang bagus ketika membuat sebuah class yang berhubungan dengan logic, adalah membuat interface sebagai kontraknya**. Agar nantinya implementasi dari interface bisa lebih dari 1, jadi tidak perlu mengubah-ubah class yang sama. 

Alasan lainnya adalah agar kita bisa dengan mudah menggunakan class-class tersebut, karena walaupun core logic didalam classnya berbeda, tetapi cara menggunakannya sama, karena menerapkan interface yang sama.

Atau alasan yang lain adalah misalnya kita membuat library Export, yang mana bisa meng export ke PDF dan Spreadsheet, agar supaya mudah untuk mengelola kodenya, kita pisah masing-masing ke dalam classnya tersediri, ada class Export, ExportPDF dan ExportSpreadsheet, dimana Export PDF dan ExportSpreadsheet akan digunakan oleh Export. Didalam class Export tentunya kita akan menggunakan Type Hinting, untuk memastikan bahwa object yang dimasukkan adalah benar. Mungkin jika target exportnya hanya 2, kita bisa menggunakan fitur `union`, tetapi bagaimana jika kedepannya ada export-export yang lain? atau bagaimana jika ada orang lain yang ingin berkontribusi untuk membuat tipe export yang lain? pastinya akan sangat merepotkan dan susah untuk mantaince-nya (mengelolah), apalagi jika librarynya sudah besar, karena harus menambahkan class baru di `union` type nya, dan juga jika itu adalah orang lain, kita tidak bisa memastikan bahwa class yang dibuatnya betul sesuai yang dibutuhkan (misalnya method yang kita butuhkan ada atau tidak), atau misalnya kita butuh untuk mengubah nama classnya, atau bahkan menghapus salah satu tipe export, dsb.

Maka dari itu alangkah baiknya kita membuat interface (kontrak) untuk class Export PDF, Export Spreadsheet dll. Sehingga pada class Export, nantinya yang kita jadikan type hinting nya adalah interface tersebut. Sedangkan implementasi dari interface tersebut bisa bermacam-macam.

