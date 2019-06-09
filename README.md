# Learning-react-grammar
> 리액트(네이티브)를 공부하면서 필요한 지식들을 정리합니다. (ES6, React, JS, etc...)

<br/><br/>
## 두꺼운 화살표 함수 ( => )
 > 빠른 모바일 앱 개발을 위한 React-Native 부록 A-5

* Arrow function은 새로운 ES6의 기능
* ES5 호환 자바스크립트에서는 함수가 실행되는 context(예를 들어 this 변수)를 명확히 하기 위해 함수 실행 시 bind명령을 이용하곤 한다.
* callback을 다룰 때 특히 많이 사용한다.

( ? callback 함수 : 콜백 함수란 1. 다른 함수의 인자로써 이용되는 함수. 2. 어떤 이벤트에 의해 호출되어지는 함수. )

참고 : [CallBack함수의 정확한 의미는 무엇일까?](https://satisfactoryplace.tistory.com/18)

<br/>


  * 수동으로 함수 바인딩

```javascript
var callbackFunc = function(val) {
    console.log('Do Something');
}.bind(this);
```

  * 바인딩을 위해 두꺼운 화살표 함수 사용

```javascript
var callbackFunc = (val) => {
    console.log('Do Something');
};
```

* 추가적인 예시

```javascript
// regular function
const testFunction = function() {
  // content..
}

// arrow function
const testFunction = () => {
  // content..
}
```

* parameters의 전달에는 여전히 괄호가 사용되며, parameter가 한 개라면 괄호가 생략될 수 있다.

```javascript
const testFunction = (firstName, lastName) => {
  return firstName+' '+lastName;
}

const singleParam = firstName => {
  return firstName;
}
```

* implicit return
> arrow function이 오직 한 줄이라면 return 키워드 없이 사용할 수 있다.

```javascript
const testFunction = () => 'hello there.';
testFunction(); 
```

* Use in React
> React component를 생성하는 또 다른 방법 ( arrow function을 사용하여 ) <br/>
> 하단의 두 코드는 동등한 수준의 코드

```javascript
const HelloWorld = (props) => {
  return <h1>{props.hello}</h1>;
}
```
```javascript
class HelloWorld extends Component {
  render() {
    return (
      <h1>{props.hello}</h1>;
    );
  }
}
```

* arrow function의 사용은 코드를 간결하게 해주지만 component의 state사용을 제거한다.
> stateless functional component


<br/><br/>
## Promise
 > 빠른 모바일 앱 개발을 위한 React-Native 부록 A-8

<br/>

* Promise는 나중에 실행되는 무언가를 나타내는 객체라 할 수있다. 성공과 실패 콜백 처리를 직접 구현할 필요가 없도록 promise는 비동기 연산을 대응하기 위한 API를 제공한다. 

<br/>

1) 성공 콜백
```javascript
function successCallback(result) {
  console.log("It succeeded: ", result);
}
```

2) 실패 콜백
```javascript
 function errorCallback(error) {
   console.log("It failed: ", error);
 }
```
 
* 예전 방식의 자바스크립트에서 사용하는 성공과 실패 콜백을 넘기기
```javascript
uploadToSomeAPI(successCallback, errorCallback);
```

* 성공과 실패 콜백을 promise에 넘기기
```javascript
uploadToSomeAPI().then(successCallback, errorCallback);
```

#### 위의 두 가지 예는 비슷해 보이지만 콜백이나 실행해야 할 비동기 연산이 많을 경우 그 차이가 뚜렷해진다.

1) callback hell
> 여러 콜백을 연속적으로 연결하면 코드가 지저분해지고 반복적으로 작성하는 코드가 많아진다.

```javascript
uploadToSomeAPI(
 (result) => {
   updateUserInterface(
     result,
     uiUpateResult => {
       checkForNewData(
         uiUpateResult,
         newDataResult => {
           successCallback(newDataResult);
         },
         errorCallback
       );
     },
     errorCallback
   );
 }, errorCallback
);
```

