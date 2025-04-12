index.html

<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Flappy Bird</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <canvas id="game-canvas" width="400" height="600"></canvas>
  <script src="script.js"></script>
</body>
</html>

styles.css

body {
  background-color: #f0f0f0;
}

#game-canvas {
  border: 1px solid #ccc;
}

script.js

const canvas = document.getElementById('game-canvas');
const ctx = canvas.getContext('2d');

// Game variables
let birdX = 100;
let birdY = 200;
let birdVel = 0;
let gravity = 0.2;
let pipeX = 400;
let pipeY = Math.random() * 300;
let pipeGap = 150;
let score = 0;
let gameOver = false;

// Draw bird
function drawBird() {
  ctx.fillStyle = '#fff';
  ctx.fillRect(birdX, birdY, 20, 20);
}

// Draw pipe
function drawPipe() {
  ctx.fillStyle = '#0f0';
  ctx.fillRect(pipeX, 0, 50, pipeY);
  ctx.fillRect(pipeX, pipeY + pipeGap, 50, 600 - pipeY - pipeGap);
}

// Update game state
function update() {
  birdVel += gravity;
  birdY += birdVel;

  pipeX -= 2;

  if (pipeX < -50) {
    pipeX = 400;
    pipeY = Math.random() * 300;
  }

  if (birdY > 580 || birdY < 0) {
    gameOver = true;
  }

  if (pipeX < birdX + 20 && pipeX + 50 > birdX && (birdY < pipeY || birdY + 20 > pipeY + pipeGap)) {
    gameOver = true;
  }

  if (pipeX + 50 === birdX && birdY > pipeY && birdY < pipeY + pipeGap) {
    score++;
  }
}

// Draw game
function draw() {
  ctx.clearRect(0, 0, 400, 600);
  ctx.fillStyle = '#87CEEB';
  ctx.fillRect(0, 0, 400, 600);

  drawBird();
  drawPipe();

  ctx.fillStyle = '#fff';
  ctx.font = '24px Arial';
  ctx.textAlign = 'left';
  ctx.textBaseline = 'top';
  ctx.fillText(`Score: ${score}`, 10, 10);

  if (gameOver) {
    ctx.font = '48px Arial';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    ctx.fillText('Game Over!', 200, 300);
  }
}

// Main loop
function loop() {
  update();
  draw();

  if (!gameOver) {
    requestAnimationFrame(loop);
  }
}

// Handle keyboard input
document.addEventListener('keydown', (e) => {
  if (e.key === ' ') {
    birdVel = -6;
  }
});

// Start game
loop();
