<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PLANTRACK</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Space+Grotesk:wght@400;600&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;}
body{
    font-family:'Poppins',sans-serif;
    background: linear-gradient(135deg, #f0f4f8, #e0e7ff);
    color:#1e293b;
    min-height:100vh;
}

/* Floating shapes */
.shape{position:absolute;border-radius:50%;opacity:0.2;animation:float 10s ease-in-out infinite;}
.shape1{width:150px;height:150px;background:#38bdf8;top:10%;left:5%;}
.shape2{width:100px;height:100px;background:#a78bfa;top:70%;left:80%;}
.shape3{width:120px;height:120px;background:#facc15;top:40%;left:50%;}
@keyframes float{0%,100%{transform:translateY(0);}50%{transform:translateY(-20px);}}

/* NAV */
header{display:flex;justify-content:space-between;padding:20px 40px;}
header h2{font-family:'Space Grotesk'; color:#38bdf8;}
nav a{margin-left:15px; color:#1e293b; text-decoration:none; font-weight:500;}
nav a:hover{color:#a78bfa;}

/* HERO */
.hero{text-align:center;margin-top:50px;}
.hero img{width:150px;margin:20px 0;}
.hero h2{
    font-family:'Space Grotesk'; font-size:50px;
    background: linear-gradient(90deg,#38bdf8,#a78bfa);
    -webkit-background-clip:text; color:transparent;
}

/* BUTTONS */
button{
    padding:10px 25px; border-radius:30px; border:none;
    background: linear-gradient(135deg,#38bdf8,#a78bfa);
    color:white; cursor:pointer; transition:0.3s;
}
button:hover{transform:scale(1.05);}

/* QUOTE */
.quote-box{
    text-align:center;margin:30px auto;padding:15px 25px;
    background: rgba(255,255,255,0.7); border-radius:20px;
    font-size:18px; max-width:600px; color:#1e293b; font-weight:500;
    animation: floatBox 3s ease-in-out infinite;
}
@keyframes floatBox{0%,100%{transform:translateY(0);}50%{transform:translateY(-8px);}}

/* CONTAINER */
.container{
    margin:50px auto; width:90%; max-width:600px;
    background: rgba(255,255,255,0.8); padding:30px; border-radius:25px;
}

/* INPUTS */
input, select{padding:10px; border-radius:10px; border:none; margin:5px;}
input#taskInput{width:45%;}
input#date{width:25%;}
select{width:20%;}
button#addBtn{width:100%; margin-top:10px;}

/* TASK LIST */
ul{list-style:none;}
li{
    background: #e0e7ff; padding:10px; margin-top:10px;
    border-radius:10px; display:flex; justify-content:space-between; align-items:center;
}
.high{border-left:5px solid #f43f5e;}
.medium{border-left:5px solid #f97316;}
.low{border-left:5px solid #22c55e;}
.done{text-decoration:line-through; opacity:0.5;}
</style>
</head>
<body>

<!-- floating shapes -->
<div class="shape shape1"></div>
<div class="shape shape2"></div>
<div class="shape shape3"></div>

<header>
    <h2>PLANTRACK</h2>
    <nav>
        <a href="index.html">Home</a>
        <a href="tutor.html">AI Tutor</a>
    </nav>
</header>

<div class="hero">
    <img src="https://cdn-icons-png.flaticon.com/512/2910/2910763.png" alt="Planner Illustration">
    <h2>Plan Smarter, Not Harder</h2>
    <button>Start Planning</button>
</div>

<div class="quote-box" id="quoteText">Loading inspiration...</div>

<div class="container">
<div style="display:flex; flex-wrap:wrap; justify-content:space-between;">
    <input id="taskInput" placeholder="Task...">
    <input type="date" id="date">
    <select id="priority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
    </select>
</div>
<button id="addBtn">Add Task</button>

<ul id="list"></ul>
<div id="progress"></div>
</div>

<script>
// TASK SYSTEM
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function save(){localStorage.setItem("tasks",JSON.stringify(tasks));}

function render(){
    let list=document.getElementById("list"); list.innerHTML="";
    let done=0;
    tasks.forEach((t,i)=>{
        let li=document.createElement("li");
        li.className=t.priority + (t.done?" done":"");
        li.innerHTML=`<span onclick="toggle(${i})">${t.text} (${t.date})</span>
        <button onclick="del(${i})">✕</button>`;
        if(t.done) done++;
        list.appendChild(li);
    });
    document.getElementById("progress").innerText=`Completed ${done}/${tasks.length}`;
}

function addTask(){
    let text=document.getElementById("taskInput").value;
    let date=document.getElementById("date").value;
    let priority=document.getElementById("priority").value;
    if(!text) return alert("Please enter a task!");
    tasks.push({text,date,priority,done:false});
    save(); render();
    document.getElementById("taskInput").value="";
}

function toggle(i){tasks[i].done=!tasks[i].done; save(); render();}
function del(i){tasks.splice(i,1); save(); render();}
document.getElementById("addBtn").addEventListener("click", addTask);

render();

// QUOTES
const quotes=["Do it now 👀","Start ugly, finish legendary","Stop scrolling. Start doing.","You got this ✨","Romanticize your productivity era 💖"];
let q=0;
setInterval(()=>{q=(q+1)%quotes.length; document.getElementById("quoteText").innerText=quotes[q];},3000);
</script>
</body>
</html>
