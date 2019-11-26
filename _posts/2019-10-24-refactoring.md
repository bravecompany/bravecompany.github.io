---
layout: post
title: 'Refactoring'
author: judy.kim
date: 2019-10-28 13:11
categories: [techCamp]
tags: [refactoring]
comments: true
---
## Chapter1. 리팩토링

### 리팩토링이란?
기능(동작)의 변화없이 코드를 개선하는 작업 (결과는 동일)

### 리팩토링은 왜 해야하는가?
- 코드의 가독성을 높여 소프트웨어를 더 이해하기 쉽게 만든다.
- 프로그램의 구조를 명확히 함으로써 버그를 더 잘 찾을 수 있게 한다.
- 소프트웨어의 디자인을 개선시킨다.
- 시간단축, 유지보수 편리 등의 개발생산성을 높여준다.

### 리팩토링은 언제 해야 하는가?
- 동일한 코드가 3번 이상 반복될 때
- 기능을 추가하기 전 (수정해야 할 코드에 대한 이해가 높아지며 기능 추가가 쉬운 디자인으로 변경 됨)
- 버그를 수정해야 할 때
- 코드 검토(Code Review)를 할 때

### 리팩토링 기본 원칙
- 간단해야 한다. Keep It Simple and Stupid (KISS)
- 반복하지 말아야 한다. Don't Repeat Yourself (DRY)
- 테스트 주도 개발 Test-Driven Development (TDD)


## Chapter2. 리팩토링 대상 선정 (코드의 구린내)
### 정리되지 않아 누적된 코드

|||
|--------------------------------------------- | ---- |
|장황한 메서드(Long Method)                     |하나의 메서드가 너무 많은 기능을 하며 쉽게 이해하기 어려운 경우
|방대한 클래스(Large Class)                     |기능이 지나치게 많은 클래스에는 엄청난 수의 인스턴스 변수가 들어있으며 중복된 코드가 존재할 확률이 높다.
|과다 매개변수(Long Parameter List)             |메서드에 매개변수가 3~4개 이상 되는 경우
|데이터 덩어리                                   |많은 매개변수 리스트 등의 데이터 덩어리
|기본 타입에 대한 강박관념                          |관련된 데이터를 묶지 못하고 흩어놓게 되면, 각각의 데이터에 대한 정보를 외부에 공개해야한다. 메서드를 만들때도 각각의 데이터를 파라미터로 넘겨주어야 하기에 파라미터의 갯수가 늘어나게 된다.|

### 변경을 어렵게 만드는 코드

|||
|--------------------------------------------- | ---- |
|수정의 산발(Divergent Change)                   |한 클래스가 다양한 원인때문에 다양한 방식으로 수정해야 하는 경우 (버그 발생 확률이 높은 상태)
|기능의 산재                                     |하나의 기능을 변경하기 위해 많은 클래스들을 수정하는 경우 (클래스가 구조적으로 엮여있는 형태)
|평행 상속 구조                    |기능의 산재(하나의 기능을 변경하기 위해 많은 클래스를 수정하는 경우)의 특별한 경우 중 하나로, 한 클래스의 서브클래스를 만들면 다른 곳에도 모두 서브클래스를 만들어 주어야 하는 경우

### 불필요한 코드

|||
|--------------------------------------------- | ---- |
|게으른 클래스                     |충분한 기능을 하지 않는 클래스
|추측성 일반화                     |코드가 구현되지 않는 미래의 필요한 기능을 지원하기 위해 "경우에 따라" 작성되는 것으로 현재 사용되지 않고 있는 코드
|불필요한 주석                     |코드 자체를 이해하는데 방해가 될 수 있는 의미없는 주석
|중복된 코드(Duplicated Code)                   |한 클래스의 서로 다른 두 메서드 안에 같은 코드가 있거나 동일한 슈퍼클래스를 갖는 두 서브클래스에서 같은 코드가 나타나는 경우
|데이터 클래스                     |특별한 기능없이 데이터만을 저장하기 위한 클래스로 각 필드에 대한 get/set기능만 보유하고 있을 뿐 아무 기능도 하지 않는 클래스

### 객체지향기법이 잘못 적용된 코드

