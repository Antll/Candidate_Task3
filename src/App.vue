<template>
  <div id="app">
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <br />

    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <br />

    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <br />

    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <div class="square"></div>
    <br />

    <button class="startBtn">Старт</button>

    <p id="timer">00:00:000</p>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
.square {
  display: inline-block;
  width: 100px;
  height: 100px;
  border: 1px solid grey;
}

.startBtn {
  width: 100px;
  margin-top: 10px;
}
</style>
