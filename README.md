# Learning-react-grammar
> 리액트를 공부하는 데 처음 마주하는 Javascript와 react-native의 문법이나 함수 들을 정리합니다.


### 두꺼운 화살표 함수 ( => )
 > 빠른 모바일 앱 개발을 위한 React-Native 부록 A-5

+ ES5 호환 자바스크립트에서는 함수가 실행되는 context(예를 들어 this 변수)를 명확히 하기 위해 함수 실행 시 bind명령을 이용하곤 한다.
+ callback을 다룰 때 특히 많이 사용한다.


= 수동으로 함수 바인딩

```
var callbackFunc = function(val) {
    console.log('Do Something');
}.bind(this);
```

= 바인딩을 위해 두꺼운 화살표 함수 사용

```
var callbackFunc = (val) => {
    console.log('Do Something');
};
```