|||
|--------------------------------------------- | ---- |
switch문                                       |switch 문의 단점은 반드시 중복이 생긴다는 점이다. 동일한 switch가 다른 곳에서 또 쓰일가능성이 크다. switch 문에 새 코드행을 추가하려면 산발적으로 존재하는 switch 문을 전부 찾아서 수정하는 등의 어려움이 있다. (간단한 switch문 제외)
임시필드                          |복잡한 알고리즘이 여러 변수를 필요로 할 때 흔히 나타나는 경우로 특정 상황에 따라 값이 변하게 되는 임시 변수를 의미
인터페이스가 다른 대용 클래스       |다른 클래스의 두 메소드가 같은 일을하지만 다른 메소드 시그니처를 가질 때 발생하는 경우
거부된 유산                       |부모 클래스로부터 상속 받은 메서드 혹은 데이터를 사용하지 않는 경우

### 잘못 엮여있는 코드

|||
|--------------------------------------------- | ---- |
|잘못된 소속(기능에 대한 욕심)                    |자기가 속한 클래스보다 다른 클래스를 더 많이 사용하는 경우
메시지 체인                       |클라이언트가 한 객체에 다른 객체를 요청하면 다른 객체를 요청하고 그 다른 객체가 또 다른 객체에 묻는 경우 (클라이언트와 클래스 구조와 결합)
과잉 중개 메서드                   |클래스가 다른 클래스로 위임하는 역할만 담당하는 경우
지나친 관여                       |클래스가 지나치게 밀접하게 연관되어 있어 데이터와 동작이 한 곳에서 처리되지 않는 경우
불완전한 라이브러리 클래스          |필요한 기능을 추가하는데 있어서 라이브러리 클래스를 변경할 수 없는 경우



## Chapter3. 리팩토링 기법

### 1. 메서드 정리
메서드의 가독성을 높이고 기능을 단순화한다.

#### 1) 메서드 추출(Extract Method)
동일한 코드 뭉치를 별도의 메서드로 빼내거나 코드 재사용을 위해 기능별로 메서드를 적절히 잘게 쪼갠다.
>리팩토링 전

~~~
function printOwing() {
  $this->printBanner();

  print("name:  " . $this->name);
  print("amount " . $this->getOutstanding());
}
~~~

>리팩토링 후

~~~
function printOwing() {
  $this->printBanner();
  $this->printDetails($this->getOutstanding());
}

function printDetails($outstanding) {
  print("name:  " . $this->name);
  print("amount " . $outstanding);
}
~~~

#### 2) 메서드 내용 직접 삽입(Inline Method)
메서드의 기능이 지나치게 단순한 경우 메서드를 없애고 호출하는 메서드 자체에 해당 메서드의 기능을 삽입해야 한다.
(보통 리팩토링은 의도한 기능을 한 눈에 파악할 수 있게 직관적인 메서드명을 사용하고 메서드 자체가 간결하게 하지만 간혹 따로 메서드로 만들 필요가 없이 지나치게 단순할 경우 위와 같이 진행함)
>리팩토링 전

~~~
function getRating() {
  return ($this->moreThanFiveLateDeliveries()) ? 2 : 1;
}
function moreThanFiveLateDeliveries() {
  return $this->numberOfLateDeliveries > 5;
}
~~~
>리팩토링 후

~~~
function getRating() {
  return ($this->numberOfLateDeliveries > 5) ? 2 : 1;
}
~~~

#### 3) 임시변수 내용 직접 삽입(inline temp)
간단한 수식을 대입받는 임시변수로 인해 다른 리팩토링 기법 적용이 힘들 때 그 임시변수를 참조하는 부분을 전부 수식으로 치환한다.
>리팩토링 전
~~~
$basePrice = $anOrder->basePrice();
return $basePrice > 1000;
~~~

>리팩토링 후
~~~
return $anOrder->basePrice() > 1000;
~~~

#### 4) 임시변수를 메서드 호출로 전환(Replace Temp with Query)
- 수식의 결과를 저장하는 임시변수가 있을 때, 그 수식을 빼내어 메서드로 만든 후 임시변수 참조 부분을 전부 수식으로 교체한다. (다른 메서드에서도 호출 가능)
- 메서드 추출을 적용하기 전에 적용되어야 한다.(지역변수가 많을수록 메서드 추출이 힘들어지므로 최대한 많은 변수를 메서드 호출로 전환해야 함)