2) promise를 이용하여 여러 동작을 간단히 연결
```javascript
uploadToSomeAPI()
  .then(result => updateUserInterface(result))
  .then(uiUpdateResult => checkForNewData(uiUpdateResult))
  .then(newDataResult => successCallBack(newDataResult))
  .catch(errorCallBack)
```
<br/><br/>
# 프레임워크와 라이브러리

- 둘은 비슷하지만 다른 개념입니다. 둘다 개발에서 반복되는 작업을 줄여주는 역할을 한다는 점에서는 같습니다. 하지만 사용할 때 정해진 방식대로 코드를 짜도록 강요받는 것이 프레임워크, 그때그때 필요할 때 가져다 쓸수 있는 것이 라이브러리입니다.

    (여러분이 배우는건 프레임워크니까 이건 왜 이렇게 되지 고민하는 자세도 중요하지만 그냥 이렇게 하는 거구나 라며 넘어갈 줄도 아는 자세를 가져야합니다..)

출처 : [https://www.notion.so/4eed5a2343bb4f09874fe6c56ea4ace8?v=138c8b8b488e42b6a2cc603714db9e4f]
 
 <br/><br/>
# Virtual DOM
> 화면이 어떤 모습이어야 하는지 개발자가 작성한 내용과 실제 화면에 렌더링되는 것 사이에 존재하는 레이어

* 브라우저에서 상호작용하는 사용자 인터페이스를 렌더링하기 위해서 개발자는 반드시 브라우저의 DOM(Document Object Model)을 수정해야한다.
  -> 이는 값비싼 동작으로, 과도한 DOM 수정은 심각한 성능 저하를 유발한다.
  
* 리액트는 페이지의 변화를 바로 렌더링하지 않고 먼저 메모리에 존재하는 가상 DOM에서 변화가 필요한 곳을 계산하고 필요한 최소한의 변경사항만 렌더링한다. 

<br/><br/>
# Life Cycle
> 


<br/><br/>
# JavaScript Basics Before You Learn React Summary
> 리액트를 공부하기 전에 알아야 할 필수적인 자바스크립트 기본 지식 정리

## ES6 Classes

```javascript
class Developer {
  constructor(name){
    this.name = name;
  }

  hello(){
    return 'Hello World! I am ' + this.name + ' and I am a web developer';
  }
}
```


```javascript
var nathan = new Developer('Nathan');
nathan.hello(); // Hello World! I am Nathan and I am a web developer
```

<br/><br/>

## Class inheritance
> 클래스는 다른 클래스의 정의를 확장할 수 있으며, 클래스에서 초기화된 새로운 개체는 두 클래스의 모든 방법을 가진다.

```javascript
class ReactDeveloper extends Developer {
  installReact(){
    return 'installing React .. Done.';
  }
}

var nathan = new ReactDeveloper('Nathan');
nathan.hello(); // Hello World! I am Nathan and I am a web developer
nathan.installReact(); // installing React .. Done.

```

<br/>

### overriding 

```javascript
class ReactDeveloper extends Developer {
  installReact(){
    return 'installing React .. Done.';
  }

  hello(){
    return 'Hello World! I am ' + this.name + ' and I am a REACT developer';
  }
}

var nathan = new ReactDeveloper('Nathan');
nathan.hello(); // Hello World! I am Nathan and I am a REACT developer

```

<br/><br/>

## Use in React
> 이것은 리액트 컴포넌트지만, 사실 리액트 패키지에서 가져온 리액트 컴포넌트 클래스의 정의를 계승하는 일반적인 ES6 클래스일 뿐이다.

```javascript

import React, { Component } from 'react';

class App extends Component {
  // class content
  render(){
    return (
      <h1>Hello React!</h1>
    )
  }
}

```

* 그러나 나중에 알게 되겠지만, Class만이 리액트 컴포넌트를 정의하는 유일한 방법은 아니다. 
* state 및 다른 life cycle 방법이 필요하지 않으면 대신 function 을 사용할 수 있다.

<br/>

## Declaring variables with ES6 let and const
> JS에서의 var 키워드 변수 선언은 전역적이기 때문에, 2개의 새로운 방법이 ES6에서 소개되었다.
> let과 const는 이러한 이슈들을 해결한다.

```javascript
const name = "David";
let age = 28;
var occupation = "Software Engineer";
```

* let과 const의 차이점 : const는 선언 이후 값의 변화를 허용하지 않는다 / let은 허용한다.
* 둘다 local declaration이다. function scope 내부에서 선언했다면 외부에서 호출할 수 없다.

<br/>

### rule of thumb

* 기본적인 선언은 const로 한다. 이후 application을 작성할 때 값에 변화를 주어야 한다면 그 때 let으로 refactor한다.

EX)
```javascript
import React, { Component } from 'react';

class App extends Component {
  // class content
  render(){
    const greeting = 'Welcome to React';
    return (
      <h1>{greeting}</h1>
    )
  }
}
```

