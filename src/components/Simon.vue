<template>
  <button @click="showMenu" class="start-button">Старт</button>
  <p class="level">{{this.level}}</p>
  <div class="container">
    <button v-bind:class="yellowClass" @click="buttonPress('yellow')">

    </button>
    <button v-bind:class="blueClass" @click="buttonPress('blue')">

    </button>
    <button v-bind:class="redClass" @click="buttonPress('red')">

    </button>
    <button v-bind:class="greenClass" @click="buttonPress('green')">

    </button>
    <div class="menu-bar" v-if="menu">
      <p class="level">Выбери сложность:</p>
      <button class="menu-button" @click="newGame(1, 1500)">Легкая</button>
      <button class="menu-button" @click="newGame(1, 1000)">Обычная</button>
      <button class="menu-button" @click="newGame(1, 400)">Сложная</button>
      <button class="menu-button" @click="hideMenu">Назад</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      game: 1,
      menu: false,
      number: 0,
      level: 'Уровень 1',
      time: 1000,
      colors: ['yellow', 'red', 'green', 'blue'],
      task: [],
      yellowClass: 'simon-button yellow',
      blueClass: 'simon-button blue',
      redClass: 'simon-button red',
      greenClass: 'simon-button green',
    };
  },
  methods: {
    buttonPress(color) {
      if (this.game === 1) {
        this[color + 'Class'] = `simon-button ${color}-active`;
        const audio = new Audio(require(`../assets/${color}.mp3`));
        audio.play();
        setTimeout(() => {
          this[color + 'Class'] = `simon-button ${color}`;
        
          if (this.task[this.number] === color) {
            this.number++;
          } else {
            this.level = `Ты проиграл. Твой счет - ${Number(this.level.slice(8)) - 1}`
            this.game = 0;
          }

          if (this.number === this.task.length) {
            setTimeout(() => {
              this.newGame(this.number+1, this.time);
            }, 1000);
          }
        }, 300);
      }
    },
    newGame(n, time) {
      this.menu = false;
      this.time = time;
      this.level = `Уровень ${n}`;
      if (this.game === 0) {
        this.task = []
      }
      this.task.push(this.colors[Math.floor(Math.random()*4)]);

      this.showTask(0);
    },
    showTask(n) {
      this.game = 1;
      this.number = 0;

      this[this.task[n] + 'Class'] = `simon-button ${this.task[n]}-active`;
        const audio = new Audio(require(`../assets/${this.task[n]}.mp3`));
        audio.play();
        setTimeout(() => {
          this[this.task[n] + 'Class'] = `simon-button ${this.task[n]}`;
          setTimeout(() => {
            if (n !== this.task.length - 1) {
              this.showTask(n+1)
            }
          }, this.time-200);
        }, 200);
    },
    showMenu() {
      this.menu = true;
      this.game = 0;
    },
    hideMenu() {
      this.menu = false;
      this.game = 1;
    }
  }
};
</script>

<style lang='scss'>
.container {
  width: 36vw;
  height: 36vw;
  margin: 0 auto;
  min-width: 300px;
  min-height: 300px;
}
.simon-button {
  width: 50%;
  height: 50%;
  float: left;
  cursor: pointer;
  border: 1vw solid black;
}
.yellow {
  background: radial-gradient(circle at 60% 65%, rgb(255, 255, 92), rgb(204, 204, 0));
  border-radius: 100% 0 0 0;
  border-width: 1vw .5vw .5vw 1vw;
}
.blue {
  background: radial-gradient(circle at 40% 65%, rgb(114, 114, 255), blue);
  border-radius: 0 100% 0 0;
  border-width: 1vw 1vw .5vw .5vw;
}
.red {
  background: radial-gradient(circle at 60% 35%, rgb(255, 94, 94), red);
  border-radius: 0 0 0 100%;
  border-width: .5vw .5vw 1vw 1vw;
}
.green {
  background: radial-gradient(circle at 40% 35%, rgb(121, 255, 121), green);
  border-radius: 0 0 100% 0;
  border-width: .5vw 1vw 1vw .5vw;
}
.yellow-active {
  background: radial-gradient(circle at 60% 65%, rgb(255, 255, 179), rgb(255, 255, 85));
  border-radius: 100% 0 0 0;
  border-width: 1vw .5vw .5vw 1vw;
}
.blue-active {
  background: radial-gradient(circle at 40% 65%, rgb(171, 171, 255), rgb(75, 75, 255));
  border-radius: 0 100% 0 0;
  border-width: 1vw 1vw .5vw .5vw;
}
.red-active {
  background: radial-gradient(circle at 60% 35%, rgb(255, 150, 150), rgb(255, 70, 70));
  border-radius: 0 0 0 100%;
  border-width: .5vw .5vw 1vw 1vw;
}
.green-active {
  background: radial-gradient(circle at 40% 35%, rgb(172, 255, 172), rgb(56, 151, 56));
  border-radius: 0 0 100% 0;
  border-width: .5vw 1vw 1vw .5vw;
}
.start-button {
  margin: 50px 0 20px 0;
  width: 10vw;
  height: 5vw;
  min-width: 100px;
  min-height: 50px;
  border-radius: 50vw;
  border: 3px solid black;
  font-size: 20px;
  background: rgb(255, 255, 255);
  cursor: pointer;
  &:hover {
    background: rgb(218, 218, 218);
  }
}
.menu-bar {
  width: 40vw;
  height: fit-content;
  position: absolute;
  top: 20vw;
  left: 30vw;
  background: #fff;
  border: 3px solid black;
}
.menu-button {
  width: 10vw;
  height: 5vw;
  min-width: 100px;
  min-height: 50px;
  border-radius: 50vw;
  border: 3px solid black;
  font-size: 20px;
  background: rgb(255, 255, 255);
  cursor: pointer;
  &:hover {
    background: rgb(218, 218, 218);
  }
  margin: 20px;
}
.level {
  font-size: 150%;
}
</style>