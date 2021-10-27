# This, self, static and parent

PHP ima sledece kljucne reci za referenicanje **instanci**

- `this`
- `self`
- `static`
- `parent`

Sve one se koriste unutar definicije **klase** i omogucavaju nam da referenciramo instancu te klase u zavisnosti od toga sta nam treba.

## This

### Uvod

```php
<?php
class Nesto
{
    public function test()
    {
        var_dump($this);
    }
}
```

Kada interpreter obradjuje klasu "Nesto" u tom trenutku ne znamo sta ce biti this, klase samo sadrze informacije o **metodama**, a podaci, odnosno polja se uvek vezuju za **instancu**.

Blok, odnosno **body** funkcije test (u ovom slucaju metode, jer svaka funkcija koja pripada klasi se zove **metoda**) ce se izvrsavati tek kada pozovemo i **uvek ce tada dobiti "kontekst" za this**.

**THIS se uvek setuje kada se zove metoda, i bude on koji je pozvao**

```php
<?php

class Nesto
{
    public $bar;
    public $bla;

    public function test()
    {
        $this->bla = 5;
        $this->foo(); // u nekom trenutku this postaje $n
    }

    public function setBar($value)
    {
        $this->bar = $value;
    }

    private function foo()
    {
        var_dump($this); // u nekom trenutku this ce biti $n
        return 5;
    }
}


$n = new Nesto();
$n->test(); // $n mi postaje "this"
var_dump($n->bla) // 5

$instance = [];
for ($i = 0; $i < 3; $i++)
{
    $t = new Nesto();
    $instance[] = $t; // push $t
    $t->setBar($i);
}

var_dump($instance[0]->bar); // 0
var_dump($instance[1]->bar); // 1
var_dump($instance[2]->bar); // 2
```

### "Returning" this

Na prvi pogled konfuzno deluje, ali mozemo da vratimo `$this`

```php
class Chain
{
    public $data = [];

    public function add($n)
    {
        $this->data[] = $n; // push
        return $this;
    }
}

$chain = new Chain();
$t = $chain->add(5)->add(15);
```

Komplikovaniji primer sa jos jednom instancom:

```php
<?php

class Chain
{
    public $data = [];

    public function add($n)
    {
        $this->data[] = $n; // push
        return new ChainWrap($this);
    }
}

class ChainWrap
{
    private $chain;

    public function __construct($chain)
    {
        $this->chain = $chain;
    }

    public function unwrap()
    {
        return $this->chain;
    }
}

$chain = new Chain();
$chain->add(5)->unwrap()->add(10)->unwrap()->add()->unwrap();

var_dump($chain);
```

## Beleske

- Dve `instance` su jednake kada su im iste **reference**
- Dva primitivna tipa (broj, bool, float) su jednaka kada im je ista **vrednost**
