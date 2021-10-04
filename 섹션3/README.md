# 컬렉션(데이터 집합) 중심 프로그래밍의 4가지 유형과 함수
## 1. 수집하기 - map ,keys, values, pluck, ...

map: 어떤 프로퍼티의 값을 수집한다.

value: 값을 수집한다.
```js
var users = [{id: 10, name: '1', age: 1}, {id: 20, name: '2', age: 2}]
function _values(data) {
    return _map(data, function(val) {
        return val
    })
}

console.log(_values(users[0]), _map(users[0], function(val) { return val }))
/**
[10,"1",1] // [object Array] (3)
[10,"1",1]
**/

function _is_object(obj) {
  // 참고: typeof [] === 'object'
  return typeof obj === 'object' && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : []; 
}

function _map(list, mapper) {
  var new_list = [];
  _each(list, function(val) {
    new_list.push(mapper(val))
  });
  return new_list;
}

function _each(list, iter) {
  console.log(_keys(list)) // object.length 가 없으므로 keys로 대체한다.
  var keys = _keys(list)
  for (var i = 0; i < keys.length; i++) {
    iter(list[keys[i]]);
  }
  return list;
}
```
_pluck: map과 같음, 단 더 간결하게

```js
function _pluck(data, key) {
    return _map(data, function(obj) {
        return obj[key]
    })
}

console.log(_pluck(users, 'age'))
```

## 2. 거르기 - filter, reject, compact, without, ...

### filter
조건문에 속하는 값들만 반환한다.
```js
function _filter(list, predi) {
  var new_list = [];
  _each(list, function(val) {
    if (predi(val)) new_list.push(val);
  });
  return new_list;
}
```
```js
// 조건문에 속하는 값만 반환한다.
_filter(users, function(usere) {
  return user.age > 30
})

// 조건문에 속하는 값을 제외한다.(filter와 반대)
_reject(users, function(user) {
  return user.age > 30
})
```

### reject 함수
조건문에 속하는 값들을 제외한다.(filter와 반대)
```js
function _reject(data, predi) {
  return _filter(data, function(val) {
    return !predi(val)
  })
}
```

reject함수는 negate함수를 사용하면 더 간결해질 수 있다.
```js
// 결과를 반대로 바꾸는 함수
function _negate(func) {
  return function(val) {
    return !func(val)
  }
}

function _reject(data, predi) {
  return _filter(data, _negate(predi))
}
```
### compact
배열에서 Truthy(falsee, 0, -0, 0n, "", null, undefined, NaN가 아닌 값)인 값만 걸러주는 함수 
```js
// 긍정적인 값만 남기는 함수
_compact([1,2,0,false,null, {}]) // [1,2, {...}]
```

```js
function _identity(val) {
  return val
}

// itentity함수가 호출되면 filter 내부의 에서 함수 호출 되고 그 결과값 중 truthy인 값은 if 문을 통과하게 된다.
var _compact = _filter(_identity)
```

## 3. 찾아내기 - find, some, every, ...
### 1. find 
컬렉션에서 조건에 따라 먼저 찾은 값을(하나만) 반환한다.
```js
function _find(list, iter) {
  /**
  function _keys(obj) {
    return _is_object(obj) ? Object.keys(obj) : []; 
  }
  **/
  var keys = _keys(list) // 무조건 배열로 나온다.
  for (var i = 0, len=keys.length; i<len; i++) {
    var val = list[keys[i]] 
    if (perdi(list[keys[i]])) return val
  }
}

console.log(_find(users, function(user) {
  return user.age < 30
})) // 반환: { id: 40, name: 'a', age: 29}
```

### 2. find_index
find와 같지만 인덱스를 반환한다.

```js
function _find_index(list, iter) {
  /**
  function _keys(obj) {
    return _is_object(obj) ? Object.keys(obj) : []; 
  }
  **/
  var keys = _keys(list) // 무조건 배열로 나온다.
  for (var i = 0, len=keys.length; i<len; i++) {
    if (perdi(list[keys[i]])) return i
  }
}

// 또 다른 방법
// #1
_get(_find(users, function(user) {
  return user.id === 50
}), 'name')

// #2
// curry를 사용해야 _go 안에서 사용 가능하다.
var _find = _curryr(function(list, pred) {
  var keys = _keys(list) // 무조건 배열로 나온다.
  for (var i = 0, len=keys.length; i<len; i++) {
    var val = list[keys[i]] 
    if (perdi(list[keys[i]])) return val
  }
})

_go(users,
_find(function(user) {
  return user.id === 50
}), 
_get('name')),
console.log
```
4. 줄이기 - reduce, min, max, group_by, count_by, ...
