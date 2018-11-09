## Factory Methodパターンとは？

インスタンスの生成をTemplate Methodパターンで構成したもの。

## 使い所

クラスの生成する際に他のクラスの連携やデータの参照が必要な場合で、且つ、実行するものがある程度決まっているとき？


## phpによるサンプル

```
<?php

abstract class Product
{
    /**
     * @var string
     */
    protected $owner;

    /**
     * Product constructor.
     * @param string $owner
     */
    public function __construct(string $owner)
    {
        $this->owner = $owner;
    }

    abstract public function execute();
}

abstract class Factory
{
    public function create(string $owner) : Product
    {
        $product = $this->createProduct($owner);
        $this->registerProduct($product);
        return $product;
    }

    protected abstract function createProduct(string $owner) :Product;
    protected abstract function registerProduct(Product $product) :void;
}


class Card extends Product
{
    public function __construct(string $owner)
    {
        echo sprintf("create %s card", $owner);
        parent::__construct($owner);
    }

    public function execute()
    {
        echo sprintf("use %s card", $this->owner);
    }
}


class CardFactory extends Factory
{
    /**
     * @var array
     */
    private $cards = [];

    protected function createProduct(string $owner): Product
    {
        return new Card($owner);
    }

    protected function registerProduct(Product $product): void
    {
        $this->cards[] = $product;
    }

    /**
     * @return array
     */
    public function getCards(): array
    {
        return $this->cards;
    }

}

$factory = new CardFactory();
$card = $factory->create('polidog');
var_dump($card);
var_dump($factory->getCards()); // getCardsはLSPに違反している気がする
```
