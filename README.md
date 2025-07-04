# Cat-snake-
Snake game but it’s cats 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cat Snake Game</title>
  <style>
    body {
      margin: 0;
      background-color: #fefefe;
      display: flex;
      flex-direction: column;
      align-items: center;
      height: 100vh;
    }
    canvas {
      background: #e0f7fa;
      border: 4px solid #333;
      touch-action: none;
    }
    .controls {
      margin-top: 10px;
      display: grid;
      grid-template-columns: 80px 80px 80px;
      grid-template-rows: 80px 80px;
      gap: 10px;
    }
    .btn {
      background: #ffb74d;
      border: 2px solid #333;
      font-size: 2em;
      display: flex;
      justify-content: center;
      align-items: center;
      user-select: none;
      border-radius: 10px;
    }
    .btn:active {
      background: #ff9800;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="400" height="400"></canvas>

  <div class="controls">
    <div></div>
    <div class="btn" id="up">↑</div>
    <div></div>
    <div class="btn" id="left">←</div>
    <div class="btn" id="down">↓</div>
    <div class="btn" id="right">→</div>
  </div>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const gridSize = 20;
    const tileCount = canvas.width / gridSize;

    let catSnake = [{ x: 10, y: 10 }];
    let direction = { x: 0, y: 0 };
    let fish = { x: 5, y: 5 };

    let gameSpeed = 100;

    function drawCat(x, y) {
      ctx.fillStyle = "orange";
      ctx.fillRect(x * gridSize, y * gridSize, gridSize - 2, gridSize - 2);
      ctx.fillStyle = "black";
      ctx.fillRect(x * gridSize + 5, y * gridSize + 5, 4, 4);
    }

    function drawFish(x, y) {
      ctx.fillStyle = "blue";
      ctx.beginPath();
      ctx.arc(x * gridSize + gridSize / 2, y * gridSize + gridSize / 2, gridSize / 2 - 2, 0, Math.PI * 2);
      ctx.fill();
    }

    function gameLoop() {
      let head = { x: catSnake[0].x + direction.x, y: catSnake[0].y + direction.y };

      if (head.x < 0) head.x = tileCount - 1;
      if (head.x >= tileCount) head.x = 0;
      if (head.y < 0) head.y = tileCount - 1;
      if (head.y >= tileCount) head.y = 0;

      for (let part of catSnake) {
        if (part.x === head.x && part.y === head.y) {
          alert("Game Over! Your cat got tangled up!");
          catSnake = [{ x: 10, y: 10 }];
          direction = { x: 0, y: 0 };
          fish = { x: Math.floor(Math.random() * tileCount), y: Math.floor(Math.random() * tileCount) };
          return;
        }
      }

      catSnake.unshift(head);

      if (head.x === fish.x && head.y === fish.y) {
        fish = {
          x: Math.floor(Math.random() * tileCount),
          y: Math.floor(Math.random() * tileCount)
        };
      } else {
        catSnake.pop();
      }

      ctx.clearRect(0, 0, canvas.width, canvas.height);

      drawFish(fish.x, fish.y);

      for (let part of catSnake) {
        drawCat(part.x, part.y);
      }
    }

    window.addEventListener("keydown", e => {
      switch (e.key) {
        case "ArrowUp":
          if (direction.y === 1) break;
          direction = { x: 0, y: -1 };
          break;
        case "ArrowDown":
          if (direction.y === -1) break;
          direction = { x: 0, y: 1 };
          break;
        case "ArrowLeft":
          if (direction.x === 1) break;
          direction = { x: -1, y: 0 };
          break;
        case "ArrowRight":
          if (direction.x === -1) break;
          direction = { x: 1, y: 0 };
          break;
      }
    });

    document.getElementById("up").addEventListener("click", () => {
      if (direction.y !== 1) direction = { x: 0, y: -1 };
    });
    document.getElementById("down").addEventListener("click", () => {
      if (direction.y !== -1) direction = { x: 0, y: 1 };
    });
    document.getElementById("left").addEventListener("click", () => {
      if (direction.x !== 1) direction = { x: -1, y: 0 };
    });
    document.getElementById("right").addEventListener("click", () => {
      if (direction.x !== -1) direction = { x: 1, y: 0 };
    });

    setInterval(gameLoop, gameSpeed);
  </script>
</body>
</html>
