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

    <button class="startBtn" v-on:click="generate()">Старт</button>
  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      // Последние два элемента, на которые кликнули
      elementsPair: [-1, -1],

      // Должна ли пара быть перекрашена в цвет по-умолчанию
      toDissapear: false,

      // Общее количество пар
      pairsCount: 8,

      // Количество оставшихся неотгаданных пар
      pairsRemained: 8,

      /**
       * Каждый элемент - вектор из 3-ёх елементов:
       * Первое строковое значение - угадываемый цвет
       * Второе строковое значение - отображаемый цвет (изначально цвет по-умолчанию)
       * Третье булево значение - скрыть ли угадываемый цвет
       */
      colors: [],
      elementColors: [],
    };
  },

  methods: {
    /**
     * Сгенериовать 8 случайных цветов
     * и для каждого присвоить 1 цвет 2-ум случайно
     * расположенным элементам
     */
    generate() {
      console.log('Generated');
      const colorsCount = this.pairsCount;
      const elementsCount = 16;
      const count = [0, 0, 0, 0, 0, 0, 0, 0];

      this.pairsRemained = this.pairsCount;

      for (let i = 0; i < colorsCount; i += 1) {
        if (this.colors.length < colorsCount) {
          this.colors.push([`#${(0x1000000 + (Math.random() * 0xFFFFFF)).toString(16).substr(1, 6)}`, 'none', false]);
        } else {
          this.colors[i] = [`#${(0x1000000 + (Math.random() * 0xFFFFFF)).toString(16).substr(1, 6)}`, 'none', false];
        }
      }

      for (let i = 0; i < elementsCount; i += 1) {
        let pickedColor = Math.floor(Math.random() * colorsCount);
        let isColored = 0;

        while (!isColored) {
          if (count[pickedColor] < 2) {
            const color = this.colors[pickedColor]; // Need a copy, not a reference

            if (this.elementColors.length < elementsCount) {
              this.elementColors.push([color[0], color[1], color[2]]);
            } else {
              this.elementColors[i] = [color[0], color[1], color[2]];
            }

            count[pickedColor] += 1;
            isColored = 1;
          } else {
            pickedColor = Math.floor(Math.random() * colorsCount);
          }
        }
      }
    },

    /**
     * По клику если в текущий момент не отображаетс пара
     * с временным отображением цвета, то показать цвет,
     * иначе проверяется угадана ли пара, и, если да, то
     * цвета остаются, иначе обратно перекрашиваются
     *
     * Также если неугаданных пар не осталось, то выводится
     * сообщение
     */
    guess(number) {
      if (!this.toDissapear) {
        console.log(`${number} had to show ${this.elementColors[number - 1][0]}`);
        this.elementColors[number - 1][1] = this.elementColors[number - 1][0];
      }

      if (this.elementsPair[0] === -1) {
        console.log('Prev set');
        this.elementsPair[0] = number - 1;
      } else if (this.toDissapear === false) {
        console.log('Dissapear set');
        this.elementsPair[1] = number - 1;
        this.toDissapear = true;
      } else if (this.toDissapear === true) {
        console.log('Reset');

        if (
          (this.elementColors[this.elementsPair[0]][1]
          === this.elementColors[this.elementsPair[1]][1])
          && (this.elementsPair[0] !== this.elementsPair[1])
        ) {
          this.elementColors[this.elementsPair[0]][2] = true;
          this.elementColors[this.elementsPair[1]][2] = true;
          this.pairsRemained -= 1;
        }

        if (this.elementColors[this.elementsPair[0]][2] === false) {
          this.elementColors[this.elementsPair[0]][1] = 'none';
        }

        if (this.elementColors[this.elementsPair[1]][2] === false) {
          this.elementColors[this.elementsPair[1]][1] = 'none';
        }

        this.elementsPair[0] = -1;
        this.elementsPair[1] = -1;
        this.toDissapear = false;

        console.log(`Pairs remained: ${this.pairsRemained}`);
        if (this.pairsRemained === 0) {
          alert('Вы выйграли!');
        }
      }

      // Необходимо для отображение изменённого цвета
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
