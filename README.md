<!DOCTYPE html><html lang="es">
<head>
<meta charset="UTF-8">
<title>CrimsonLuck 🎰</title>
<style>
:root {--rojo:#c0392b;--negro:#1c1c1c;--blanco:#f5f5f5;}
body{font-family:Arial;background-color:var(--negro);color:var(--blanco);margin:0;padding:0;}
header{background-color:var(--negro);padding:15px;font-size:24px;font-weight:bold;color:var(--rojo);text-align:center;}
input, button{padding:10px;margin:5px;border-radius:5px;border:none;}
button{background-color:var(--rojo);color:var(--blanco);cursor:pointer;}
button:hover{background-color:var(--blanco);color:var(--rojo);}
.user-panel, .admin-panel, .casino, .chat-panel, .referidos-panel{margin:10px;padding:20px;border-radius:10px;}
.user-panel{background-color:var(--rojo);}
.admin-panel{background-color:var(--negro);color:var(--rojo);}
.casino{background-color:var(--negro);}
.chat-panel, .soporte-panel{background-color:#111;height:200px;overflow-y:auto;}
</style>
</head>
<body>
<header>CrimsonLuck 🎰</header>
<div id="app"></div>
<script>
// -------------------- Datos globales --------------------
let users=[{username:"admin",password:"1234",balance:0,vip:"VIP0",referidos:[],status:"aprobado",isAdmin:true}];
let pendingTransactions=[];
let currentUser=null;
let chatMessages=["Juanita retiró 500 DOP","Pedro depositó 1000 DOP","Ana apostó 50 DOP en la ruleta"];// -------------------- Generar voucher -------------------- function generateVoucher(){return 'VCH-'+Math.floor(Math.random()*1000000);}

// -------------------- VIP -------------------- const vipLevels=[{level:"VIP1",amount:300},{level:"VIP2",amount:2000},{level:"VIP3",amount:3500},{level:"VIP4",amount:10000},{level:"VIP5",amount:50000},{level:"VIP6",amount:80000}]; function getVip(balance){let vip="VIP0";vipLevels.forEach(v=>{if(balance>=v.amount) vip=v.level});return vip;}

// -------------------- Login / Registro -------------------- function renderLogin(){document.getElementById("app").innerHTML=<h2>Login / Registro</h2><input type="text" id="username" placeholder="Nombre de usuario"><br><input type="password" id="password" placeholder="Contraseña"><br><button onclick="register()">Registrarse</button><button onclick="login()">Entrar</button>;} function register(){ const username=document.getElementById("username").value; const password=document.getElementById("password").value; if(!username||!password){alert("Completa todos los campos");return;} users.push({username,password,balance:0,vip:"VIP0",referidos:[],status:"aprobado",isAdmin:false}); alert("Usuario registrado correctamente. Ahora puedes ingresar.");} function login(){ const username=document.getElementById("username").value; const password=document.getElementById("password").value; const user=users.find(u=>u.username===username && u.password===password); if(!user){alert("Usuario o contraseña incorrecta");return;} currentUser=user; user.isAdmin?renderAdmin():renderDashboard();}

// -------------------- Logout -------------------- function logout(){currentUser=null;renderLogin();}

// -------------------- Dashboard usuario -------------------- function renderDashboard(){ currentUser.vip=getVip(currentUser.balance); document.getElementById("app").innerHTML=`

<div class="user-panel">
<h2>Panel Usuario - ${currentUser.username}</h2>
<p>Saldo: <span id="balance">${currentUser.balance}</span> DOP</