### React에서의 사용

```javascript
import React, { Component } from 'react';

class App extends Component {
  // class content
  render(){
    const greeting = 'Welcome to React';
    return (
      <h1>{greeting}</h1>
    )
  }
}
```

## Destructuring assignment for arrays and objects
> 구조 분해 할당

* 구조분해 할당은 간단히 array나 object의 일부분을 복사한다.

```javascript
const developer = {
  firstName: 'Nathan',
  lastName: 'Sebhastian',
  developer: true,
  age: 25,
}

//destructure developer object
const { firstName, lastName } = developer;
console.log(firstName); // returns 'Nathan'
console.log(lastName); // returns 'Sebhastian'
console.log(developer); // returns the object
```

* 새로운 변수에 할당.
> firstName을 name이라는 새로운 변수에 넣는다.

```javascript
const { firstName:name } = developer;
console.log(name); // returns 'Nathan'
```

* 구조분해는 array에게도 동작한다.

```javascript
const numbers = [1,2,3,4,5];
const [one, two] = numbers; // one = 1, two = 2
```

* ,를 사용한 구조분해를 통해 index를 skip할 수 있다.

```javascript
const [one, two, , four] = numbers; // one = 1, two = 2, four = 4
```

### Use in React

* method에서 구조분해 state가 가장 많이 사용된다.

```javascript
reactFunction = () => {
  const { name, email } = this.state;
};
```

* functional stateless component에서 

```javascript
const HelloWorld = (props) => {
  return <h1>{props.hello}</h1>;
}

```

* 아래와 같이 비구조화 하여 사용할 수 있다.
```javascript
const HelloWorld = ({ hello }) => {
  return <h1>{hello}</h1>;
}

```

## Map and filter
> 데이터 처리에 특히 많이 이용된다.

```javascript
const users = [
  { name: 'Nathan', age: 25 },
  { name: 'Jack', age: 30 },
  { name: 'Joe', age: 28 },
];
```

* 아래와 같이 map method를 사용하여 리액트에서 아이템들의 리스트를 렌더링할 수 있다. 

```javascript
import React, { Component } from 'react';

class App extends Component {
  // class content
  render(){
    const users = [
      { name: 'Nathan', age: 25 },
      { name: 'Jack', age: 30 },
      { name: 'Joe', age: 28 },
    ];

    return (
      <ul>
        {
           users.map(user => <li>{user.name}</li>)
        }
      </ul>
    )
  }
}
```
* filter를 사용하여 렌더링 할 수 있다.
```javascript
<ul>
  {users
    .filter(user => user.age > 26)
    .map(user => <li>{user.name}</li>)
  }
</ul>
```

(?) map method example

```javascript
var array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]

```

## ES6 module system
> ES6 모듈 시스템은 자바스크립트가 파일을 가져오고 내보낼 수 있게 해준다.

* 네이밍된 export는 모듈당 여러 개를 가질 수 있다.
* 하지만 export default는 오직 하나만 있을 수 있다.

```javascript


```
```javascript


```
```javascript


```
(?) curly braces : {} (괄호)













