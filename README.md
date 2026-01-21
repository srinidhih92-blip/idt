# idt
Nutrition and calorie tracking app
<!DOCTYPE html>
<html>
<head>
<title>Smart Nutrition Tracker</title>

<style>
body {
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(to right, #e8f5e9, #f1f8e9);
    margin: 0;
    padding: 0;
}
.container {
    width: 90%;
    max-width: 900px;
    margin: auto;
    padding: 20px;
}
h1 {
    text-align: center;
    color: #2e7d32;
}
.card {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px #aaa;
    margin-bottom: 20px;
}
input, select {
    padding: 8px;
    margin: 5px 0;
    width: 100%;
}
button {
    background: #2e7d32;
    color: white;
    border: none;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
    width: 100%;
}
button:hover {
    background: #1b5e20;
}
table {
    width: 100%;
    border-collapse: collapse;
}
th {
    background: #4caf50;
    color: white;
    padding: 10px;
}
td {
    border: 1px solid #ccc;
    padding: 8px;
    text-align: center;
}
.progress-bar {
    height: 20px;
    background: #c8e6c9;
    border-radius: 10px;
    overflow: hidden;
}
.progress {
    height: 100%;
    background: #4caf50;
    width: 0%;
}
</style>
</head>

<body>
<div class="container">

<h1>🥗 Smart Nutrition Tracker</h1>

<!-- Add Food -->
<div class="card">
<h3>Add Food</h3>
<select id="food">
    <option value="Rice,130,2.7,28,0.3">Rice (100g)</option>
    <option value="Chapati,120,3,20,1">Chapati</option>
    <option value="Egg,155,13,1.1,11">Egg</option>
    <option value="Milk,42,3.4,5,1">Milk (100ml)</option>
    <option value="Banana,89,1.1,23,0.3">Banana</option>
    <option value="Chicken,165,31,0,3.6">Chicken (100g)</option>
    <option value="Paneer,265,18,2,20">Paneer (100g)</option>
    <option value="Apple,52,0.3,14,0.2">Apple</option>
    <option value="Oats,389,17,66,7">Oats (100g)</option>
</select>

<input type="number" id="qty" value="1" min="1" placeholder="Servings">
<button onclick="addFood()">Add</button>
</div>

<!-- Table -->
<div class="card">
<table>
<tr>
    <th>Food</th>
    <th>Servings</th>
    <th>Calories (kcal)</th>
    <th>Protein (g)</th>
    <th>Carbs (g)</th>
    <th>Fat (g)</th>
    <th>Remove</th>
</tr>
<tbody id="list"></tbody>
</table>
</div>

<!-- Nutrition -->
<div class="card">
<h3>Total Nutrition</h3>
<p>Calories: <span id="cal">0</span> kcal</p>
<p>Protein: <span id="protein">0</span> g</p>
<p>Carbs: <span id="carbs">0</span> g</p>
<p>Fat: <span id="fat">0</span> g</p>

<h4>Daily Calorie Goal</h4>
<div class="progress-bar">
    <div class="progress" id="progress"></div>
</div>
</div>

<!-- BMI -->
<div class="card">
<h3>📏 BMI Calculator</h3>
<input type="number" id="weight" placeholder="Weight (kg)">
<input type="number" id="height" placeholder="Height (cm)">
<button onclick="calculateBMI()">Calculate BMI</button>

<p>BMI: <span id="bmi">0</span></p>
<p>Status: <span id="status">---</span></p>
<p>Ideal Weight: <span id="ideal">---</span> kg</p>
<p>Recommended Calories: <span id="dailyCal">2000</span> kcal</p>
</div>

</div>

<script>
let totalCal=0, totalPro=0, totalCarb=0, totalFat=0;
let goal = 2000;

function addFood(){
    let foodData = document.getElementById("food").value.split(",");
    let qty = parseFloat(document.getElementById("qty").value);

    let name = foodData[0];
    let cal = parseFloat(foodData[1]) * qty;
    let pro = parseFloat(foodData[2]) * qty;
    let carb = parseFloat(foodData[3]) * qty;
    let fat = parseFloat(foodData[4]) * qty;

    totalCal += cal;
    totalPro += pro;
    totalCarb += carb;
    totalFat += fat;

    let row = document.createElement("tr");
    row.innerHTML = `
        <td>${name}</td>
        <td>${qty}</td>
        <td>${cal.toFixed(1)} kcal</td>
        <td>${pro.toFixed(1)} g</td>
        <td>${carb.toFixed(1)} g</td>
        <td>${fat.toFixed(1)} g</td>
        <td><button onclick="removeRow(this, ${cal}, ${pro}, ${carb}, ${fat})">❌</button></td>
    `;

    document.getElementById("list").appendChild(row);
    update();
}

function removeRow(btn, cal, pro, carb, fat){
    totalCal -= cal;
    totalPro -= pro;
    totalCarb -= carb;
    totalFat -= fat;
    btn.parentElement.parentElement.remove();
    update();
}

function update(){
    document.getElementById("cal").innerText = totalCal.toFixed(1);
    document.getElementById("protein").innerText = totalPro.toFixed(1);
    document.getElementById("carbs").innerText = totalCarb.toFixed(1);
    document.getElementById("fat").innerText = totalFat.toFixed(1);
    document.getElementById("progress").style.width = (totalCal/goal*100) + "%";
}

function calculateBMI(){
    let w = parseFloat(document.getElementById("weight").value);
    let h = parseFloat(document.getElementById("height").value) / 100;

    let bmi = w / (h*h);
    document.getElementById("bmi").innerText = bmi.toFixed(2);

    let status="";
    if(bmi<18.5) status="Underweight";
    else if(bmi<25) status="Normal";
    else if(bmi<30) status="Overweight";
    else status="Obese";

    document.getElementById("status").innerText=status;

    let ideal = 22*(h*h);
    document.getElementById("ideal").innerText = ideal.toFixed(1);

    goal = ideal * 30;
    document.getElementById("dailyCal").innerText = goal.toFixed(0);
    update();
}
</script>

</body>
</html>
