<template>
  <div>
    <div class="row">
      <div class="col">
        <strong class="text-center text-danger"
          >Click anywhere to generate a new ball!</strong
        >
        <canvas
          id="myCanvas"
          v-bind:width="screenWidth"
          v-bind:height="screenHeight"
          v-on:click="createBall()"
        ></canvas>
      </div>
      <div class="col">
        <div id="sidebar">
          <div
            v-for="ball in ballsArray"
            :key="ball.id"
            class="mb-3"
            style="font-size: 0.7rem"
          >
            <strong class="d-block" v-bind:style="{ color: ball.color }"
              >Ball #{{ ball.id }}
            </strong>
            <span class="d-block">
              <strong>
                Radius:
              </strong>
              {{ ball.radius }}px
            </span>
            <span class="d-block">
              <strong>
                Pos:
              </strong>
              ({{ ball.posX.toFixed(0) }}, {{ ball.posY.toFixed(0) }})
            </span>
            <span class="d-block">
              <strong>
                Vel:
              </strong>
              ({{ ball.velX.toFixed(1) }}, {{ ball.velY.toFixed(1) }})
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ctx: null,
      screenWidth: window.innerWidth - 150,
      screenHeight: window.innerHeight,
      ballsArray: [],
      gravity: 0.2
    };
  },
  methods: {
    draw() {
      this.ctx = document.getElementById("myCanvas").getContext("2d");
      this.ctx.clearRect(0, 0, this.screenWidth, this.screenHeight);

      this.ballsArray.forEach(ball => {
        this.ctx.beginPath();
        this.ctx.fillStyle = ball.color;

        ball.velY += this.gravity;

        if (ball.posX - ball.radius <= 0) {
          ball.velX = -ball.velX * 0.9;
          ball.posX = ball.radius;
        } else if (ball.posX + ball.radius >= this.screenWidth) {
          ball.velX = -ball.velX * 0.9;
          ball.posX = this.screenWidth - ball.radius;
        }

        if (ball.posY - ball.radius <= 0) {
          ball.velY = -ball.velY * 0.9;
          ball.posY = ball.radius;
        } else if (ball.posY + ball.radius >= this.screenHeight) {
          ball.velY = -ball.velY * 0.9;
          ball.posY = this.screenHeight - ball.radius;
        }

        ball.posX += ball.velX;
        ball.posY += ball.velY;

        this.ctx.arc(ball.posX, ball.posY, ball.radius, 0, Math.PI * 2);
        this.ctx.closePath();
        this.ctx.fill();
      });
    },
    createBall() {
      let newBall = {
        id: this.ballsArray.length + 1,
        color: this.getRandomColor(),
        radius: this.getRandomInt(25, 50),
        posX: this.screenWidth / 2,
        posY: this.screenHeight - 100,
        velX: this.getRandomInt(-5, 5),
        velY: this.getRandomInt(-12, -7)
      };

      this.ballsArray.push(newBall);
    },
    getRandomColor() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    getRandomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    }
  },
  mounted() {
    setInterval(() => {
      this.draw();
    }, 10);
  }
};
</script>
