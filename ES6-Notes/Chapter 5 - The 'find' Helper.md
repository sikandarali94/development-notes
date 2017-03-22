# The 'find' Helper

### Querying For Records With Find

* It is very common to use a for loop to query data from an array of objects. Here is an example:

```js
var users = [
    { name: 'Jill' },
    { name: 'Alex' },
    { name: 'Bill' }
];

var user;

for (var i = 0; i < users.length; i++) {
    if (users[i].name === 'Alex') {
        user = users[i];
        break;
    }
}
console.log(user);
```

![](/assets/Screen Shot 2017-03-21 at 7.26.54 am.png)

* With the `find` helper we walk through every element in the array. For each element in the array we pass it to the Iterator Function and that Iterator Function must return a truthy or falsy value. The `find` helper will keep calling elements with the Iterator Function until the Iterator Function eventually returns `true`. Once it returns `true` the find helper immediately exits its iterator returning the record it had found.
* Here is how we code with the find helper:

```js
user = users.find(function(user) {
    return user.name === 'Alex';
});
console.log(user);
```

* One drawback of the `find` helper is that it will return only one and first instance of an element that we queried. Say we have two names of Alex in the array; find will return the first instance of name Alex in the array and step out of iterating through the elements in the array.
* Here is a complex example of using the `find` helper:

```js
var posts = [
    { id: 1, title: 'New Post' },
    { id: 2, title: 'Old Post' }
];

/*
    Here we want to extract the single record from posts array whose
    id matches the postId in comment.
*/
var comment = { postId: 1, content: 'Great Post' };

function postForComment(posts, comment) {
    return posts.find(function(post) {
        return post.id === comment.postId;
    });
}

console.log(postForComment(posts, comment));
```

### Using Find To Search For Users

* `find` is great for looking for data that a user has queried in the Address URL.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

