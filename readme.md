# Collaborative Filtering Simple Example

# Install

```
npm install collaborative-filtering
```

# Usage

```javascript
const CF = require('collaborative-filtering');
const fs = require('fs');

// load movie dataset: user_id movie_id rating like
const movie = JSON.parse(fs.readFileSync(__dirname + "/test.json", "utf-8"));

// split data as train & test
let train = [], test = [];
for (let i = 0; i < movie.length; i++) {
    if (Math.random() > 0.8) test.push(movie[i]);
    else train.push(movie[i]);
}

// set data
const cf = new CF();

cf.maxRelatedItem = 1000;
cf.maxRelatedUser = 100;

cf.train(train, 'user_id', 'movie_id', 'rating');

// select 100 data for recommendation
let gt = cf.gt(test, 'user_id', 'movie_id', 'rating');
let gtr = {};
let users = [];
for (let user in gt) {
    gtr[user] = gt[user];
    users.push(user);
    if (users.length === 100) break;
}


let result = cf.recommendToUsers(users, 10);
console.log(result)
```
