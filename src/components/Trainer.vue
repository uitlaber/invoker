<template>
  <div class="trainer">
    <div
      class="point"
      v-if="point > 0 && !started"
      v-text="
        'Вы заработали ' +
          point +
          ' очко за 30 сек. Совершили ' +
          mistakes +
          ' ошибок!'
      "
    ></div>
    <div class="casts" v-if="started">
      <div class="casts__item" v-for="cast in casts">
        <img
          :src="sphere.icon"
          alt=""
          v-for="sphere in spheres"
          v-if="cast == sphere.key"
        />
      </div>
    </div>
    <img
      class="invoker"
      src="https://avatanplus.com/files/resources/mid/5670093fe95a8151a5a421d4.png"
    />
    <div class="spells" v-show="">
      <div class="spells__item" v-for="spell in spells">
        <img :src="spell.icon" alt="" />
        <p v-text="spell.name"></p>
      </div>
    </div>

    <div class="buttons">
      <div class="buttons__item" v-for="sphere in spheres">
        <img :src="sphere.icon" />
      </div>
      <div class="buttons__item">
        <img :src="ready_spells[0].icon" alt="" v-if="ready_spells[0]" />
      </div>
      <div class="buttons__item">
        <img :src="ready_spells[1].icon" alt="" v-if="ready_spells[1]" />
      </div>

      <div class="buttons__item">
        <img
          src="https://www.invokergame.com/images/spells/invoke.png"
          alt=""
        />
      </div>
    </div>
    <div class="random_spell" v-if="started">
      <img :src="random_spell.icon" alt="" v-if="random_spell" />
    </div>
    <button class="button-start" @click="startGame();" v-if="!started">
      Старт
    </button>
  </div>
</template>

<script>
/*
  Q: 81,
  W: 87,
  E: 69,
  R: 82
  */
export default {
  name: "Trainer",
  created() {
    var self = this;
    window.addEventListener("keyup", function(event) {
      if (event.keyCode == 81 || event.keyCode == 87 || event.keyCode == 69) {
        if (self.casts.length == 3) {
          self.casts.shift();
          self.casts.push(event.keyCode);
        } else {
          self.casts.push(event.keyCode);
        }
      } else if (event.keyCode == 82) {
        var audio = new Audio(
          "https://d1u5p3l4wpay3k.cloudfront.net/dota2_gamepedia/c/c7/Invoke.mp3"
        ); // path to file

        audio.play();
        self.invoke();
      }
    });
  },
  methods: {
    invoke() {
      if (!this.started) return;

      var spell = this.invokeSpell();
      if (this.ready_spells.length < 2) {
        this.ready_spells.unshift(spell);
        if (this.random_spell == spell) {
          this.point++;
          this.loadRandom();
        } else {
          this.mistakes++;
        }
      } else {
        if (this.ready_spells[0] != spell) {
          this.ready_spells.splice(-1, 1);
          this.ready_spells.unshift(spell);
          if (this.random_spell == spell) {
            this.point++;
            this.loadRandom();
          } else {
            this.mistakes++;
          }
        }
      }
    },
    invokeSpell() {
      var self = this;
      return self.spells.find(x => x.keys === self.casts.join());
    },
    startGame() {
      var self = this;
      self.started = true;
      this.loadRandom();
      setTimeout(function() {
        self.started = false;
      }, 30 * 1000);
    },
    loadRandom() {
      var self = this;
      if (self.started) {
        var spell =
          self.spells[Math.floor((Math.random() * self.spells.length) | 0)];
        var exists = self.ready_spells.some(function(obj) {
          return obj.keys === spell.keys;
        });

        if (exists) {
          self.loadRandom();
          return;
        }

        self.random_spell = spell;
      } else {
        self.random_spell = null;
      }
    }
  },
  data() {
    return {
      started: false,
      point: 0,
      mistakes: 0,
      spheres: [
        {
          key: 81,
          icon: "https://www.invokergame.com/images/spells/quas.png",
          button: "Q"
        },
        {
          key: 87,
          icon: "https://www.invokergame.com/images/spells/wex.png",
          button: "W"
        },
        {
          key: 69,
          icon: "https://www.invokergame.com/images/spells/exort.png",
          button: "E"
        }
      ],
      casts: [],

      ready_spells: [],

      random_spell: null,

      spells: [
        {
          name: "Cold Snap",
          keys: "81,81,81",
          icon: "https://www.invokergame.com/images/spells/cold_snap.png"
        },
        {
          name: "Ghost Walk",
          keys: "81,81,87",
          icon: "https://www.invokergame.com/images/spells/ghost_walk.png"
        },
        {
          name: "Ice Wall",
          keys: "81,81,69",
          icon: "https://www.invokergame.com/images/spells/ice_wall.png"
        },
        {
          name: "EMP",
          keys: "87,87,87",
          icon: "https://www.invokergame.com/images/spells/emp.png"
        },
        {
          name: "Tornado",
          keys: "87,87,81",
          icon: "https://www.invokergame.com/images/spells/tornado.png"
        },
        {
          name: "Alacrity",
          keys: "87,87,69",
          icon: "https://www.invokergame.com/images/spells/alacrity.png"
        },
        {
          name: "Sun Strike",
          keys: "69,69,69",
          icon: "https://www.invokergame.com/images/spells/sun_strike.png"
        },
        {
          name: "Forge Spirit",
          keys: "69,69,81",
          icon: "https://www.invokergame.com/images/spells/forge_spirit.png"
        },
        {
          name: "Chaos Meteor",
          keys: "69,69,87",
          icon: "https://www.invokergame.com/images/spells/chaos_meteor.png"
        },
        {
          name: "Deafening Blast",
          keys: "81,87,69",
          icon: "https://www.invokergame.com/images/spells/deafening_blast.png"
        }
      ]
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.trainer {
  background: #fff;
  max-width: 410px;
  margin: 0 auto;
}
.spells {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
}
.casts {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  max-width: 136px;
  margin: 0 auto 25px;
}

.casts__item img {
  width: 32px;
  height: 32px;
  border-radius: 100%;
  border: 3px solid #55800085;
}

.invoker {
  max-width: 100%;
}

.buttons {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
  max-width: 410px;
  margin: 0 auto;
}

.buttons__item {
  background: #000;
  width: 60px;
  height: 60px;
}

.buttons__item img {
  width: 100%;
}

.random_spell img {
  margin: 40px;
  border: 5px solid #6d8086;
}

.button-start {
  border: none;
  font-size: 25px;
  background: #eee;
  margin: 20px 0;
}

.point {
  font-size: 22px;
  font-weight: 700;
  color: red;
}
</style>
