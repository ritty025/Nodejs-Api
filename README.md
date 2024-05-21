# Nodejs-Api
Contains backend functionality codes in nodejs 
#  mongoConnection of database in mongodb
const mongoose = require("mongoose")
mongoose.connect("mongodb://localhost:27017/Registration")
.then(()=>{
    console.log("Mongodb connected");
})
.catch(()=>{
    console.log("Failed to connect");
})

const LogInSchema = new mongoose.Schema({
    name:{
        type:String,
        required:true
    },

    password:{
        type:String,
        required:true
    },

    email:{
        type:String,
        required: true
    }
       

})

const collection =  new mongoose.model("collection1",LogInSchema)

module.exports = collection

# index.js file for api

const express = require("express")
const app = express()
const path = require("path")
const hbs = require("hbs")
const collection = require("./mongodb")
// const templatePath=path.join(__dirname,'../templates')

app.use(express.json())
app.set("view engine","hbs")
// app.set("views",templatePath)

app.use(express.urlencoded({extended:false}))


app.get("/",(req,res)=>{
    res.render("login")
})

app.get("/signup",(req,res)=>{
    res.render("signup")
})

app.post("/signup",async(req,res)=>{
    const data ={
        name:req.body.name,
        password:req.body.password,
        email:req.body.email
    }

    await collection.insertMany([data])

    res.render("home")
})

app.post("/login",async(req,res)=>{
    const data ={
        name:req.body.name,
        password:req.body.password
    }

    await collection.insertMany([data])

    res.render("website")
})


app.listen(3000,()=>{
    console.log("port connected")
})

# package.json file for the installed packages

{
  "dependencies": {
    "express": "^4.19.2",
    "hbs": "^4.2.0",
    "mongoose": "^8.4.0",
    "nodemon": "^3.1.0"
  }

}

# home.hbs html page for home
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h3>HOME</h3>
</body>
</html>

# login.hbs page for login

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2>Login</h2>
    <form action="/login" method="post">

<input type="text" name="name" placeholder="Name">
<input type="password" name="password" placeholder="Password">
<input type="submit">
    </form>

    <a href="/signup">Make a new account</a>
</body>
</html>

# signup.hbs for signing up

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2>Signup</h2>
    <form action="/signup" method="post">

<input type="text" name="name" placeholder="Name">
<input type="password" name="password" placeholder="Password">
<input type="email" name="email" placeholder="Email">
<input type="submit">
    </form>
</body>
</html>

# website.hbs

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h3>Welcome to Nodejs Intership Program</h3>
    <p>Learn , Grow and Excel!</p>
</body>
</html>
