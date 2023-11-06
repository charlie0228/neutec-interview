<template>
  <div>
    <div class="container">
      <div class="ballContainer">
        <div class="ball"></div>
        <div class="ball"></div>
        <div class="ball"></div>
        <div class="ball"></div>
      </div>

      <div class="blockContainer">
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
      加分題 1:
      <button ref="startBtn" @click="clickStartThreeHandler">Start</button>
      <button ref="resetBtn" @click="clickResetHandler">Reset</button>
    </div>

    <div class="container">
      加分題 2: <br>
      <br>
      支援 100 顆球的規劃上，我會使用 JS 進行位置的定位與移動的管理，就不會使用目前直接透過 CSS 進行動畫的設定。<br>
      <br>
      透過 JS 設計一套專門控制動畫的定位與移動的 Class，統一進行狀態管理，最後再調整。<br>
      <br>
      例如一個 array object 來記錄每顆球的原始位置，位移後的位置，以及球的移動 function，<br>
      <br>
      然後透過 vue 去 execute 這些 function，讓 vue 只需要控制開關，其他動畫都由 Class 來處理。
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
  width: 640px;
  margin: 12px;

  .ballContainer, .blockContainer {
    position: relative;
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: $gap;
  }

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
