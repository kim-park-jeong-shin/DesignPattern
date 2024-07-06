### 1주차 디자인 패턴  소개와 전략 패턴
  
***
## 오리 클래스 
- 상황 : 객체지향 프로그래밍으로 오리들을 구현했는데 나는 동작과 우는 동작의 수정 요청이 들어옴
- 해결 방안 : 슈퍼클래스에 fly 메서드를 추가하여 기능을 구현함
- 문제점 : 모든 서브클래스에 fly 메서드가 적용되면서 일부 서브클래스에 의도하지 않은 행동이 추가됨

- 해결 방안 : 각 서브클래스에 fly 메서드를 오버라이드하여 기능을 수정함
- 문제점 : 정기적으로 업데이트 해야하는 내용일 경우 매번 모든 서브 클래스의 fly메서드를 일일이 살펴보고 오버라이드 해야하는 번거로움 발생

- 해결 방안 : fly메서드를 슈퍼클래스에서 빼고 fly 메서드가 들어있는 Flyalble 인터페이스를 만들어거 날 수 있는 오리에게만 인터페이스를 구현하여 넣어준다.
- 문제점 : 코드 중복, 날아가는 동작을 조금 바꾸기 위해 날아갈 수 있는 모든 서브클래스의 코드를 수정해야함. -> (두 번째 문제점은 무슨 말인지 이해가 어려움)

***
###### 디자인 원칙 1 : 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분과 분리한다.
	-> 달라지지 않는 부분을 찾아서 '캡슐화'한다.(시스템의 유연성 향상)
 따라서 Duck 클래스에서 나는 행동과 우는 행동을 별도의 클래스 집합으로 구현한다.

###### 디자인 원칙 2 : 구현보다는 인터페이스에 맞춰서 프로그래밍한다.
	-> Duck의 행동을 특정 행동 인터페이스를 구현한 별도의 클래스 안에 넣어서 Duck 클래스에서 그 행동을 구체적으로 구현할 필요가 없게 한다.
 인터페이스에 맞춰서 프로그래밍한다 = 상위 형식에 맞춰서 프로그래밍한다.
1. 인터페이스에 맞춰서 프로그래밍하는 코드
	<code>
	// 인터페이스에 맞춰서 프로그래밍 하는 코드
	Dog d = new Dog();
	d.bark();

	// 상위 형식에 맞춰서 프로그래밍 하는 코드
	Animal animal = new Dog();
	animal.makeSound();
	</code>

- ###### 가장 중요한 점은 나는 행동과 꽥꽥거리는 행동을 Duck 클래스에서 정의한 메서드를 써서 구현하지 않고 다른 클래스에 위임한다는 것이다.

* 오리 클래스에 구현 방법
	- Duck 클래스에 flyBehavior. quackBehavior라는 두 인터페이스 형식의 인스턴스 변수를 추가한다.
	- 각 오리 객체에서는 실행시에 이 변수에 특정 행동 양식의 레퍼런스를 다형적으로 설정한다.

* 실제 코드 구현

<code>
#include <iostream>

class	FlyBehavior
{
	public:
	virtual void	fly() = 0;
};

class	QuackBehavior
{
	public:
	virtual void	quack() = 0;
};

class	Duck
{
private:
	FlyBehavior		*flybehavior;
	QuackBehavior	*quackBehavior;
public:
	Duck(FlyBehavior *fly, QuackBehavior *quack) {
		flybehavior = fly;
		quackBehavior = quack;
	}
	void	hello()
	{
		std::cout<<"hello. I'm Duck."<<std::endl;
	}
	void	swim()
	{
		std::cout<<"every duck can swim!!"<<std::endl;
	}
	void	performFly()
	{
		flybehavior->fly();
	}
	void	performQuack()
	{
		quackBehavior->quack();
	}
};

class Quack : public QuackBehavior
{
public:
	void	quack()
	{
		std::cout<<"Quack"<<std::endl;
	}
};

class MuteQuack : public QuackBehavior
{
public:
	void	quack()
	{
		std::cout<<"--Mute--"<<std::endl;
	}
};

class FlyWithWings : public FlyBehavior
{
public:
	void	fly()
	{
		std::cout<<"I'm flying!!"<<std::endl;
	}
};

class FlyNoWay : public FlyBehavior
{
public:
	void	fly()
	{
		std::cout<<"I can't fly!!"<<std::endl;
	}
};

class	MallardDuck : public Duck
{
public :
	MallardDuck() : Duck(new FlyWithWings(), new Quack()){}
};

int main()
{
	Duck	*mallard = new MallardDuck();
	mallard->performFly();
	mallard->performQuack();
}
</code>

- setFlyBehavior(Flybehavior fd) 이러한 세터 메서드를 통해서 동적으로 행동을 지정하는 방식도 있다.
- 여기서는 Duck클라이언트에서 캡슐화된 행동으로 FlyBehavior, QuackBehavior 을 두었고, 각 행동을 알고리즘군으로 분류할 수 있다.

###### 디자인 원칙 3 : 상속보다는 구성을 활용한다.
-> 알고리즘 군을 별도의 클래스 집합으로 캡슐화할 수 있으며, 동적으로 행동을 변경할 수도 있다.(유연성 향상)
- 구성 : 두 클래스를 합치는 것


