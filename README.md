# Learning-react-grammar
> 리액트를 공부하는 데 처음 마주하는 Javascript와 react-native의 문법이나 함수 들을 정리합니다.

<br/><br/>
## 두꺼운 화살표 함수 ( => )
 > 빠른 모바일 앱 개발을 위한 React-Native 부록 A-5

* ES5 호환 자바스크립트에서는 함수가 실행되는 context(예를 들어 this 변수)를 명확히 하기 위해 함수 실행 시 bind명령을 이용하곤 한다.
* callback을 다룰 때 특히 많이 사용한다.

<br/><br/>
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


 
 
 
