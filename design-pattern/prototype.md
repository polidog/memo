## Prototypeパターン

オブジェクトを生成する際に、予めプロトタイプとしてオブジェクトを登録しておき、それをクローンしてオブジェクトを生成する。  

## 例

<?php

interface Product {
    public function exec(string $s) :void;

    public function createClone():Product;
}

class Manager {

    /**
     * @var Product[]
     */
    private $showcase = [];

    public function register(string $name, Product $prototype)
    {
        $this->showcase[$name] = $prototype;
    }

    public function create(string $name) : Product
    {
        $prototype = $this->showcase[$name];
        return $prototype->createClone();
    }
}

class MessageBox implements Product
{
    /**
     * @var string
     */
    private $decochar;

    /**
     * MessageBox constructor.
     * @param string $decochar
     */
    public function __construct(string $decochar)
    {
        $this->decochar = $decochar;
    }

    public function exec(string $s): void
    {
        $length = mb_strlen($s) + 4;

        $this->loop($length);

        echo sprintf("%s %s %s", $this->decochar, $s, $this->decochar);
        echo "\n";

        $this->loop($length);
    }

    private function loop(int $length)
    {
        for ($i = 0; $i < $length; $i++) {
            echo $this->decochar;
        }
        echo "\n";
    }

    public function createClone(): Product
    {
        return clone $this;
    }
}

class UnderlinePen implements Product
{
    /**
     * @var
     */
    private $unchar;

    /**
     * UnderlinePen constructor.
     * @param $unchar
     */
    public function __construct($unchar)
    {
        $this->unchar = $unchar;
    }

    public function exec(string $s): void
    {
        $length = mb_strlen($s);
        echo sprintf("\"%s\"", $s);
        echo "\n";

        for ($i = 0; $i < $length; $i++) {
            echo $this->unchar;
        }
        echo "\n";
    }

    public function createClone(): Product
    {
        return clone $this;
    }

}


$manager = new Manager();
$upen = new UnderlinePen('~');
$mbox = new MessageBox('*');
$sbox = new MessageBox('/');

$manager->register('strong message', $upen);
$manager->register('warning box', $mbox);
$manager->register('slash box', $sbox);

$p1 = $manager->create('strong message');
$p1->exec('Hello, world');

$p2 = $manager->create('warning box');
$p2->exec('Hello, world');

$p3 = $manager->create('slash box');
$p3->exec('Hello, world');