>리팩토링 전
~~~
$basePrice = $this->quantity * $this->itemPrice;
if ($basePrice > 1000) {
  return $basePrice * 0.95;
} else {
  return $basePrice * 0.98;
}
~~~

>리팩토링 후
~~~
if ($this->basePrice() > 1000) {
  return $this->basePrice() * 0.95;
} else {
  return $this->basePrice() * 0.98;
}

...

function basePrice() {
  return $this->quantity * $this->itemPrice;
}
~~~

#### 5) 직관적 임시변수 사용(Introduce Explaining Variable)
- 사용된 수식이 복잡할 때 수식의 결과나 수식의 일부분을 용도에 부합하는 직관적 이름의 임시변수에 대입한다.
- 복잡한 수식때문에 메서드를 이해하기 어려울 때, 임시변수를 사용하여 수식을 더 처리하기 쉽게 쪼갠다.

#### 6) 임시변수 분리(Split Temporary Variable)
- 루프 변수나 값 누적용이 아닌 임시변수에 여러 번 값이 대입될 때 각 대입마다 다른 임시변수를 사용한다.
- 임시변수 하나를 2가지 용도로 사용하면 해당 코드는 타인이 이해하기 어려우므로 다른 임시변수를 사용해야 함

>리팩토링 전
~~~
$temp = 2 * ($this->height + $this->width);
echo $temp;
$temp = $this->height * $this->width;
echo $temp;
~~~

>리팩토링 후
~~~
$perimeter = 2 * ($this->height + $this->width);
echo $perimeter;
$area = $this->height * $this->width;
echo $area;
~~~

#### 7) 메서드를 메서드 객체로 대체(Replace Method with Method Object)
긴 메서드가 있는데 지역변수 때문에 메서드 추출을 적용할 수 없는 경우에 메서드를 그 자신을 위한 객체로 바꿔 모든 지역변수가 그 객체의 필드가 되도록 한다.
이렇게 하면, 메서드를 같은 객체 안의 여러 메서드로 분해할 수 있다.

>리팩토링 전
~~~
class Account
{
    private $state = 3;
    public function getTripleState()
    {
        return $this->state * 3;
    }
    public function foo($first_param, $second_param)
    {
        $temporary_1 = $first_param * $second_param;
        $temporary_2 = $first_param * $this->getTripleState();
        if (($temporary_1 - $temporary2) % 2)
        {
            return $first_param - 2;
        }
        return $second_param + 4;
    }
}
~~~

>리팩토링 후
~~~
class foo
{
    private $account,$temporary_1,$temporary_2,$first_param,$second_param;
    public function __construct($account, $first_param, $second_param)
    {
        $this->account = $account;
        $this->first_param = $first_param;
        $this->second_param = $second_param;
    }

    public function foo($first_param, $second_param)
    {
        $foo = new foo($this, $first_param, $second_param);
        return $foo->compute();
    }

    public function compute()
    {
        $this->setup();
        if (($this->temporary_1 - $this->temporary2) % 2)
        {
            return $this->first_param - 2;
        }
        return $this->second_param + 4;
    }

    public function setup()
    {
        $this->temporary_1 = $this->first_param * $this->second_param;
        $this->temporary_2 = $this->first_param * $this->account->getTripleState();
    }
}
~~~

#### 8) 알고리즘 대체(Substitute Algorithm)
알고리즘을 변경하고 싶을 떄, 메서드 몸체를 새 알고리즘으로 대체한다.

>리팩토링 전
~~~
function foundPerson(array $people){
  for ($i = 0; $i < count($people); $i++) {
    if ($people[$i] === "Don") {
      return "Don";
    }
    if ($people[$i] === "John") {
      return "John";
    }
    if ($people[$i] === "Kent") {
      return "Kent";
    }
  }
  return "";
}
~~~

>리팩토링 후
~~~
function foundPerson(array $people){
  foreach (["Don", "John", "Kent"] as $needle) {
    $id = array_search($needle, $people, true);
    if ($id !== false) {
      return $people[$id];
    }
  }
  return "";
}
~~~


### 2. 객체간의 이동

