# -const-let-bar-difference
Javascript에서 var ,let ,const의 차이점


ES5까지는 자바 스크립트에서 변수를 선언하는 유일한 기능이 var였다.

ES6이 출시후 추가된 변수들의 특징과 기존 var의 변수의 문제점을 정리해 보았다.

​

## 1. var 의 범위의 문제

​

함수를 선언 할때는 전역 또는 로컬 범위가 있다. 

함수 외부에서 선언되면 전역

=>전체 함수에 적용됨. 

반면에 함수 내부에 선언되면 로컬

=>해당 함수 내에서만 액세스할 수 있음

​

예를 들어

var foo = "foo";

function dummy(){

 var bar = "bar";

 } 

​

위의 변수 bar를 function dummy 안에서 선언했지만 전역변수 foo를 선언했기 때문에 변수 bar를 쓸 수 없다.

즉, 함수 외부의 변수에 액세스할 수 없다.

​

## 2. 호이스팅의 문제

​

console.log(foo); 

var foo = "foo"; 

이렇게 쓰면

​

var foo;

console.log(foo); 

//foo is undefined foo = "foo"; 

호이스팅 할때 오류가 생길수도 있다.

​

## 3. 중복 선언의 문제

​

예를들어

var foo = "bar is smaller"; 

var bar = 5;

if(bar > 3){ 

var foo = "bar is bigger";

 } 

console.log(foo); //bar is bigger 

​

따라서 위와 같은 오류로 인해 등장한 것이 let 과 const 이다.

​

​

'let' 선언

​

### 1.1 블록 레벨 스코프

​

대부분의 프로그래밍 언어는 블록 레벨 스코프(Block-level scope)를 따르지만 자바스크립트는 함수 레벨 스코프(Function-level scope)를 따른다.

​

블록 레벨 스코프(Block-level scope)

모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.

​

함수 레벨 스코프(Function-level scope)

함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 

즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.

​

var fruit = 'apple' //전역 변수

 console.log(fruit) //'apple'

 {

 var fruit = 'banana' // 전역 변수

 } 

console.log(fruit) //'banana'

​

위에서 처럼 블록 레벨 스코프가 아닌 var의 특성 상, 코드 블럭 내 fruit는 전역 변수이다.

그런데 이미 전역변수인 fruit가 선언되어 있다.

var키워드를 사용하여 선언한 변수는 중복 사용이 가능하므로 문법적으로는 아무 문제가 없다

또한 코드 블록 내의 선언한 var도 전역 변수이기 때문에 전역에서 선언된 전역 변수 fruit의 'apple'값에

새로운 'banana'값이 재할당되어 덮어쓴다.

​

let fruit = 'apple' //전역 변수

 console.log(fruit) //'apple' 

{

 let fruit = 'banana' // 지역 변수

 let food = 'pizza' // 지역 변수

 }

 console.log(fruit) //'apple'

 console.log(food) //'ReferenceError: bar is not defined'

let키워드를 사용한 변수는 블록 레벨 스코프를 따른다.

위 예제에서 보는거 처럼 코드 블록 내의 선언된 fruit는 블록 레벨 스코프를 갖는 지역 변수이기 때문에

전역에서 선언한 fruit와 다른 변수인다.

또한, food도 블록 레벨 스코프를 갖는 지역변수이다.

따라서 전역에서 food를 참조할 수 없다.

​

### 1.2 변수 중복 선언 금지

var 키워드로는 동일한 이름을 갖는 변수를 중복해서 선언할 수 있었다.

하지만, let 키워드로는 동일한 이름을 갖는 변수를 중복해서 선언할 수 없다.

변수를 중복 선언하면 문법 에러(SyntaxError)가 발생한다.

var num='123' 

var num='456' // 중복 선언 허용

​

 let Num='123'

 let Num='456' //Uncaught SyntaxError: Identifier 'Num' has already been declared

​

### 1.3 호이스팅

호이시트(hoist)는 건축/건설이나 화물 운반에 사용되는 장비로 화물 등을 들어올리는 업무를 수행한다.

즉, 아래에 위치한 것을 위로 끌어 올리는 역할을 하는 장비인데 이 단어 자체만으로도 '들어올리다'라는 의미를 가지고 있다.

Javascript에서 호이스팅(hoisting)은 코드에 선언된 let, const를 포함하여 모든 선언(var, let, const, function, function*, class) 을 코드 상단으로 끌어올리는 것을 말하며

이는 변수 범위가 전역 범위 인지 함수 범위인지에 따라 다르게 수행 된다.

함수 내에서 선언한 함수 범위(function scope)의 변수는 해당 함수의 최상위로

함수 밖에서 선언한 전역 범위(global scope)의 전역 변수는 스크립트 단위의 최상위로 끌어올려진다.

이때 var 키워드로 선언된 변수와는 달리 let 키워드로 선언된 변수를 선언문 이전에 참조하면 참조에러(ReferenceError)가 발생한다.

이는 let 키워드로 선언된 변수는 스코프의 시작에서 변수의 선언까지 일시적 사각지대(Temporal Dead Zone; TDZ)에 빠지기 때문이다.

