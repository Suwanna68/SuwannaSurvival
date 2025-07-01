<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Suwanna Survival</title>
  <style>
    body { margin: 0; overflow: hidden; background: #111; font-family: sans-serif; color: #fff; }
    #game {
      width: 100vw; height: 100vh; position: relative;
      background: url('https://t3.ftcdn.net/jpg/00/88/98/18/360_F_88981880_YjJManMJ6hJmKr5CZteFJAkEzXIh8mxW.jpg') no-repeat center center/cover;
    }
    #player {
      width: 60px; height: 60px;
      position: absolute; bottom: 20px; left: 50%;
      transform: translateX(-50%);
    }
    .obstacle {
      width: 50px; height: 50px;
      background: url('https://png.pngtree.com/png-clipart/20230913/original/pngtree-red-devil-png-image_11062566.png') no-repeat center center/contain;
      position: absolute; top: 0;
    }
    #score {
      position: absolute; top: 10px; left: 10px;
      font-size: 20px;
      color: #fff;
      z-index: 10;
    }
    #startScreen {
      position: absolute; width: 100%; height: 100%; top: 0; left: 0;
      background: rgba(0, 0, 0, 0.8);
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      color: white; font-size: 32px;
      z-index: 20;
    }
    #startBtn {
      margin-top: 20px;
      padding: 12px 24px;
      font-size: 20px;
      background: limegreen;
      border: none; border-radius: 8px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="game">
    <div id="startScreen">
      <div>🚀 เริ่มเกม Suwanna Survival</div>
      <button id="startBtn">เริ่มเกม</button>
    </div>
    <img id="player" src="Logoกลมๆ.png" alt="Logo" />
    <div id="score">Score: 0</div>
  </div>

  <audio id="hitSound" src="https://www.soundjay.com/button/sounds/button-16.mp3"></audio>
  <audio id="bgMusic" src="https://www.bensound.com/bensound-music/bensound-epic.mp3" loop></audio>

  <script>
    const player = document.getElementById("player");
    const game = document.getElementById("game");
    const scoreDisplay = document.getElementById("score");
    const hitSound = document.getElementById("hitSound");
    const bgMusic = document.getElementById("bgMusic");
    const startScreen = document.getElementById("startScreen");
    const startBtn = document.getElementById("startBtn");

    let pos = window.innerWidth / 2;
    let score = 0;
    let speed = 2;
    let gameInterval;

    function startGame() {
      startScreen.style.display = 'none';
      bgMusic.volume = 0.5;
      bgMusic.play();
      gameInterval = setInterval(spawnObstacle, 1000);
    }

    startBtn.addEventListener('click', startGame);

    function spawnObstacle() {
      const obs = document.createElement("div");
      obs.classList.add("obstacle");
      obs.style.left = Math.random() * (window.innerWidth - 50) + "px";
      game.appendChild(obs);
      let top = 0;

      const fall = setInterval(() => {
        top += speed;
        obs.style.top = top + "px";

        const playerRect = player.getBoundingClientRect();
        const obsRect = obs.getBoundingClientRect();
        if (
          obsRect.bottom > playerRect.top &&
          obsRect.left < playerRect.right &&
          obsRect.right > playerRect.left
        ) {
          hitSound.play();
          clearInterval(fall);
          clearInterval(gameInterval);
          alert("Game Over! Final Score: " + score);
          location.reload();
        }

        if (top > window.innerHeight) {
          obs.remove();
          clearInterval(fall);
          score++;
          scoreDisplay.textContent = "Score: " + score;
          if (score % 10 === 0) speed += 0.5;
        }
      }, 20);
    }

    function movePlayer(direction) {
      if (direction === 'left') pos -= 20;
      if (direction === 'right') pos += 20;
      pos = Math.max(0, Math.min(pos, window.innerWidth - 60));
      player.style.left = pos + "px";
    }

    document.addEventListener("keydown", e => {
      if (e.key === "ArrowLeft") movePlayer('left');
      if (e.key === "ArrowRight") movePlayer('right');
    });

    document.addEventListener("touchstart", e => {
      const touchX = e.changedTouches[0].clientX;
      if (touchX < window.innerWidth / 2) movePlayer('left');
      else movePlayer('right');
    });
  </script>
</body>
</html>