#### 1) 메서드 이동(Move Method)
메서드가 자신이 정의된 클래스보다 다른 클래스의 기능을 더 많이 사용하고 있다면, 이 메서드를 가장 많이 사용하고 있는 클래스에 비슷한 몸체를 가진 새로운 메서드를 만들고 이전 메서드는 간단한 위임으로 바꾸거나 제거한다.
![메서드_이동](http://coco7846.cdn3.cafe24.com/refactoring/move_method.png)

#### 2) 필드 이동(Move Property (or Field))
필드가 자신이 정의된 클래스보다 다른 클래스에 의해 더 많이 사용되고 있다면 타겟 클래스에 새로운 필드를 만들고 기존 필드를 사용하는 모든 부분을 변경한다.
![필드_이동](http://coco7846.cdn3.cafe24.com/refactoring/move_field.png)

#### 3) 클래스 추출(Extract Class)
두 개의 클래스가 해야 할 일을 하나의 클래스가 하고 있는 경우, 새로운 클래스를 만들어 관련있는 필드와 메서드를 이전 클래스에서 새로운 클래스로 옮긴다.
![클래스_추출](http://coco7846.cdn3.cafe24.com/refactoring/extract_method.png)

#### 4) 클래스 직접 삽입(Inline Class)
래스가 하는 일이 많지 않을 때, 그 클래스에 있는 모든 변수와 메서드를 다른 클래스로 옮기고 그 클래스를 제거한다.
![클래스_직접삽입](http://coco7846.cdn3.cafe24.com/refactoring/inline_class.png)

#### 5) 대리객체 은폐(Hide Delegate)
클라이언트가 객체의 위임 클래스를 직접 호출하고 있는 경우 서버에 메서드를 만들어서 대리객체(delegate)를 숨긴다.
![대리객체_은폐](http://coco7846.cdn3.cafe24.com/refactoring/hide_delegation.png)

#### 6) 과잉 중개 메서드 제거(Remove the Middle Man)
어떤 클래스의 인터페이스가 절반도 넘는 메서드가 기능을 다른 클래스에 위임하고 있다면 원리가 구현된 객체에 직접 접근하는 방식으로 수정한다.
모든 위임을 추적할 필요없이 기능을 확장할 수 있다. (대리객체 은폐와 반대개념)
![과잉중개메서드_제거](http://coco7846.cdn3.cafe24.com/refactoring/remove_middle_man.png)

#### 7) 외래 메서드 사용(Introduce Foreign Method)
사용하고 있는 서버 클래스에 부가적인 메서드가 필요하지만 클래스를 수정할 수 없는 경우에는 첫 번째 인자로 서버 클래스의 인스턴스를 받는 메서드를 클라이언트에 만들어야 한다.

>리팩토링전
~~~
Class Bill
{
    protected $previous_end;
    protected $new_start;

    public function setPreviousEnd($date)
    {
        $this->previous_end = new DateTime($date);
        $this->new_start = clone $this->previous_end;
        $this->new_start->modify('+1 day');
    }

    public function getNewStart($format = 'm-d-Y')
    {
        return $this->new_start->format($format);
    }

    public function getPreviousEnd($format = 'm-d-Y')
    {
        return $this->previous_end->format($format);
    }
}
~~~

>리팩토링후
~~~
Class Bill
{
    protected $previous_end;
    protected $new_start;

    public function setPreviousEnd($date)
    {
        $this->previous_end = new DateTime($date);
        $this->new_start = self::nextDay($this->previous_end);
    }

    public function getNewStart($format = 'm-d-Y')
    {
        return $this->new_start->format($format);
    }

    public function getPreviousEnd($format = 'm-d-Y')
    {
        return $this->previous_end->format($format);
    }

    private static function nextDay(DateTime $date)
    {
        $next_day = clone $date;
        return $next_day->modify('+1 day');
    }
}
~~~


### 3.조건문의 간결화
#### 1) 조건문 쪼개기(Decompose Conditional)
복잡한 조건문이 있는 경우 메서드로 추출

>리팩토링 전
~~~
if ($date->before(SUMMER_START) || $date->after(SUMMER_END)) {
  $charge = $quantity * $winterRate + $winterServiceCharge;
} else {
  $charge = $quantity * $summerRate;
}
~~~

>리팩토링 후
~~~
if (isSummer($date)) {
  $charge = summerCharge($quantity);
} else {
  $charge = winterCharge($quantity);
}
~~~

#### 2) 조건문 통합하기(Consolidate Conditional Expression)
같은 결과값을 가진 조건문의 경우, 하나의 조건식으로 결합한다. (AND 연산자 사용)

>리팩토링 전
~~~
function disabilityAmount() {
  if ($this->seniority < 2) {
    return 0;
  }
  if ($this->monthsDisabled > 12) {
    return 0;
  }
  if ($this->isPartTime) {
    return 0;
  }
  // disability amount 계산
  ...
}
~~~

>리팩토링 후
~~~
function disabilityAmount() {
  if ($this->isNotEligableForDisability()) {
    return 0;
  }
  // disability amount 계산
  ...
}
~~~

#### 3) 중복된 조건식 통합하기(Consolidate Duplicate Conditional Fragments)
같은 코드가 조건식의 모든 분기에 포함되어 있다면 조건식의 밖으로 옮긴다.

>리팩토링 전
~~~
if (isSpecialDeal()) {
  total = price * 0.95;
  send();
}
else {
  total = price * 0.98;
  send();
}
~~~

>리팩토링 후
~~~
if (isSpecialDeal()) {
  total = price * 0.95;
}
else {
  total = price * 0.98;
}
send();
~~~

#### 4) 제어플래그 제거(Remove Control Flag)
일련의 boolean식에서 컨트롤 플래그 역할을 하는 변수가 있는 경우, break 또는 return을 대신 사용한다.

>리팩토링 전
~~~
class CreditCardRepository
{
    public $transactions = array();
    public $max_amount = 10000;
    public $card_blocked = false;

    public function checkAmount()
    {
        $found = false;
        foreach($this->transactions as $transaction)
        {
            if(!$found)
            {
                if ($transaction > $this->max_amount)
                {
                    $this->blockCreditCardForOneMonth(time());
                    $found = true;
                }
            }
        }
    }

    public function blockCreditCardForOneMonth($time)
    {
        $this->card_blocked = $time;
    }
}
~~~

>리팩토링 후
~~~
class CreditCardRepository
{
    ...
    public function checkAmount()
    {
        foreach($this->transactions as $transaction)
        {
            if ($transaction > $this->max_amount)
            {
                $this->blockCreditCardForOneMonth(time());
                break;
            }
        }
    }
    ...
}
~~~
#### 5) 중첩된 조건문을 보호절로 대체(Replace Nested Conditional with Guard Clauses)
메소드가 정상실행경로가 명확하지 않은 조건별 행동을 하고  모든 특정 경우에 보호구문(Guard Clause, 감시절)을 사용한다.

>리팩토링 전
~~~
function getPayAmount() {
  if ($this->isDead) {
    $result = $this->deadAmount();
  } else {
    if ($this->isSeparated) {
      $result = $this->separatedAmount();
    } else {
      if ($this->isRetired) {
        $result = $this->retiredAmount();
      } else {
        $result = $this->normalPayAmount();
      }
    }
  }
  return $result;
}
~~~

>리팩토링 후
~~~
function getPayAmount() {
  if ($this->isDead) {
    return $this->deadAmount();
  }
  if ($this->isSeparated) {
    return $this->separatedAmount();
  }
  if ($this->isRetired) {
    return $this->retiredAmount();
  }
  return $this->normalPayAmount();
}
~~~

#### 6) 조건문을 재정의로 전환(Replace Conditional with Polymorphism)
- 객체에 타입에 따라 조건문이나 분기문을 두어 작성을 하게 되면 메서드가 길어지며 객체 타입에 따라 다른 기능을 실행하는 조건문이 있을 땐 조건문의 각 절을 하위클래스의 재정의 메소드 안으로 옮긴다.
- 객체의 타입에 따라서 다른 행동을 하는 조건이 있다면 각 조건의 내용을 서브클래스의 메소드로 오버라이딩하도록 옮기고, 원본 메소드는 추상메소드로 변경한다.

>리팩토링 전
~~~
class Bird {
  // ...
  public function getSpeed() {
    switch ($this->type) {
      case EUROPEAN:
        return $this->getBaseSpeed();
      case AFRICAN:
        return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
    }
    throw new Exception("Should be unreachable");
  }
  // ...
}
~~~

>리팩토링 후
~~~
abstract class Bird {
  ...
  abstract function getSpeed();
  ...
}

class European extends Bird {
  public function getSpeed() {
    return $this->getBaseSpeed();
  }
}
class African extends Bird {
  public function getSpeed() {
    return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  public function getSpeed() {
    return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
  }
}

$speed = $bird->getSpeed();
~~~

#### 7) introduce assertion
특정부분의 코드가 프로그램의 상태에 관한 가정을 하고 있다면 그 가정을 명확한 Assert의 형태로 변경한다.

>리팩토링 전
~~~
function getExpenseLimit() {
  return ($this->expenseLimit !== NULL_EXPENSE) ?
    $this->expenseLimit:
    $this->primaryProject->getMemberExpenseLimit();
}
~~~

>리팩토링 후
~~~
function getExpenseLimit() {
  assert($this->expenseLimit !== NULL_EXPENSE || isset($this->primaryProject));

  return ($this->expenseLimit !== NULL_EXPENSE) ?
    $this->expenseLimit:
    $this->primaryProject->getMemberExpenseLimit();
}
~~~
#### 8) null객체 사용
null검사를 null객체에 위임

>리팩토링 전
~~~
if ($customer === null) {
  $plan = BillingPlan::basic();
} else {
  $plan = $customer->getPlan();
}
~~~

>리팩토링 후
~~~
class NullCustomer extends Customer {
  public function isNull() {
    return true;
  }
  public function getPlan() {
    return new NullPlan();
  }
}

$customer = ($order->customer !== null) ?
$order->customer :
new NullCustomer;

$plan = $customer->getPlan();
~~~


### 4. 메서드 호출의 단순화
#### 1) 메서드명 변경(Rename Method)
기능 및 목적에 걸맞는 이름으로 변경
#### 2) 매개변수 추가(Add Parameter)
어떤 메서드가 그 메서드를 호출하는 부분에서 더 많은 정보를 필요로 한다면 이 정보를 넘길 수 있는 객체에 대한 매개변수를 추가한다.
#### 3) 매개변수 제거(Remove Parameter)
매개변수가 메서드 몸체에서 더 이상 사용되지 않는다면, 해당 매개변수를 제거한다.
#### 4) 상태변경 메서드와 값 반환 메서드 분리(Separate Query from Modifier)
값 반환 기능과 객체 상태 변경 기능이 한 메서드에 들어 있을 땐 질의 메서드와 변경 메서드로 분리한다.)
#### 5) 메서드의 매개변수화(Parameterize Method)
기능은 비슷하지만 몇 가지 값에 따라 결과가 달라지는 메서드가 있을 때 각 메서드를 전달 된 매개변수에 따라 다른 작업을 처리하는 하나의 메서드로 만들어 관리하면 편하다.
#### 6) 매개변수의 메서드화(Replace Parameter with Method)
객체가 메서드(A)를 호출해서 그 결과를 다른 메서드(B)에 매개변수로 전달하는데, 결과를 매개변수로 받는 메서드(B)도 직접 메서드(A)를 호출할 수 있을 땐 매개변수를 없애고 메서드(A)를 메서드(B)가 호출하게 한다.
(경우에 따라 값이 달라지는 함수는 적용하지 않는 것이 좋음)

>리팩토링 전
~~~
$basePrice = $this->quantity * $this->itemPrice;
$seasonDiscount = $this->getSeasonalDiscount();
$fees = $this->getFees();
$finalPrice = $this->discountedPrice($basePrice, $seasonDiscount, $fees);
~~~

>리팩토링 후
~~~
$basePrice = $this->quantity * $this->itemPrice;
$finalPrice = $this->discountedPrice($basePrice);
~~~
#### 7) 객체를 통째로 전달(Preserve Whole Object)
객체에서 가져온 여러값을 메서드 호출에서 매개변수로 전달할 땐 그 객체를 통째로 전달한다.
#### 8) 매개변수를 메서드로 대체(Replace Parameter with Explicit Method)
매개변수로 전달된 값에 따라 메서드가 다른 코드를 실행할 땐 그 매개변수로 전달될 수 있는 모든 값에 대응하는 메서드를 각각 작성한다.

>리팩토링 전
~~~
function setValue($name, $value) {
  if ($name === "height") {
    $this->height = $value;
    return;
  }
  if ($name === "width") {
    $this->width = $value;
    return;
  }
  assert("Should never reach here");
}
~~~

>리팩토링 후
~~~
function setHeight($arg) {
  $this->height = $arg;
}
function setWidth($arg) {
  $this->width = $arg;
}
~~~

#### 9) 매개변수 대신 객체 사용(Introduce parameter object)
여러 개의 매개변수가 항상 붙어 다닐 땐 그 매개변수들을 객체로 바꾼다.
#### 10) 쓰기메서드 제거(Remove Setting Method)
생성 시 지정한 필드 값이 절대로 변경되지 말아야 할 때, 그 필드를 설정하는 모든 쓰기 메서드를 제거한다.
#### 11) 메서드 은폐(Hide Method)
메서드가 다른 클래스에서 사용되지 않는다면, 해당 메서드를 private으로 만든다.
#### 12) 생성자를 팩토리 메서드로 전환(Replace Constructor with Factory Method)
객체를 생성할 때 단순한 생성만 수행하게 해야 할 땐 생성자를 팩토리 메서드로 교체한다.
* 팩토리 메서드란? 직접 객체를 생성하지 않고 팩토리 메서드 클래스를 통해 객체를 대신 생성시키고 그 객체를 반환 받아 사용
>리팩토링 전
~~~
class Employee {
  // ...
  public function __construct($type) {
   $this->type = $type;
  }
  // ...
}
~~~

>리팩토링 후
~~~
class Employee {
  // ...
  static public function create($type) {
    $employee = new Employee($type);
    return $employee;
  }
  // ...
}
~~~

#### 13) 에러코드는 예외로 대체(Replace Error Code with Exception)
메서드가 에러를 나타내는 특별한 코드를 가지고 있는 경우, 대신 예외로 처리한다.
#### 14) 예외는 테스트로 대체(Replace Exception with Test)
호출부에서 먼저 검사할 수 있는 조건에 대해 예외를 던지고 있다면, 호출부가 먼저 검사하도록 변경한다.



### 5. 일반화 다루기
#### 1) 수퍼클래스로 필드 이동(Pull Up Field)
두 서브클래스가 동일한 필드를 가지고 있다면, 그 필드를 수퍼클래스로 이동시킨다.
#### 2) 수퍼클래스로 메서드 이동(Pull Up Method)
동일한 일을 하는 메서드를 여러 서브클래스에서 가지고 있다면, 이 메서드들이 메서드를 수퍼클래스로 이동시킨다.
#### 3) 생성자 몸체 이동(Pull Up Constructor Body)
서브 클래스들이 대부분 동일한 몸체를 가진 생성자를 가지고 있다면, 수퍼클래스에 생성자를 만들고 서브클래스 메서드에서 이것을 호출한다.
>리팩토링 전
~~~
class Manager extends Employee {
  public function __construct($name, $id, $grade) {
    $this->name = $name;
    $this->id = $id;
    $this->grade = $grade;
  }
  // ...
}
~~~

>리팩토링 후
~~~
class Manager extends Employee {
  public function __construct($name, $id, $grade) {
    parent::__construct($name, $id);
    $this->grade = $grade;
  }
  // ...
}
~~~

#### 4) 서브클래스로 메서드 이동(Push Down Method)
수퍼클래스에 있는 동작이 서브클래스 중 일부에만 관련되어 있다면, 그 동작을 관련된 서브클래스로 이동시킨다.
#### 5) 서브클래스로 필드 이동(Push Down Field)
어떤 필드가 일부 서브클래스에 의해서만 사용되고 있다면, 그 필드를 관련된 서브클래스로 이동시킨다.
#### 6) 서브클래스 추출(Extract Subclass)
어떤 클래스가 일부 인스턴스에 의해서만 사용되는 기능을 가지고 있다면, 기능의 부분집합을 담당하는 서브클래스를 생성한다.
#### 7) 수퍼클래스 추출(Extract Super Class)
비슷한 메서드와 필드를 가진 두 개의 클래스가 있다면, 수퍼클래스를 만들어 공통된 메서드와 필드를 수퍼클래스로 이동시킨다.
#### 8) 계층 축소(Collapse Hierarchy)
수퍼클래스와 서브클래스가 별로 다르지 않다면 하나로 합친다.
#### 9) 템플릿 메서드화(Form Template Method)
각각의 서브클래스에 동일한 순서로 비슷한 단계를 행하지만 단계가 완전히 같지는 않은 두 메서드가 있다면, 그 단계를 동일한 시그니처를 가진 메서드로 만든다.
이렇게 하면 원래의 두 메서드는 서로 같아지므로, 수퍼클래스로 올릴 수 있다.
![템플릿_메서드화](http://coco7846.cdn3.cafe24.com/refactoring/form_template_method.png)

#### 10) 상속을 위임으로 대체(Replace Inheritance with Delegation)
서브클래스가 수퍼클래스 인터페이스의 일부분만 사용하거나 또는 데이터를 상속받고 싶지 않은 경우 수퍼클래스를 위한 필드를 만들고 메서드들이 수퍼클래스에 위임하도록 변경한 후 상속 관계를 제거한다.
![위임으로대체](http://coco7846.cdn3.cafe24.com/refactoring/replace_inheritance_with_delegation.png)

#### 11) 위임을 상속으로 대체(Replace Delegation with Inheritance)
위임(delegation)을 사용하고 있고 전체 인터페이스를 위한 단순 델리게이션을 자주 사용한다면 대신하는(delegating) 클래스를 위임(delegate)의 서브클래스로 만든다.
![상속으로대체](http://coco7846.cdn3.cafe24.com/refactoring/replace_delegation_with_inheritance.png)

### 6. 데이터 체계화
#### 1) 필드 자체 캡슐화(Self-Encapsulate Field)
필드에 직접 접근하고 있는데 필드에 대한 결합이 이상해지면 get/set 메소드를 만들어 필드에 접근한다.
>리팩토링 전
~~~
private $low;
private $high;

function includes($arg) {
  return $arg >= $this->low && $arg <= $this->high;
}
~~~
>리팩토링 후

~~~
private $low;
private $high;

function includes($arg) {
  return $arg >= $this->getLow() && $arg <= $this->getHigh();
}
function getLow() {
  return $this->low;
}
function getHigh() {
  return $this->high;
}
~~~

#### 2) 데이터 값을 객체로 전환(Replace Data Value with Object)
데이터 항목에 데이터나 기능을 더 추가해야 할 때는 데이터 항목을 객체로 전환한다.
#### 3) 값을 참조로 전환(Change Value to Reference)
여러 개의 동일한 인스턴스를 하나의 객체로 바꾸고 싶다면 그 객체를 참조객체로 변경한다.
#### 4) 참조를 값으로 전환(Change Reference to Value)
참조 객체가 작고 수정할 수 없으며 관리하기 힘들 땐 그 참조객체를 값 객체로 만든다.
#### 5) 클래스의 단방향 연결을 양방향으로 전환(Change Unidirectional Association to Bidirectional)
각각 서로의 기능을 필요로 하는 클래스에서 한 방향으로만 연결되어 있을 때, 참조를 추가하고 두 클래스를 모두 업데이트 할 수 있게 접근 한정자를 수정한다.
#### 6) 클래스의 양방향 연결을 단방향으로 전환(Change Bidirectional Association to Unidirectional)
두 클래스가 양방향으로 연결되어있는데 한 클래스가 다른 클래스의 기능을 더 이상 사용하지 않을 때 연결을 제거한다.
#### 7) 매직넘버를 심볼릭 상수로 전환(Replace Magic Number with Symbolic Constant)
특정 숫자값을 사용 중일 경우, 해당 값의 의미를 나타내는 이름을 가진 상수를 만들어 대체한다.
>리팩토링 전
~~~
function potentialEnergy($mass, $height) {
  return $mass * $height * 9.81;
}
~~~

>리팩토링 후
~~~
define("GRAVITATIONAL_CONSTANT", 9.81);

function potentialEnergy($mass, $height) {
  return $mass * $height * GRAVITATIONAL_CONSTANT;
}
~~~

#### 추천사이트
1)[리팩토링_정리](https://refactoring.guru/smells/duplicate-code)
2)[refactoring GURU](https://refactoring.guru/extract-method)

