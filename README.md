const player = document.getElementById("player");
const platform = document.getElementById("platform");
const score = document.getElementById("score");
const obstacles = document.querySelector(".obstacles");

let playerBottom = parseInt(getComputedStyle(player).bottom);
let playerRight = parseInt(getComputedStyle(player).right);
let playerWidth = parseInt(getComputedStyle(player).width);
let platformBottom = parseInt(getComputedStyle(platform).bottom);
let platformHeight = parseInt(getComputedStyle(platform).height);
let jumping = false;
let scoreValue = 0;

function jump() {
  if (jumping) return;
  jumping = true;

  const jumpHeight = platformHeight + 250;
  const jumpDuration = 20;
  const jumpStep = 10;

  const upTime = setInterval(() => {
    if (playerBottom >= jumpHeight) {
      clearInterval(upTime);
      const downTime = setInterval(() => {
        if (playerBottom <= platformHeight + 10) {
          clearInterval(downTime);
          jumping = false;
        }

        playerBottom -= jumpStep;
        player.style.bottom = playerBottom + "px";
      }, jumpDuration);
    }

    playerBottom += jumpStep;
    player.style.bottom = playerBottom + "px";
  }, jumpDuration);
}

function scoreCounter() {
  scoreValue++;
  score.innerHTML = scoreValue;
}

setInterval(scoreCounter, 100);

function obstacleGen() {
  const obstacle = document.createElement("div");
  obstacle.className = "obstacle";
  obstacles.appendChild(obstacle);

  const randomTime = Math.floor(Math.random() * 500) + 900;
  let obstacleRight = -30;
  let obstacleBottom = 100;
  const obstacleHeight = Math.floor(Math.random() * 70) + 50;
  const obstacleWidth = parseInt(getComputedStyle(obstacle).width);

  function moveObstacle() {
    obstacleRight += 5;
    obstacle.style.right = obstacleRight + "px";
    obstacle.style.height = obstacleHeight + "px";
    obstacle.style.width = obstacleWidth + "px";
    obstacle.style.bottom = obstacleBottom + "px";

    if (
      playerRight >= obstacleRight - playerWidth &&
      playerRight <= obstacleRight + obstacleWidth &&
      playerBottom <= obstacleBottom + obstacleHeight
    ) {
      clearInterval(obstacleInterval);
      clearTimeout(obstacleTimer);
      alert("Game Over");
      location.reload();
    }
  }

  const obstacleInterval = setInterval(moveObstacle, 10);
  const obstacleTimer = setTimeout(obstacleGen, randomTime);
}

obstacleGen();

function controller(e) {
  if (e.keyCode === 32 || e.key === " ") {
    jump();
  }
}

document.addEventListener("keydown", controller);
