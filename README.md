<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mini Platformer</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    canvas { display: block; background: linear-gradient(skyblue, lightgreen); margin: auto; }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="480" height="320"></canvas>  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");

    const player = {
      x: 50,
      y: 250,
      width: 30,
      height: 30,
      dx: 0,
      dy: 0,
      speed: 2,
      jumpPower: -5,
      onGround: false
    };

    const gravity = 0.2;

    const keys = {
      left: false,
      right: false,
      up: false
    };

    const platforms = [
      { x: 0, y: 300, width: 480, height: 20 },
      { x: 100, y: 250, width: 80, height: 10 },
      { x: 200, y: 200, width: 80, height: 10 },
      { x: 300, y: 150, width: 80, height: 10 },
    ];

    function drawPlayer() {
      ctx.fillStyle = "red";
      ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawPlatforms() {
      ctx.fillStyle = "brown";
      platforms.forEach(p => ctx.fillRect(p.x, p.y, p.width, p.height));
    }

    function updatePlayer() {
      player.dx = 0;
      if (keys.left) player.dx = -player.speed;
      if (keys.right) player.dx = player.speed;

      player.dy += gravity;

      player.x += player.dx;
      player.y += player.dy;

      player.onGround = false;
      for (let p of platforms) {
        if (
          player.x < p.x + p.width &&
          player.x + player.width > p.x &&
          player.y + player.height < p.y + 10 &&
          player.y + player.height + player.dy >= p.y
        ) {
          player.y = p.y - player.height;
          player.dy = 0;
          player.onGround = true;
        }
      }

      if (keys.up && player.onGround) {
        player.dy = player.jumpPower;
      }
    }

    function gameLoop() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawPlatforms();
      updatePlayer();
      drawPlayer();
      requestAnimationFrame(gameLoop);
    }

    document.addEventListener("keydown", (e) => {
      if (e.key === "ArrowLeft") keys.left = true;
      if (e.key === "ArrowRight") keys.right = true;
      if (e.key === "ArrowUp") keys.up = true;
    });

    document.addEventListener("keyup", (e) => {
      if (e.key === "ArrowLeft") keys.left = false;
      if (e.key === "ArrowRight") keys.right = false;
      if (e.key === "ArrowUp") keys.up = false;
    });

    gameLoop();
  </script></body>
</html>
