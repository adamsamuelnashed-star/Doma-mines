<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Mines Game</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: #0f172a;
        color: #fff;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    .game {
        background: #020617;
        padding: 20px;
        border-radius: 12px;
        width: 320px;
        text-align: center;
    }
    .controls {
        margin-bottom: 10px;
    }
    input, button {
        padding: 6px;
        margin: 5px;
        border-radius: 6px;
        border: none;
        outline: none;
    }
    button {
        background: #2563eb;
        color: #fff;
        cursor: pointer;
    }
    button:hover {
        background: #1d4ed8;
    }
    .grid {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 6px;
        margin-top: 10px;
    }
    .cell {
        width: 50px;
        height: 50px;
        background: #1e293b;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        font-size: 22px;
        border-radius: 6px;
    }
    .cell.open {
        background: #334155;
        cursor: default;
    }
    .cell.mine {
        background: #dc2626;
    }
</style>
</head>
<body>

<div class="game">
    <h2>💣 Mines Game</h2>

    <div class="controls">
        <label>عدد الألغام:</label>
        <input type="number" id="minesCount" value="5" min="1" max="24">
        <button onclick="startGame()">Start</button>
    </div>

    <div class="grid" id="grid"></div>
    <p id="status"></p>
</div>

<script>
const gridElement = document.getElementById("grid");
const statusText = document.getElementById("status");

let mines = [];
let gameOver = false;

function startGame() {
    gridElement.innerHTML = "";
    statusText.textContent = "";
    gameOver = false;
    mines = [];

    const minesCount = Number(document.getElementById("minesCount").value);
    const totalCells = 25;

    // توليد الألغام عشوائي
    while (mines.length < minesCount) {
        const r = Math.floor(Math.random() * totalCells);
        if (!mines.includes(r)) mines.push(r);
    }

    for (let i = 0; i < totalCells; i++) {
        const cell = document.createElement("div");
        cell.className = "cell";
        cell.onclick = () => openCell(cell, i);
        gridElement.appendChild(cell);
    }
}

function openCell(cell, index) {
    if (gameOver || cell.classList.contains("open")) return;

    cell.classList.add("open");

    if (mines.includes(index)) {
        cell.classList.add("mine");
        cell.textContent = "💣";
        statusText.textContent = "❌ خسرت!";
        revealMines();
        gameOver = true;
    } else {
        cell.textContent = "💎";
    }
}

function revealMines() {
    document.querySelectorAll(".cell").forEach((cell, i) => {
        if (mines.includes(i)) {
            cell.classList.add("mine");
            cell.textContent = "💣";
        }
    });
}
</script>

</body>
</html>
