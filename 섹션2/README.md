```js

var users = [ 
{id: 1, name: 'ID', age: 35},
{id: 2, name: 'ID1', age: 34},
{id: 3, name: 'ID2', age: 33},
{id: 4, name: 'ID3', age: 32},
{id: 5, name: 'ID4', age: 31}
]

var temp_users = [];
for (var i = 0; i < users.length; i++) {
    if (users[i].age >= 30) {
        temp_users.push(users[i])
    }
}
conosle.log(temp_users)
```