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

function _curryr(fn){
  return function(a,b){
    return arguments.length === 2 ? fn(b,a) : function(b){return fn(b,a);};
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
  _each(list, function (fn) {
    memo = iter(memo, fn);
  });
  return memo;
}

console.log(_reduce([1, 2, 3, 4], add, 0)); // 10
```

# 파이프라인, _go, _pipe, 화살표 함수
## _pipe
함수를 연속적으로 실행할 수 있게 함.
```js
function _pipe() {
  var fns = arguments;
  return function(arg) {
    return _reduce(fns, function(arg, fn) {
      return fn(arg)
    }, arg)
  }
}

// 함수 인자를 넣고, 이 순서대로 실행된다. 
var f1 = _pipe(
  function(a) { return a + 1 },
  function(b) { return b + 2 }
);

f1(1) // 4
```
# _go
```js
_go(1, 
function(a) { return a + 1 },
function(a) { return a + 2 },
function(a) { return a + 3 },
console.log
) // 7

function _go(arg) {
  return _pipe.apply(null, Array.prototype.slice.call(arguments).slice(1))(arg)
}
```

# curry_r을 사용하면 더 간결하게 사용 가능
```js
var _map = _curryr(_map)
var _filter = _curryr(_filter)
```
# 정리
```js
var users = [{ age: 10, name: '1' }, { age: 31, name: '2' }]
var temp_users = []
for (var i = 0; i < users.length; i++) {
  if (users[i].age >= 30) {
    temp_users.push()
  }
}

for (var i = 0; i < temp_users.length; i++) {
  console.log(temp_users['name'])
}
```
위의 코드가 아래의 코드로 변경됐다.
```js
var _map = _curryr(_map)

_go(users,
_filter(user => user.age < 30),
_map(_get('name')),
console.log)


_go([1,2,3], 
// curryr을 사용해 함수가 첫번째 인자로 들어올 수 있도록 해준다.
// 하지만 결국에 map에 들어오는 인자순서는 그 형식에 맞게 들어오게 함.(list, mapper)
_map(n => n+1),
console.log
)
```