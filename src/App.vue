<template>
  <div>
    <div class="container">
      <div class="ball"></div>
      <div class="ball"></div>
      <div class="ball"></div>
      <div class="ball"></div>

      <div class="block"></div>
      <div class="block"></div>
      <div class="block"></div>

      <div class="block"></div>
      <div class="block"></div>
      <div class="block"></div>

      <div class="block"></div>
      <div class="block"></div>
      <div class="block"></div>
    </div>

    <div class="container">
      Method 1:
      <button ref="startBtn" @click="clickStartOneHandler">Start</button>
      <button ref="resetBtn" @click="clickResetHandler">Reset</button>
    </div>

    <div class="container">
      Method 2:
      <button ref="startBtn" @click="clickStartTwoHandler">Start</button>
      <button ref="resetBtn" @click="clickResetHandler">Reset</button>
    </div>

    <div class="container">
      Method 3 (to right bottom):
      <button ref="startBtn" @click="clickStartThreeHandler">Start</button>
      <button ref="resetBtn" @click="clickResetHandler">Reset</button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
const startBtn = ref<HTMLButtonElement | null>(null)
const resetBtn = ref<HTMLButtonElement | null>(null)

const clickResetHandler  = () => {
  document.querySelectorAll('.ball').forEach(ball => {
    ball.classList.remove('move1')
    ball.classList.remove('move2')
    ball.classList.remove('move3')
  })
}

const clickStartOneHandler  = () => {
  clickResetHandler()

  document.querySelectorAll('.ball').forEach(ball => {
    ball.classList.add('move1')
  })
}

const clickStartTwoHandler  = () => {
  clickResetHandler()

  document.querySelectorAll('.ball').forEach(ball => {
    ball.classList.add('move2')
  })
}

const clickStartThreeHandler  = () => {
  clickResetHandler()

  document.querySelectorAll('.ball').forEach(ball => {
    ball.classList.add('move3')
  })
}
</script>
<style lang="scss" scoped>
$block-width: 200px;
$block-height: 100px;
$block-border-width: 2px;
$ball-size: 30px;
$gap: 12px;

@mixin ball-position($x, $y) {
  top: calc(($block-height + $block-border-width * 2) * ($y - 0.5) + $gap * ($y - 1) - $ball-size * 0.5);
  left: calc(($block-width + $block-border-width * 2) * ($x - 0.5) + $gap * ($x - 1) - $ball-size * 0.5);
}

.container {
  position: relative;
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  width: 640px;
  margin: 12px;
  gap: $gap;

  .block {
    position: relative;
    flex: 0 0 auto;
    width: $block-width;
    height: $block-height;
    border: black solid $block-border-width;
    background: radial-gradient(circle, rgba(113,81,95,1) 81%, rgba(0,0,0,1) 100%);

    &:nth-child(3),
    &:nth-child(5),
    &:nth-child(9) {
      animation: blink 1000ms infinite;
    }
  }

  .ball {
    position: absolute;
    width: $ball-size;
    height: $ball-size;
    background-color: #A5F12B;
    border-radius: 100%;
    z-index: 1;

    &:nth-child(1) {
      @include ball-position(1, 1);
    }

    &:nth-child(2) {
      @include ball-position(3, 1);
    }

    &:nth-child(3) {
      @include ball-position(1, 3);
    }

    &:nth-child(4) {
      @include ball-position(3, 3);
    }

    &.move1 {
      animation: move1 1500ms linear;
      animation-fill-mode: forwards;
    }

    &.move2 {
      animation: move2 1500ms linear;
      animation-fill-mode: forwards;
    }

    &.move3 {
      animation: move3 1500ms linear;
      animation-fill-mode: forwards;
    }
  }
}

@keyframes blink {
  0% {
    opacity: 1;
  }

  50% {
    opacity: 0.6;
  }

  100% {
    opacity: 1;
  }
}

@keyframes move1 {
  100% {
    margin-left: 500px;
  }
}

@keyframes move2 {
  100% {
    transform: translateX(500px);
  }
}

@keyframes move3 {
  100% {
    top: 321px;
    left: 621px;
  }
}
</style>
