# Namespace and use

## Kljucna rec namespace

Namespace takodje radi 2 stvari:

- Omogucava da vise autora moze da nazove klasu slicno
- Omogucava `autoload`-u da locira ovu klasu

Primer:

```php
<?php
namespace App;

class Nesto {}
```

Primer za koriscenje namespace-a (bez autoload)

Fajl: **primer.php**

```php
namespace Lang\En;

class Student
{
    public function hi() { return "Hello"; }
}
```

Fajl: **primer2.php**

```php
namespace Lang\Es;

class Student
{
    public function hi() { return "Hola"; }
}
```

Fajl **index.php**

```php
<?php
// Ovo radimo jer demonstiramo primarne funkcije namespace-a
// sto ne obuhvata njihov mehanizam autoload-a
require_once "primer.php";
require_once "primer2.php";

// 1. nacin
(new \Lang\En\Student())->hi() // "returns 'Hello'
(new \Lang\Es\Student())->hi() // "returns 'Hola'

// 2. nacin
use Lang\En\Student;
use Lang\Es\Student as EsStudent; // "as" definise alias na studenta
// as <BiloSta>
// ili, drugo studenta mozemo da zovemo sa celom putanjom:
Student::class // EN Student
\Lang\Es\Student::class // ES Student

// loguje: "Hello"
(new Student())->hi() // mislim na ovog definisanog sa "use"
// paznja! ovo ne bi radilo:
(new \Student()) // jer ovde mislimo na nekog tamo "globalnog" studenta
```

Jako je bitno da u samom namespace-u pisemo samo "foldere", a ne i fajl.
U gore navedenom primeru ovaj fajl nam se nalazi u `App/Nesto.php`. Primetimo kako
namespace nije "Nesto", nego samo do foldera.

## Kljucna rec use (iznad klase)

Kada koristimo `use` radimo 2 stvari:

- Ucitavamo "namespace" `\Nesto\Bla`
- "Govorimo" `composer`-u, odnosno njegovom `autoload` sistemu da nam ucita **klasu**/**interfejs**/**trait** `Bla` koji se fizicki nalazi na putanji relativno u projektu na `/Nesto/Bla.php`

Primer:

```php
<?php
use \Nesto\Bla;
```
