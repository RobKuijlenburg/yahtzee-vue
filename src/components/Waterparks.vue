<template>
  <div class="hello">
    <button @click="throwDice()">Click me!!!!</button>
    <div>
      <p>
        You threw:
      </p>
      <table>
        <tr v-for="(dice, index) in diceArray" :key="dice-{index}">
          <td>
            {{index}}
          </td>
          <td>
            {{dice}}
          </td>
        </tr>
      </table>

    </div>
  </div>
</template>

<script>
const DICE_SIDES = 6;

export default {
  name: 'Waterparks',


  data() {
    return {
      diceArray: {},
    }
  },
  
  methods: {
    clearDice() {
      this.diceArray={};
    },

    throwDice() {
          for (let i = 0 ; i < 5 ; i++) {
          var dice = Math.floor(Math.random()*DICE_SIDES) + 1;
          this.diceArray[dice]++;
      }
      console.log(this.diceArray);
    },

    resetThrows() {
            this.diceArray = {};
            for (let side = 1; side <= DICE_SIDES; side++) {
                this.$set(this.diceArray, side, 0)
            }
    },

    fullHouse(obj, key){
        if(Object.keys(obj).includes(key)){
          console.log('full house');
        }

    },

    getSmStraightScore() {
        let joinDices = this.diceArray.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        let x = (/1234|2345|3456/.test(repDices));
        console.log('Small straight');
        return x ? 30 : 0;
    },

    getLgStraightScore() {
        let joinDices = this.diceArray.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        let x = (/12345|23456/.test(repDices));
        console.log('Large Straight');
        return x ? 40 : 0;
    }
  },
  
  props: {
  },

  computed: {
    containsFullHouse() {
      // return it so it can be processed
      // add logic: Object.values(this.diceArray) ....
      Object.values(this.diceArray).includes(3 && 2)
      return console.log('fullhouse');
    }
  },

  mounted() {
    this.resetThrows()
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

table {
  border: 1px solid #000;
}

td {
  padding: 5px 20px;
  border: 1px solid #000;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
