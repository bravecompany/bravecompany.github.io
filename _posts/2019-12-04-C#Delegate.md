layout: post
title: 'C# Delegate'
author: jun.park
date: 2019-12-04 23:30
categoried: [techCamp]
tags: [C#]
published: true



# C#

## 델리게이트
- C의 함수 포인터와 비슷한 개념. 메서드 파라미터와 리턴 타입에 대한 정의를 한 후, 동일한 파라미터와 리턴 타입을 가진 메서드를 서로 호환해서 불러 쓸 수 있는 기능.
- 델리게이트는 인스턴스가 아닌 타입임. 즉, int나 string같은 형식이며, "메서드를 참조하는 그 무엇"을 만들려면 델리게이트의 인스턴스를 따로 만들어야 한다.
- C의 함수 포인터와 다른 점

|함수 포인터|델리게이트|
|:---:|:---:|
|외부의 어떤 함수에 대한 주소값만을 가짐|클래스 객체의 인스턴스 메서드에 대한 레퍼런스를 갖기 위해<br>객체 레퍼런스의 주소를 함께 가지고 있음|
|하나의 함수 포인터를 가짐|하나 이상의 메서드 레퍼런스를 가질 수 있음|
|Type Safety를 완전히 보장하지 않음|엄격하게 Type Safety를 지원함|

### Multicast Delegate
---
 - 델리게이트 하나에 여러 개의 메서드를 할당. 
 - += 연산자를 사용하여 메서드를 계속 델리게이트에 추가하게 되는데, 내부적으로는 .NET MulticastDelegate 클래스에서 이 메서드들의 리스트 (InvocationList)를 관리하게 된다.
 - 복수개의 메서드들이 한 델리게이트에 할당되면, 이 델리게이트가 실행될 때 InvocationList로부터 순서대로 메서드를 하나씩 가져와 실행한다.
 - 예제 코드
  
  ``` java
  delegate void CustomDel(string s);

  class TestClass
  {
      static void Hello(string s)
      {
          Console.WriteLine($"  Hello, {s}!");
      }

      static void Goodbye(string s)
      {
          Console.WriteLine($"  Goodbye, {s}!");
      }

      static void Main()
      {
          CustomDel hiDel, byeDel, multiDel, multiMinusHiDel;
          hiDel = Hello;
          byeDel = Goodbye; 
          multiDel = hiDel + byeDel;

          multiMinusHiDel = multiDel - hiDel;

          Console.WriteLine("Invoking delegate hiDel:");
          hiDel("A");
          Console.WriteLine("Invoking delegate byeDel:");
          byeDel("B");
          Console.WriteLine("Invoking delegate multiDel:");
          multiDel("C");
          Console.WriteLine("Invoking delegate multiMinusHiDel:");
          multiMinusHiDel("D");
      }
  }
  ```
### Func 델리게이트
---
 - 결과를 반환하는 메서드를 참조함. 입력 파라미터는 최대 16개까지 사용 가능
 - 사용자 지정 델리게이트를 명시적으로 선언하지 않고, 매개 변수로 전달할 수 있는 메서드를 나타낼 수 있다.
 - 캡슐화된 메서드는 이 델리게이트에 의해 정의되는 메서드 시그니처 (파라미터 타입과 갯수)와 일치해야 함
 - 즉, 캡슐화된 메서드에는 값으로 전달되는 매개변수 하나가 있어야 하고, 값을 반환해야 함
 - 구문
   - public delegate TResult Func<in T, out TResult> ( T arg ) ...
 - 예제 코드
``` java
delegate string ConvertMethod(string inString);
class Program
{
	static void Main(string[] args)
    {
    	Func<string, string> convertMethod = UppercaseString;
        string name = "Func Delegate Example";
        
        Console.WriteLine("Before convert string : {0}", name);
        Console.WriteLine("After convert string : {0}", convertMeth(name));
        
    }
    
    static string UppercaseString(string inputString)
    {
    	return inputString.ToUpper();
    }
}
```

### Action 델리게이트
---
- 결과를 반환하지 않는 메서드를 참조함. 입력 파라미터는 최대 17개까지 사용 가능
- Action<T> 델리게이트를 사용하면 사용자 지정 대리자를 명시적으로 선언하지 않고도 메서드를 매개변수로 전달할 수 있다.
- 캡슐화된 메서드는 이 델리게이트에 의해 정의되는 메서드 시그니처(파라미터 타입 갯수)와 일치해야 함.
- 즉, 캡슐화된 메서드에는 값으로 전달되는 매개변수 하나가 있어야 하고, 값을 반환하지 않아야 함.
- 이 메서드는 void를 반환하거나 무시되는 값을 반환해야 함.
- 일반적으로는 작업을 수행하는 데 사용됨
- 구문
  - public delegate TResult Func<in T, out TResult> ( T arg ) ...
- 예제 코드
```java
class Program
{
	static void Main(string[] args)
    {
    	Action<string> messageTarget;
        
        if(Environment.GetCommandLineArgs().Length > 1)
        	messageTarget = ShowWindowMessage;
		else
        	messageTarget = Console.WriteLine();
		
        messageTarget("Hello Action Delegate");
    }
    
    static void ShowWindowMessage(string message)
    {
    	MessageBox.Show(message);
    }
}
```

  

## 이벤트
- 모든 이벤트는 특수한 형태의 델리게이트이다. 할당 연산자 ( = )를 사용할 수 없으며, 오직 이벤트 핸들러를 추가 / 삭제만 할 수 있다.
  - 이벤트 핸들러 추가
    - += 연산자. Subscribe
  - 이벤트 핸들러 삭제
    - -= 연산자. Unsubscribe
- 델리게이트와는 달리, 해당 클래스 외부에서는 직접 이벤트를 호출할 수 없다.
- 클래스 내에서 특정한 일(event)이 일어났음을 외부의 이벤트 가입자(subscriber)들에게 알려주는 기능.
- 클래스 내에서 일종의 필드처럼 정의된다.
- 이벤트 핸들러
  - 이벤트에 가입하는 외부 가입자 측에서는 이벤트가 발생했을 때 어떤 명령들을 실행할지 지정해 주는데, 이를 이벤트 핸들러라고 한다.
  - 하나의 이벤트에는 여러 개의 이벤트 핸들러를 추가할 수 있으며, 이벤트가 발생하면 추가된 이벤트핸들러들이 모두 차례로 호출된다.
  - 프로퍼티에서 get, set을 사용하듯이 add, remove를 사용할 수 있다.
 
![event](http://www.csharpstudy.com/CSharp/Image/event.png)

- 예제 코드

```java
class MyButton
{
	public string Text;
    public event EventHandler Click;
    
    public void MouseButtonDown()
    {
    	if(this.Click != null)
        {
        	Click(this, EventArgs.Empty);
        }
    }
}

public void Run()
{
	MyButton btn = new MyButton();
    btn.Click += new EventHandler(btn_Click);
    btn.Text = "Run";
}

void btn_Click(object sender, EventArgs e)
{
	MessageBox.Show("Button 클릭");
}
```

```java
class MyButton
{
	private EventHandler _click;
    public event EventHandler Click
    {
    	add
        {
        	_click += value;
        }
        remove
        {
        	_click -= value;
        }
    }
    
    public void MouseButtonDown()
    {
    	if(this._click != null)
        {
        	_click(this, EventArgs.Empty);
        }
    }
}
```

## 람다
- 익명 메서드를 단순한 계산식으로 표현한 것
- 델리게이트를 만드는데 사용할 수 있음.
1. 식 람다
   - 식이 본문으로 포함된 형태
   - (input-parameters) => expression
2. 문 람다
   - 문 블록이 본문으로 포함된 형태
   - (input-parameters) => { <sequence-of-statements> }
- 람다 선언 연산자 ( => )를 사용하여 본문에서 람다의 매개변수 목록을 구분함.
- 예제 코드

delegate를 이용하여 3개의 변수를 더해 출력하는 간단한 코드
```java
delegate double DelPlusMinus(int fVal, int mVal, int lVal);
public static void Main(string[] args)
{
	DelPlusMinus cal = delegate(int fVal, int mVal, int lVal)
    {
    	return fVal + mVal + lVal;
    };
    Console.WriteLine(cal(5, 4, 1));
}
```
람다식으로 바꾼 코드
```java
delegate double DelPlusMinus(int fVal, int mVal, int lVal);
public static void Main(string[] args)
{
	DelPlusMinus cal = (int fVal, int mVal, int lVal) => { return fVal + mVal + lVal; };
    Console.WriteLine(cal(5, 4, 1));
}
```

변수타입을 생략하고 더 간단히 표현
- 좌항의 델리게이트 타입을 참고하여 타입을 유추함
```java
delegate double DelPlusMinus(int fVal, int mVal, int lVal);
public static void Main(string[] args)
{
	DelPlusMinus cal = (fVal, mVal, lVal) => { return fVal + mVal + lVal; };
    Console.WriteLine(cal(5, 4, 1));
}
```
