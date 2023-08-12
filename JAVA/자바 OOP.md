### 인스턴스

: 클래스의 복제본

클래스를 인스턴스로 만들기 위해 앞에 new를 붙여준다

```java
public class Instance {

	public static void main(String[] args) {
		Print p1 = new Print();
		p1.delimiter = "----";
		p1.A();
		p1.A();
		p1.B();
		p1.B();
		
		Print p2 = new Print();
		p2.delimiter = "****";
		p2.A();
		p2.A();
		p2.B();
		p2.B();
		
		p1.A();
		p2.A();
		p1.A();
		p2.A();

	}

}
```

new를 이용하여 Print라는 데이터형(클래스)를 가진 p1라는 이름의 인스턴스를 만들고 인스턴스로 사용하기 위해 Print 클래스의 변수와 메소드들에서 static을 지워준다

이제 delimiter를 설정하고 "----"이 필요한 인스턴스의 경우 p1.A() 혹은 p1.B() 형식으로 사용

만약 delimiter가 "****"가 되어야 한다면 새롭게 p2라는 인스턴스를 만들고, delimiter를 설정한 후에

p2.A() 혹은 p2.B() 형식으로 사용

### 생성자와 this

생성자 : 인스턴스 선언시 내부의 세부 설정을 누락하지 않기 위해 또는 인스턴스 선언을 좀 더 간략히 하기 위해 인스턴스를 생성하려는 대상 클래스 내부에 만드는 메소드

```java
class 클래스 {
    public String 변수;
    public 클래스 (String 변수) { //인스턴스 생성시 static 혹은 void 관련 수식어 붙이지 않음
        this.변수 = 변수;
    }
}
```

this: 그 클래스가 인스턴스화 되었을 때 인스턴스를 가르키는 특수한 이름

```java
public class ConstructorAndThis {

	public String delimiter = "";
	public ConstructorAndThis(String delimiter) {
		this.delimiter = delimiter;
	}
	public void A() {
		System.out.println(this.delimiter);
		System.out.println("A");
		System.out.println("A");
	}
	
	public void B() {
		System.out.println(this.delimiter);
		System.out.println("B");
		System.out.println("B");
	}
}
```

### 활용

```java

public class AccountingApp {
	
    public static void main(String[] args) {
	    	Accounting.valueOfSupply = 10000.0;
        System.out.println("Value of supply : " + Accounting.valueOfSupply);
        System.out.println("VAT : " + Accounting.getVAT());
        System.out.println("Total : " + Accounting.getTotal());
 
				Accounting.valueOfSupply = 20000.0;
        System.out.println("Value of supply : " + Accounting.valueOfSupply);
        System.out.println("VAT : " + Accounting.getVAT());
        System.out.println("Total : " + Accounting.getTotal());
    }
 
}
```

하지만 10000원일 때의 공급가액을 출력하고, 20000원일 때의 공급가액을 출력한 뒤에

다시 10000원일 때의 부가가치세를 출력하고, 20000원일 때의 부가가치세를 출력하고

10000월일 때의 총 가격을 출력하고, 20000원일 때의 총 가격을 출력한다고 가정하면

계속 상태가 변하기에 valueOfSupply를 계속 바꿔주어야 한다.

이때 사용하는 것이 인스턴스

a1과 a2라는 Accounting 클래스의 인스턴스를 만들면 a1과 a2라는 독립적인 공간에 valueOfSupply가 저장한다

```java
class Accounting{
	public static double valueOfSupply;
	public static double vatRate = 0.1;
	public static double getVAT() {
		return valueOfSupply * vatRate;
	} 
  public static double getTotal() {
      return valueOfSupply + getVAT();
  }
}
public class AccountingApp {
	
    public static void main(String[] args) {
    	Accounting a2 = new Accounting();
			a2.valueOfSupply = 10000.0;

			Accounting a2 = new Accounting();
			a2.valueOfSupply = 20000.0;
    }
 
}
```

```java
class Accounting{
	// 공급가액
	public double valueOfSupply;
	// 부가가치세율
	public static double vatRate = 0.1;
	// 생성자
	public Accounting(double valueOfSupply) {
		this.valueOfSupply = valueOfSupply;
	}
	
	public double getVAT() {
		return valueOfSupply * vatRate;
	}
		
	public double getTotal() {
		return valueOfSupply + getVAT();
	}
}

public class AccountingApp {
	
	
	public static void main(String[] args) {
		
		Accounting a1 = new Accounting(10000.0);
		
		Accounting a2 = new Accounting(20000.0);
		
		System.out.println("Value of supply : " + a1.valueOfSupply);
		System.out.println("Value of supply : " + a2.valueOfSupply);

		System.out.println("VAT : " + a1.getVAT());
		System.out.println("VAT : " + a2.getVAT());
		
		System.out.println("VAT : " + a1.getTotal());
		System.out.println("VAT : " + a2.getTotal());

	}

}

```