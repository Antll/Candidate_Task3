<template>
  <div id="app">
    <div v-on:click="guess(1)" v-bind:style="{ background: elementColors[0][1] }"></div>
    <div v-on:click="guess(2)" v-bind:style="{ background: elementColors[1][1] }"></div>
    <div v-on:click="guess(3)" v-bind:style="{ background: elementColors[2][1] }"></div>
    <div v-on:click="guess(4)" v-bind:style="{ background: elementColors[3][1] }"></div>
    <br />

    <div v-on:click="guess(5)" v-bind:style="{ background: elementColors[4][1] }"></div>
    <div v-on:click="guess(6)" v-bind:style="{ background: elementColors[5][1] }"></div>
    <div v-on:click="guess(7)" v-bind:style="{ background: elementColors[6][1] }"></div>
    <div v-on:click="guess(8)" v-bind:style="{ background: elementColors[7][1] }"></div>
    <br />

    <div v-on:click="guess(9)" v-bind:style="{ background: elementColors[8][1] }"></div>
    <div v-on:click="guess(10)" v-bind:style="{ background: elementColors[9][1] }"></div>
    <div v-on:click="guess(11)" v-bind:style="{ background: elementColors[10][1] }"></div>
    <div v-on:click="guess(12)" v-bind:style="{ background: elementColors[11][1] }"></div>
    <br />

    <div v-on:click="guess(13)" v-bind:style="{ background: elementColors[12][1] }"></div>
    <div v-on:click="guess(14)" v-bind:style="{ background: elementColors[13][1] }"></div>
    <div v-on:click="guess(15)" v-bind:style="{ background: elementColors[14][1] }"></div>
    <div v-on:click="guess(16)" v-bind:style="{ background: elementColors[15][1] }"></div>
    <br />

    <button class="startBtn">Старт</button>

    <p id="timer">00:00:000</p>
  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      colors: [],
      emptyElementColor: [
        'none',
        'none',
        0,
      ],

      elementColors: [],
    };
  },

  methods: {
    generate() {
      const colorsCount = 8;
      const elementsCount = 16;
      const count = [0, 0, 0, 0, 0, 0, 0, 0];

      for (let i = 0; i < colorsCount; i += 1) {
        this.colors.push([`#${(0x1000000 + (Math.random() * 0xFFFFFF)).toString(16).substr(1, 6)}`, 'none', 0]);
      }

      for (let i = 0; i < elementsCount; i += 1) {
        let pickedColor = Math.floor(Math.random() * colorsCount);
        let isColored = 0;

        while (!isColored) {
          if (count[pickedColor] < 2) {
            const color = this.colors[pickedColor]; // Need a copy, not a reference
            this.elementColors.push([color[0], color[1], color[0]]);
            count[pickedColor] += 1;
            isColored = 1;
          } else {
            pickedColor = Math.floor(Math.random() * colorsCount);
          }
        }
      }
    },

    guess(number) {
      this.elementColors[number - 1][1] = this.elementColors[number - 1][0];
      this.$forceUpdate();
    },
  },

  created() {
    this.generate();
  },
};
</script>

<style>
#app div {
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
