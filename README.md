<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>🔥 Workout Pro</title>

<style>
body {
    font-family: Arial;
    background: linear-gradient(black, #222);
    color: white;
    text-align: center;
}

h1 { color: #00ffcc; }

.container {
    background: #111;
    padding: 20px;
    border-radius: 15px;
    width: 320px;
    margin: auto;
}

input {
    width: 60px;
    margin: 5px;
}

button {
    padding: 10px;
    margin: 5px;
    background: #00ffcc;
    border: none;
    border-radius: 8px;
    cursor: pointer;
}

.exercise {
    display: flex;
    justify-content: space-between;
    margin: 10px 0;
}

.done {
    text-decoration: line-through;
    color: gray;
}

#timer {
    font-size: 20px;
    margin: 10px;
}

.levelBox {
    margin: 10px;
    padding: 10px;
    background: #333;
    border-radius: 10px;
}
</style>

</head>

<body>

<h1>💪 Workout Game</h1>

<div class="container">

<div class="levelBox">
⭐ Level: <span id="level">1</span><br>
🔥 Points: <span id="points">0</span>
</div>

<p>حدد العدات</p>

ضغط <input type="number" id="pushups" value="30" max="100"><br>
سكواد <input type="number" id="squat" value="30" max="100"><br>
باي <input type="number" id="biceps" value="30" max="100"><br>
تراي <input type="number" id="triceps" value="30" max="100"><br>
بلانك (ثواني) <input type="number" id="plank" value="50" max="100"><br>
بطن <input type="number" id="abs" value="30" max="100"><br><br>

<button onclick="startWorkout()">🚀 ابدأ</button>

<div id="timer">⏱️ 0</div>

<div id="workout"></div>

<button onclick="finish()">🏁 خلصت</button>

<p id="result"></p>

</div>

<audio id="doneSound" src="https://www.soundjay.com/buttons/sounds/button-3.mp3"></audio>

<script>

let points = localStorage.getItem("points") || 0;
let level = localStorage.getItem("level") || 1;

document.getElementById("points").innerText = points;
document.getElementById("level").innerText = level;

let timerInterval;
let time = 0;

function startWorkout() {
    let workoutDiv = document.getElementById("workout");
    workoutDiv.innerHTML = "";

    let exercises = [
        {name: "ضغط", val: pushups.value},
        {name: "سكواد", val: squat.value},
        {name: "باي", val: biceps.value},
        {name: "تراي", val: triceps.value},
        {name: "بلانك", val: plank.value},
        {name: "بطن", val: abs.value}
    ];

    exercises.forEach((ex, index) => {
        workoutDiv.innerHTML += `
        <div class="exercise">
            <span id="ex${index}">${ex.name} - ${ex.val}</span>
            <input type="checkbox" onclick="markDone(${index})">
        </div>
        `;
    });

    startTimer();
}

function startTimer() {
    clearInterval(timerInterval);
    time = 0;

    timerInterval = setInterval(() => {
        time++;
        document.getElementById("timer").innerText = "⏱️ " + time + "s";
    }, 1000);
}

function markDone(i) {
    document.getElementById("ex"+i).classList.toggle("done");
}

function finish() {
    clearInterval(timerInterval);

    let checks = document.querySelectorAll("input[type='checkbox']");
    let done = 0;

    checks.forEach(c => {
        if (c.checked) done++;
    });

    let gained = done * 10;
    points = parseInt(points) + gained;

    if (points >= level * 50) {
        level++;
    }

    localStorage.setItem("points", points);
    localStorage.setItem("level", level);

    document.getElementById("points").innerText = points;
    document.getElementById("level").innerText = level;

    document.getElementById("result").innerText =
        "🔥 خلصت " + done + " تمارين وكسبت " + gained + " نقطة 🔥";

    document.getElementById("doneSound").play();
}

</script>

</body>
</html>
