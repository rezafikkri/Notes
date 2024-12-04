Dalam pengembangan perangkat lunak ada konsep yang namanya *dependency injection*. Dependecy Injection adalah teknik dimana sebuah object menerima object lain yang dibutuhkan atau istilahnya *dependency*. Dan saat kita membuat object, sering sekali kita membuat object yang butuh object lain.

> Dependency adalah external komponen yang dibutuhkan kode untuk berfungsi dengan baik

## Foo dan Bar

```php
<?php

namespace App\Data;

class Foo
{
    public function foo(): string
    {
        return 'Foo';
    }
}
```

```php
<?php

namespace App\Data;

class Bar
{
    public function __construct(
        private Foo $foo,
    ) {

    }

    public function bar(): string
    {
        return $this->foo->foo() . 'and Bar';
    }
}
```

Dari class `Foo` dan `Bar` kita tahu bahwa `Bar` membutuhkan `Foo`, artinya `Bar` depends on `Foo` atau `Foo` adalah dependency untuk `Bar`.

Dependency Injection berarti kita perlu memasukkan object `Foo` ke dalam `Bar`, sehingga `Bar` bisa menggunakan object `Foo`.

Pada kode `Foo` dan `Bar` kita menggunakan Constructor untuk melakukan injection (memasukkan dependency), sebenarnya caranya tidak hanya menggunakan Constructor, bisa menggunakan Property atau Method (seperti setter method misalnya), namun sangat direkomendasikan menggunakan Constructor agar bisa terlihat jelas dependencies nya dan kita tidak lupa menambahkan dependencies nya, karena ketika membuat object dari class-nya kita akan langsung tahu, bahwa dia membutuhkan object lain.