---
layout: post
title: Passport to User Authentication in Express
subtitle: A minimalist app to authenticate users
bigimg: /img/key.jpg
image: /img/key-square.jpg
share-img: /img/key-square.jpg
tags: nodejs express mongodb authentication security technology
excerpt: 
---

In a [previous]({% post_url 2017-12-18-nodejs %}) post, I took you into a deep dive to create a RESTful nodejs express app. One component which was missing is the user authentication function. In this post, I would like to build a minimalist app for user authentication sitting on the shoulder of some ready-made express modules.

## Modules to Use

The big hero here is [passport](http://www.passportjs.org/). In addition, I will need to use a number of other modules especially that I am using `mongoose` as my database. These are [passport local](https://github.com/jaredhanson/passport-local) and [passport local mongoose](https://github.com/saintedlama/passport-local-mongoose
). Also, I will need sessions. 

Here is my dependencies from `package.json` file. 

``` json
  "dependencies": {
    "body-parser": "^1.18.2",
    "ejs": "^2.5.7",
    "express": "^4.16.2",
    "express-session": "^1.15.6",
    "mongoose": "^4.13.7",
    "passport": "^0.4.0",
    "passport-local": "^1.0.0",
    "passport-local-mongoose": "^4.4.0"
  }
```

Here is the look of my `app.js` file's config section.

```javascript
var express               = require("express"),
    mongoose              = require("mongoose"),
    passport              = require('passport'),
    bodyParser            = require('body-parser'),
    User                  = require("./models/user"),
    LocalStrategy         = require("passport-local"),
    passportLocalMongoose = require('passport-local-mongoose');

mongoose.Promise = global.Promise;
mongoose.connect("mongodb://localhost/auth_demo_app", {useMongoClient: true});
var app = express();   
app.set('view engine', 'ejs');
app.use(bodyParser.urlencoded({extended:true}));

//passport and session config
app.use(require("express-session")({
    secret: 'I am not going to tell you',
    resave: false,
    saveUninitialized: false
}));

//set passport to work
app.use(passport.initialize());
app.use(passport.session());

passport.use(new LocalStrategy(User.authenticate()));
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

## User Data Model

Here is the user data model created in a folder called `/models/user.js`.

```javascript
var mongoose = require("mongoose");
var passportLocalMongoose = require("passport-local-mongoose");

var UserSchema = new mongoose.Schema({
    username: String,
    password: String
});

UserSchema.plugin(passportLocalMongoose);

module.exports = mongoose.model("User", UserSchema);
```

Note how we used the `.plugin` function to get this giant do everything for us!

## Home.ejs

Here is my minimum `home.ejs` file that contains few links.

```html
<h1>This is the homepage</h1>

<li><a href="/register">Sign Up</a></li>
<li><a href="/login">Login</a></li>
<li><a href="/Logout">Logout</a></li>
```

and here is the associating route in `app.js`

```javascript
app.get('/', function(req,res){
    res.render('home');
});
```

## Our `secret.ejs` file

This is the file we want to secure and prevent un-authenticated access. Let us just put something inside. 

##### secret.ejs

```html
<h1>This is the secret page</h1>

<p>You found me!</p>


<li><a href="/register">Sign Up</a></li>
<li><a href="/login">Login</a></li>
<li><a href="/logout">Logout</a></li>
```

and here is the route to it.

##### app.js

```javascript
app.get('/secret',isLoggedIn, function(req,res){
    res.render('secret');
});
```

`isLoggedIn` is a middle ware function defined in this file as follows:

```javascript
function isLoggedIn(req,res, next){
    if(req.isAuthenticated()){
        return next();
    }
    res.redirect('/login');
};
```

This function uses our passport and session modules to check is the user is authenticated. It will allow `next()` only if the user is authenticated. This next is the call function used in the `/secret` route above. 

## Sign Up file `register.ejs`

##### register.ejs

```html
<h1>Regiser Form</h1>

<form action="/register" method='POST'>
    <input type='text' placeholder='user name' name='username'>
    <input type='password' placeholder='password' name='password'>
    <button>Submit</button>
</form>


<li><a href="/register">Sign Up</a></li>
<li><a href="/login">Login</a></li>
<li><a href="/Logout">Logout</a></li>
```

I will need to routes to `/register` one `get` that will show the above form and the other `post` that will actually add the user in the database. Let us see them.

```javascript
app.get('/register', function(req,res){
    res.render('register');
});

//post
app.post('/register', function(req,res){
    User.register(new User({username: req.body.username}), req.body.password, function(err,user){
       if(err){
           console.log(err);
           return res.render('register');
       } 
           passport.authenticate('local')(req,res, function(){
              res.redirect("/secret"); 
           });
       
    });
});
```

## Login `login.ejs`

Similar to register, we will have `/login` routes twice one get and the other post

##### login.ejs

```html
<h1>Login</h1>

<form action='/login' method='POST'>
    <input type='text' name='username' placeholder='user name'>
    <input type='password' name='password' placeholder='password'>
    <button>Login</button>
</form>


<li><a href="/register">Sign Up</a></li>
<li><a href="/login">Login</a></li>
<li><a href="/Logout">Logout</a></li>
```

```javascript
app.get('/login', function(req,res){
    res.render('login');
});

app.post('/login', passport.authenticate('local',{
    successRedirect: '/secret',
    failureRedirect: '/login'
}) , function(req,res){
    
});
```

## Logout

This is easier given the power of `passport`. All I needed is just a link to the following route from any page.

```javascript
app.get('/logout', function(req,res){
    req.logout();
    res.redirect('/');
});
```

## Dynamically show/hide links

Final bit is to dynamically adjust the links in the header of your project. So if a user is already logged in welcome him and say 'Welcome Abdulbaqi' and show a 'logout' link, and if not show both login and sign up links.

The step is to create another middle ware before any route as follows.

```javascript
app.use(function(req,res,next){
    res.locals.currentUser = req.user;
    next();
}
```

The above middle ware will pass a variable `currentUser` to all routes, so you can use this variable to create your logic as follows (for example in the header file);

##### header.ejs

```html
{%raw%}<% if(!currentUser) {%>
        <li><a href="/login">Login </a></li>
        <li><a href="/register">Sign Up</a></li>
      <% } else { %>
        <li><a href="#"> Welcome <%=currentUser.username%></a></li>
        <li><a href="/logout">Logout</a></li>
      <% } %>
}{%endraw%}
```

## Conclusion

It would be a hell of a lifetime if we opted to build authentication from scratch. With the above skeleton you can plugin this solution to any express solution. 

Here again I must express my thanks to the wonderful tutorial by Colt Steele in this web development bootcamp at Udemy. 