console.log(fruit)//'undefined'

var fruit='apple'

 console.log(food)//'Error: Uncaught ReferenceError: bar is not defined' 

let food ='pizza'

​

그렇다면 이 차이는 왜 생기는 걸까? 차이를 알아보기전 변수가 어떻게 생성되고 호이스팅은 어떻게 이루어지는지 좀 더 살펴보자.

변수는 선언 -> 초기화 -> 할당 이라는 3단계에 걸쳐 생성된다.

​

##### 1. 선언 단계(Declaration phase)

변수 객체(Variable Object)를 등록한다. 이 변수 객체는 스코프가 참조하는 대상이 된다.

​

##### 2. 초기화 단계(Initialization phase)

변수 객체(Variable Object)에 등록된 변수를 위한 공간을 메모리에 확보한다.

이 단계에서 변수는 undefined로 초기화된다.

​

##### 3. 할당 단계(Assignment phase)

undefined로 초기화된 변수에 실제 값을 할당한다.

​

var로 선언된 변수는 선언,초기화 단계가 한번에 이루어진다.

즉, 스코프에 변수를 선언하고 메모리에 변수를 위한 공간을 확보한 후 undefined로 초기화된다.

따라서 변수 선언문 이전에 변수에 접근하여도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않는다.

다만 undefined를 반환하고 이후 변수 할당문에 도달하면 비로소 값이 할당된다.

이러한 현상을 변수 호이스팅(Variable Hoisting)이라 한다.


let 로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.

즉, 스코프에 변수를 선언하지만 초기화 단계는 변수 선언문에 도달했을때 이루어진다.

그래서 초기화 이전에 변수에 접근하려고 하면 참조 에러(ReferenceError)가 발생한다.

이는 변수가 아직 초기화 되지 않았기 때문이고, 다시 말하면 변수를 위한 메모리 공간이 확보되지 않았다이다.

따라서 스코프의 시작 지점부터 초기화 시작 지점까지는 변수를 참조할수 없다.

스코프의 시작 지점부터 초기화 시작 저짐꺼자의 구간을 '일시적 사각지대(Temporal Dead Zone;TDZ)라고 부른다.

// 스코프의 선두에서 선언 단계가 실행된다. // 아직 변수가 초기화(메모리 공간 확보와 undefined로 초기화)되지 않았다. // 따라서 변수 선언문 이전에 변수를 참조할 수 없다. console.log(fruit); // ReferenceError: fruit is not defined let fruit; // 변수 선언문에서 초기화 단계가 실행된다. console.log(fruit); // undefined fruit = 'apple'; // 할당문에서 할당 단계가 실행된다. console.log(fruit); // 'apple'


많은 사람들이 변수는 호이스팅이 되지 않는다고 생각하지만 그렇지않다.

let fruit = 'apple' // 전역 변수 

{ 

console.log(fruit) //ReferenceError: fruit is not defined

 let fruit = 'banana' // 지역 변수

 }

위와 같이 ES6의 선언문도 호이스팅이 발생하기 때문에 참조 에러(ReferenceError)가 발생한다.

ES6의 let으로 선언된 변수는 블록 레벨 스코프를 가지므로 코드 블록 내에서 선언된 변수 fruit는 지역 변수이다.

따라서 지역 변수 fruit도 해당 스코프에서 호이스팅되고 코드 블록의 선두부터 초기화가 이루어지는 지점까지 일시적 사각지대(TDZ)에 빠진다.

따라서 전역 변수 fruit의 값이 출력되지 않고 참조 에러(ReferenceError)가 발생한다.

​

​

'const' 선언

​

const 변수는 let과 매우 유사하지만 차이점은 const 로 선언되면 값이 상수화되어 *변경이 불가능하다. *

또한 const 로 선언될 경우 선언과 동시에 초기화를 해야 한다.

const fruit='apple'

console.log(fruit) // 'apple'

주의할 점은 const는 반드시 선언과 동시에 할당이 이루어져야 한다는 것이다.

그렇지 않으면 다음처럼 문법 에러(SyntaxError)가 발생한다

fruit='banana'

 console.log(fruit) // Uncaught TypeError: Assignment to constant variable.

​

또한, const는 let과 마찬가지로 블록 레벨 스코프를 갖는다.

{ 

const fruit = 'apple; 

console.log(fruit); // 'apple' 

}

 console.log(friut); // ReferenceError: fruit is not defined

​

## 정리

상황에 따라,어떤 변수 선언 방식을 써야할까?

var와 let, 그리고 const는 다음처럼 사용하는 것을 추천한다.

​

ES6를 사용한다면 var 키워드는 사용하지 않는다.

​

재할당이 필요한 경우에 한정해 let 키워드를 사용한다.

​

변경이 발생하지 않는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. 

​

따라서 변수를 선언할 때에는 일단 const 키워드를 사용한다.

반드시 재할당이 필요하다면(반드시 재할당이 필요한지 한번 생각해 볼 일이다.)

그때 const를 let 키워드로 변경해도 결코 늦지 않는다.

​
