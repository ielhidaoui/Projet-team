<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>To-Do List</title>

<style>
body {
    font-family: Arial;
    background: #f4f4f4;
    text-align: center;
}

.container {
    width: 300px;
    margin: auto;
}

input {
    padding: 10px;
    width: 70%;
}

button {
    padding: 10px;
    border: none;
    color: white;
    cursor: pointer;
}

.addBtn { background: blue; }

li {
    list-style: none;
    margin: 10px;
    padding: 10px;
    color: white;
    display: flex;
    justify-content: space-between;
}

.red { background: red; }
.green { background: green; }
.blue { background: blue; }
</style>
</head>

<body>

<div class="container">
    <h2>Ma To-Do List</h2>

    <input type="text" id="task" placeholder="Ajouter une tâche">
    <button class="addBtn" onclick="addTask()">Ajouter</button>

    <ul id="list"></ul>
</div>

<script>
function loadTasks() {
    fetch("getTasks.php")
    .then(res => res.json())
    .then(data => {
        document.getElementById("list").innerHTML = "";

        data.forEach(task => {
            let colors = ["red","green","blue"];
            let randomColor = colors[Math.floor(Math.random()*colors.length)];

            let li = document.createElement("li");
            li.className = randomColor;

            li.innerHTML = `
                ${task.name}
                <button onclick="deleteTask(${task.id})">❌</button>
            `;

            document.getElementById("list").appendChild(li);
        });
    });
}

function addTask() {
    let task = document.getElementById("task").value;
    if(task === "") return;

    fetch("save.php", {
        method: "POST",
        headers: {
            "Content-Type": "application/x-www-form-urlencoded"
        },
        body: "task=" + task
    }).then(() => loadTasks());

    document.getElementById("task").value = "";
}

function deleteTask(id) {
    fetch("delete.php", {
        method: "POST",
        headers: {
            "Content-Type": "application/x-www-form-urlencoded"
        },
        body: "id=" + id
    }).then(() => loadTasks());
}

// تحميل المهام
loadTasks();
</script>

</body>
</html>
