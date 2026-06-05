# Stephanie-design-
const express = require("express");
const app = express();

app.use(express.json({limit:"10mb"}));

let orders = [];

const ADMIN_USERNAME = "admin";
const ADMIN_PASSWORD = "Stephanie123";

app.get("/", (req, res) => {
res.send(`
<!DOCTYPE html>
<html>
<head>
<title>Stephanie Fashion Trend</title>

<meta name="viewport" content="width=device-width, initial-scale=1">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:#111;
color:white;
}

header{
background:black;
padding:25px;
text-align:center;
border-bottom:2px solid hotpink;
}

header h1{
color:hotpink;
}

.hero{
padding:40px;
text-align:center;
}

.gallery{
display:flex;
flex-wrap:wrap;
justify-content:center;
gap:20px;
padding:20px;
}

.gallery img{
width:280px;
height:350px;
object-fit:cover;
border-radius:10px;
}

form{
max-width:600px;
margin:auto;
padding:20px;
}

input,textarea{
width:100%;
padding:12px;
margin:10px 0;
border:none;
border-radius:5px;
}

button{
background:hotpink;
color:white;
padding:12px 20px;
border:none;
border-radius:5px;
cursor:pointer;
}

button:hover{
background:deeppink;
}

.admin{
text-align:center;
padding:30px;
}

#result{
margin-top:10px;
color:lightgreen;
}

</style>
</head>

<body>

<header>
<h1>Stephanie Fashion Trend</h1>
<p>Luxury Fashion Designer</p>
<p>Order Line: 08047829074</p>
</header>

<section class="hero">
<h2>Welcome To Stephanie Fashion Trend</h2>
<p>Elegant Designs • Custom Sewing • Modern Fashion</p>
</section>

<section>

<h2 style="text-align:center">Portfolio</h2>

<div class="gallery">

<img src="https://images.unsplash.com/photo-1529139574466-a303027c1d8b">

<img src="https://images.unsplash.com/photo-1496747611176-843222e1e57c">

<img src="https://images.unsplash.com/photo-1483985988355-763728e1935b">

</div>

</section>

<section>

<h2 style="text-align:center">Place Order</h2>

<form id="orderForm">

<input
id="name"
placeholder="Full Name"
required>

<input
id="phone"
placeholder="Phone Number"
required>

<textarea
id="measurements"
placeholder="Enter Measurements">
</textarea>

<textarea
id="details"
placeholder="Describe Your Design">
</textarea>

<input
type="file"
id="photo">

<button type="submit">
Submit Order
</button>

<p id="result"></p>

</form>

</section>

<section class="admin">

<h2>Admin Login</h2>

<input id="user" placeholder="Username">

<input id="pass"
type="password"
placeholder="Password">

<button onclick="login()">
Login
</button>

<div id="orders"></div>

</section>

<script>

document.getElementById("orderForm")
.addEventListener("submit",
async function(e){

e.preventDefault();

const file =
document.getElementById("photo").files[0];

let image="";

if(file){

const reader = new FileReader();

reader.onload = async function(){

image = reader.result;

sendOrder(image);

}

reader.readAsDataURL(file);

}else{

sendOrder("");

}

});

async function sendOrder(image){

const response = await fetch("/order",{

method:"POST",

headers:{
"Content-Type":"application/json"
},

body:JSON.stringify({

name:
document.getElementById("name").value,

phone:
document.getElementById("phone").value,

measurements:
document.getElementById("measurements").value,

details:
document.getElementById("details").value,

image

})

});

const data = await response.json();

document.getElementById("result")
.innerText = data.message;

}

async function login(){

const username =
document.getElementById("user").value;

const password =
document.getElementById("pass").value;

const response =
await fetch("/admin",{

method:"POST",

headers:{
"Content-Type":"application/json"
},

body:JSON.stringify({
username,
password
})

});

const data =
await response.json();

if(data.success){

let html="<h3>Orders</h3>";

data.orders.forEach(order=>{

html += \`
<div style="border:1px solid gray;padding:10px;margin:10px">

<p><b>Name:</b> \${order.name}</p>

<p><b>Phone:</b> \${order.phone}</p>

<p><b>Measurements:</b>
\${order.measurements}</p>

<p><b>Details:</b>
\${order.details}</p>

</div>
\`;

});

document.getElementById("orders")
.innerHTML = html;

}else{

alert("Wrong Login");

}

}

</script>

</body>
</html>
`);
});

app.post("/order",(req,res)=>{

orders.push({

...req.body,

date:new Date()

});

res.json({
message:"Order Submitted Successfully"
});

});

app.post("/admin",(req,res)=>{

const {username,password}
= req.body;

if(
username===ADMIN_USERNAME &&
password===ADMIN_PASSWORD
){

res.json({
success:true,
orders
});

}else{

res.json({
success:false
});

}

});

app.listen(3000,()=>{

console.log(
"Running at http://localhost:3000"
);

});
