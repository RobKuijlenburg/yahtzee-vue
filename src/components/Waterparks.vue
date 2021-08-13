<template>
  <div class="hello">
    <button @click="throwDice()">Click me!!!!</button>
    <div>
      <p>
        You threw:
      </p>
      <table>
        <tr v-for="(dice, index) in diceArray" :key="index">
          <td>
            {{index}}
          </td>
          <td>
            {{dice}}
          </td>
        </tr>

        <tr>
          <td>
            Three of a kind
          </td>
          <td>
            {{containsThreeOfAKind}}
          </td>
        </tr>

        <tr>
          <td>
            Four of a kind
          </td>
          <td>
            {{containsFourOfAKind}}
          </td>
        </tr>

        <tr>
          <td>
            Fullhouse
          </td>
          <td>
            {{containsFullHouse}}
          </td>
        </tr>

        <tr>
          <td>
            Small straight
          </td>
          <td>
            {{getSmStraightScore}}
          </td>
        </tr>

        <tr>
          <td>
            Large straight
          </td>
          <td>
            {{getLgStraightScore}}
          </td>
        </tr>

        <tr>
          <td>
            Yahtzee
          </td>
          <td>
            {{containsYahtzee}}
          </td>
        </tr>

        <tr>
          <td>
            Chance
          </td>
          <td>
            {{getChance}}
          </td>
        </tr>

        <tr>
          <td>
            Total
          </td>
          <td>
            {{getTotal}}
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
      straightCheck: [],
      semiTotal: 0,
      total: 0
    };

  },
  
  methods: {
    throwDice() {
          for (let i = 0 ; i < 5 ; i++) {
          var dice = Math.floor(Math.random()*DICE_SIDES) + 1;
          this.diceArray[dice]++;
          this.semiTotal += dice;
          this.straightCheck.push(dice);
      }
      this.straightCheck.sort();
      this.total = this.semiTotal + this.containsThreeOfAKind + this.containsFullHouse + this.containsFourOfAKind + this.containsYahtzee + this.getSmStraightScore + this.getLgStraightScore;
      console.log(this.diceArray);
      console.log(this.semiTotal);
      console.log(this.straightCheck)
    },

    resetThrows() {
            this.diceArray = {};
            for (let side = 1; side <= DICE_SIDES; side++) {
                this.$set(this.diceArray, side, 0)
            }
            console.log(this.diceArray);
    },
  },
  
  props: {
  },

  computed: {
    containsFullHouse() {
      // return it so it can be processed
      // add logic: Object.values(this.diceArray) ....
      if(Object.values(this.diceArray).includes(3) && Object.values(this.diceArray).includes(2)){
        return 25;
      } else {
        return 0;
      }
    },

    containsThreeOfAKind() {
      if (Object.values(this.diceArray).includes(3)){
        return this.semiTotal;
      } else {
        return 0;
      }
    },

    containsFourOfAKind() {
      if (Object.values(this.diceArray).includes(4)){
        return this.semiTotal;
      } else {
        return 0;
      }
    },

    getSmStraightScore() {
        let joinDices = this.straightCheck.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        let x = (/1234|2345|3456/.test(repDices));
        if (x === true) {
           console.log('Small straight');
          return 30;
        } else {
          return 0;
        }
       
    },

    getLgStraightScore() {
        let joinDices = this.straightCheck.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        let x = (/12345|23456/.test(repDices));
        if (x === true) {
          console.log('Large straight');
          return 40;
        } else {
          return 0;
        }
    },

    containsYahtzee() {
      if (Object.values(this.diceArray).includes(5)){
        return 50;
      } else {
        return 0;
      }
    },

    getChance() {
      return this.semiTotal;
    },

    getTotal(){
      return this.total;
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
