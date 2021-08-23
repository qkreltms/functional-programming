```js
// 순수 함수
function add(a, b) {
    return a+b
}
add(1,2) // 부수 효과가 없다. 입력이 같으면 동일한 결과가 나온다.

var obj1 = { val: 10 }
function add5(obj, b) {
    return { val: obj.val + b } // 새로운 객체를 생성해 기존 값을 변경하지 않는다.
}
add5(obj1, 20)

/**
 순수 함수가 아님
 **/
var c = 10 // c가 바뀌면 값이 변경된다.
function add2(a, b) {
    return a+b+c
}
add2(1,2)
c = 20
add2(1,2) // c가 바뀌면서 리턴값이 변경된다.

var c = 20
function add3(a, b) {
    c = b
    return a+b
}
console.log(c)
add3(1,2)
console.log(c)

var c = { val: 10 }
function add4(obj, b) {
    obj.val += b
}
console.log(c.val)
add4(c, 20)
console.log(c.val)
```

함수형 프로그래밍은 어플리케이션, 함수의 구성요소, 더 나아가서 언어 자체를 함수처럼 여기도록 만들고, 이러한 함수 개념을 가장 우선순위에 놓는다.

함수형 사고방식은 문제의 해결 방법을 동사(함수)들로 구성(조합)하는 것.

-마이클 포커스 [클로저 프로그래밍의 즐거움]에서-

객체지향 프로그래밍
```js
duck.moveLeft()
```

함수형 프로그래밍
```js
moveLeft(duck)
```