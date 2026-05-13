<script setup>
import { ref, onMounted } from 'vue'

const ballX = ref(390)
const ballY = ref(190)

const speedX = ref(4)
const speedY = ref(3)

const leftPaddleY = ref(150)
const rightPaddleY = ref(150)

const fieldHeight = 400
const paddleHeight = 100
const ballSize = 20

const leftScore = ref(0)
const rightScore = ref(0)

let gameInterval = null

// движение мяча
function moveBall() {
  ballX.value += speedX.value
  ballY.value += speedY.value

  if (ballY.value <= 0 || ballY.value >= fieldHeight - ballSize) {
    speedY.value *= -1
  }

  if (ballX.value < 0) {
    rightScore.value++
    resetBall()
  }

  if (ballX.value > 800) {
    leftScore.value++
    resetBall()
  }
}

// вернуть мяч в центр
function resetBall() {
  ballX.value = 390
  ballY.value = 190
  speedX.value = 4
  speedY.value = 3
}

// управление ракетками
function handleKeyDown(event) {
  const key = event.key.toLowerCase()

  if (key === 'w' || key === 'ц') {
    if (leftPaddleY.value > 0) {
      leftPaddleY.value -= 20
    }
  }

  if (key === 's' || key === 'ы') {
    if (leftPaddleY.value < fieldHeight - paddleHeight) {
      leftPaddleY.value += 20
    }
  }

  if (event.key === 'ArrowUp') {
    if (rightPaddleY.value > 0) {
      rightPaddleY.value -= 20
    }
  }

  if (event.key === 'ArrowDown') {
    if (rightPaddleY.value < fieldHeight - paddleHeight) {
      rightPaddleY.value += 20
    }
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
  gameInterval = setInterval(moveBall, 20)
})
</script>

<template>
  <div class="game-wrapper">
    <h1>Пинг-Понг</h1>

    <div class="score">
      {{ leftScore }} : {{ rightScore }}
    </div>

    <div class="field">
      <div
        class="paddle left-paddle"
        :style="{ top: leftPaddleY + 'px' }"
      ></div>

      <div
        class="paddle right-paddle"
        :style="{ top: rightPaddleY + 'px' }"
      ></div>

      <div
        class="ball"
        :style="{
          left: ballX + 'px',
          top: ballY + 'px'
        }"
      ></div>
    </div>
  </div>
</template>

<style scoped>
.game-wrapper {
  text-align: center;
  font-family: sans-serif;
}

.score {
  font-size: 30px;
  margin: 20px 0;
}

.field {
  width: 800px;
  height: 400px;
  border: 1px solid black;
  margin: 0 auto;
  position: relative;
  background: white;
}

.paddle {
  width: 15px;
  height: 100px;
  background: black;
  position: absolute;
}

.left-paddle {
  left: 0;
}

.right-paddle {
  right: 0;
}

.ball {
  width: 20px;
  height: 20px;
  background: red;
  border-radius: 50%;
  position: absolute;
}
</style>