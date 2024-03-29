주 생성자(Primary constructor)와 초기화 블록
코틀린의 클래스는 하나의 주 생성자(primary constructor)와 다수의 부 생성자(secondary constructor)를 가질 수 있습니다. 여기서 constructor 키워드는 주 생성자나 부생성자 정의를 시작할 때 사용됩니다.
주 생성자는  생성자 파라미터를 지정하고 그 생성자 파라미터에 의해 초기화 되는 프로퍼티를 정의하는 두가지 목적에 쓰입니다.



//파라미터가 하나만 있는 주 생성자
class User constructor(_nickname: String){
	val nickname: String
    
    //초기화 블록
    init {
    	nickname = _nickname
   	}
}
생성자 내부 argment 타입은 var로 지정하지 않는 한 val 입니다.

사실 생성자 내부 argment뿐 아니라 Kotlin은 기본적으로 val로 설정합니다.

이유는 

위 코드에서 사용된 init 키워드는 초기화 블록으로 불리며, 클래스의 객체가 인스턴스화 될 때 실행된 초기화 코드가 들어갑니다.

주 생성자는 제한적이기 때문에 별도의 코드를 포함할 수 없으므로 초기화 블록이 필요합니다. 또한 필요하다면 클래스 안에 여러 초기화 블록을 선언할 수 있습니다.



생성자 파라미터 _nickname에서 맨 앞의 밑줄(_)은 프로퍼티와 생성자 파라미터를 구분해 줍니다.

자바에서 쓰는 방식처럼 this. 를 사용하여 모호성을 없애주는 것도 가능합니다.



this.nickname = nickname


주 생성자 앞에 별다른 애노테이션이나 가시성 변경자가 없다면 constructor를 생략해도 됩니다.

그리고 주 생성자의 파라미터로 초기화 할 수 있으며, 이런 방식으로 프로퍼티를 초기화 한다면 주 생성자 앞에 val 를 추가하는 방식으로 프로퍼티 정의와 초기화를 간략히 쓸 수 있습니다.



//생성자 파라미터에 대한 디폴트 값을 정의할 수 있다.
class User(val nickname: String, val isSubscribed: Boolean = true)


모든 생성자 파라미터에 디폴트 값을 지정하면 컴파일러가 자동으로 파라미터가 없는 생성자를 만들어 줍니다. 그렇게 자동으로 만들어진 프라미터가 없는 생성자는 디폴트 값을 사용해 클래스를 초기화 합니다.

의존관계 주입(DI, Dependency Injection) 프레임워크 등 자바 라이브러리 중에는 파라미터가 없는 생성자를 통해 객체를 생성해야만 라이브러리를 사용이 가능한 경우가 있는데, 코틀린이 제공하는 파라미터 없는 새성자는 그런 라이브러리와의 통합을 쉽게 해줍니다.



클래스에 기반 클래스가 있다면, 주 생성자에서 기반 클래스의 생성자를 호출해야 할 필요가 있습니다.



open class User(val nickname: String) {...}
class InstagramUser(nicknameL String) : User(nickname) {...}
여기서 기반 클래스란? 구현하고자 하는 클래스간에 계층관계가 성립하면 상위 레벨에 있는 클래스를 먼저 구현하고 하위 레벨에 있는 클래스는 상위  클래스에 있는 모든 특성을 전달 받아 만들 수 있습니다.



코틀린에서 (:) 세미콜론은 상속을 받을 때, 변수 타입을 명시할 때 사용합니다. 하지만 띄어쓰기 방식이 다릅니다.

타입을 지정해주는 경우 [변수명: 타입] 으로 변수명과 세미콜론을 붙여 사용하고 상속을 받는 경우 [하위 클래스 : 상위 클래스] 으로 하위 클래스와 상위 클래스 사이 세미콜론 간격에 띄어쓰기가 모두 들어갑니다.



추가로 클래스를 상속 받을땐 생성자 파라미터가 존재하지 않더라도 기반 클래스 뒤에 괄호()를 붙입니다.

이유는, 클래스를 정의할 때 별도로 생성자를 정의하지 않으면 컴파일러가 자동으로 아무일도 하지 않는 인자가 없는 디폴트 생성자를 만들어 주기 때문입니다. 인터페이스의 경우 괄호 없이 작성해 주면 됩니다. 그래서 괄호를 살펴보면 기반 클래스와 인터페이스를 구분할 수 있습니다.



부 생성자(secondary constructor) 상위 클래스를 다른 방식으로 초기화는 법
코틀린에서는 생성자가 여럿 있는 경우가 자바보단 훨씬 적지만, 그래도 필요한 경우가 가끔 있습니다.



가장 일반적인 상황 : 프레임워크를 확장해야 하는데 여러가지 방법으로 인스턴스를 초기화 할 수 있게 다양한 생성자를 지원해야하는 경우(자바에서 생성자가 2개인 경우) 상위 클래스의 생성자에게 객체 생성을 위임합니다.



open class View {

	//상위 클래스의 생성자를 호출한다.
	constructor(context: Context) 
    	: super(context) { 
        	...
        } 
    
	constructor(context: Context, attributeSet: AttributeSet) 
    	: super(context, attributeSet) { 
        	... 
        }
}


만약 주 생성자 안의 부 생성자를 호출하게 된다면 주 생성자 내부의 함수가 호출된 다음 부 생성자가 호출됩니다.

하지만 코틀린에서는 부 생성자 보다는 default 파라미터를 권장합니다.