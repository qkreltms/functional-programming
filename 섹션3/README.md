# 컬렉션(데이터 집합) 중심 프로그래밍의 4가지 유형과 함수
1. 수집하기 - map, ,keys, values, pluck, ...

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
_pluck: map과 마찬가지로 어떤 프로퍼티의 값을 수집한다, 단 더 간결하게

```js
function _pluck(data, key) {
    return _map(data, function(obj) {
        return obj[key]
    })
}

console.log(_pluck(users, 'age'))
```

2. 거르기 - filter, reject, compact, without, ...
3. 찾아내기 - find, some, every, ...
4. 줄이기 - reduce, min, max, group_by, count_by, ...
