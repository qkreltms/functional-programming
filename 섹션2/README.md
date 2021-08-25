# map, filter, each
```js
function _filter(list, predi) {
  var new_list = [];
  _each(list, function(val) {
    if (predi(val)) new_list.push(val);
  });
  return new_list;
}

function _map(list, mapper) {
  var new_list = [];
  _each(list, function(val) {
    new_list.push(mapper(val))
  });
  return new_list;
}

function _each(list, iter) {
  for (var i = 0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}
```

# 다형성
하나의 객체가 여러 가지 타입을 가질 수 있는 것을 말한다.

함수형 프로그래밍에서 내부 다형성: 위에서 predi, mapper, iter

함수가 먼저 나오고 그 다음에 데이터가 나온다.

외부 다형성:
array_list, arguments, document.querySelectorAll

데이터가 먼저 나오고 그 다음 함수가 나온다.
```js
[1,2,3,4].map(() => { ... })
```

# 커링
```js
 function _curry(fn) {
   return function(a, b) {
     return arguments.length === 2 ? fn(a, b) : function(b) { return fn(a, b) }
   }
 }

 var add = _curry(function(a, b) {
   return a + b
 })
 add(10)(5) // 15
 add(10, 5) // 15
``` 
```js
 var _get = _curry(function(obj, key) {
   return obj === null ? undefined : obj[key]
 })
var user = {
  age: 12
}

 _get(user)('age')
``` 

# _reduce 만들기
```js
function _reduce(list, iter, memo) {
  _each(list, function (val) {
    memo = iter(memo, val);
  });
  return memo;
}

console.log(_reduce([1, 2, 3, 4], add, 0)); // 10
```