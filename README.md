//# SapceAaarohon
//This is a game for astronomy lover. In this game you can explore many thing of the world. 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rocket Game</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <canvas id="gameCanvas" width="600" height="400"></canvas>
  <script src="game.js"></script>
</body>
</html>


body {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: black;
}

canvas {
  border: 1px solid white;
}


const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const rocketWidth = 40;
const rocketHeight = 60;
let rocketX = canvas.width / 4;
let rocketY = canvas.height / 2;
let rocketSpeed = 5;
let gravity = 0.2;
let velocityY = 0;

const obstacles = [];
let score = 0;

document.addEventListener('keydown', moveRocket);

function moveRocket(e) {
  if (e.key === 'ArrowUp' || e.key === ' ') {
    velocityY = -5; // Move rocket up
  }
}

function drawRocket() {
  ctx.fillStyle = 'white';
  ctx.fillRect(rocketX, rocketY, rocketWidth, rocketHeight);
}

function createObstacle() {
  const obstacleWidth = 50;
  const obstacleHeight = 30;
  const obstacleX = canvas.width;
  const obstacleY = Math.random() * (canvas.height - obstacleHeight);

  obstacles.push({ x: obstacleX, y: obstacleY, width: obstacleWidth, height: obstacleHeight });
}

function drawObstacles() {
  for (let i = 0; i < obstacles.length; i++) {
    const obs = obstacles[i];
    ctx.fillStyle = 'red';
    ctx.fillRect(obs.x, obs.y, obs.width, obs.height);

    obs.x -= 3; // Obstacle moves to the left

    if (obs.x + obs.width < 0) {
      obstacles.splice(i, 1); // Remove off-screen obstacles
      score++;
    }

    // Collision detection
    if (rocketX < obs.x + obs.width && rocketX + rocketWidth > obs.x &&
      rocketY < obs.y + obs.height && rocketY + rocketHeight > obs.y) {
      gameOver();
    }
  }
}

function updateRocketPosition() {
  velocityY += gravity;
  rocketY += velocityY;

  // Prevent rocket from going out of bounds
  if (rocketY + rocketHeight > canvas.height) {
    rocketY = canvas.height - rocketHeight;
    velocityY = 0;
  }
  if (rocketY < 0) {
    rocketY = 0;
    velocityY = 0;
  }
}

function gameOver() {
  alert("Game Over! Final Score: " + score);
  document.location.reload();
}

function update() {
  ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear the canvas

  drawRocket();
  drawObstacles();
  updateRocketPosition();

  requestAnimationFrame(update);
}

// Generate obstacles every 2 seconds
setInterval(createObstacle, 2000);

update(); // Start the game loop





