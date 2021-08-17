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
const reducer = (accumulator, currentValue) => accumulator + currentValue;
// TODO :: use const instead of let where possible

export default {
  name: 'Yahtzee',


  data() {
    return {
      diceArray: {},

      // TODO :: can all be computed properties
      straightCheck: [],
    };
  },
  
  methods: {
    throwDice() {
      this.resetThrows();
      for (let i = 0 ; i < 5 ; i++) {
          var dice = Math.floor(Math.random()*DICE_SIDES) + 1;
          this.diceArray[dice]++;
          this.straightCheck.push(dice);
      }
      this.straightCheck.sort();      
    },



    resetThrows() {
            this.straightCheck = [];
            this.diceArray = {};
            for (let side = 1; side <= DICE_SIDES; side++) {
                this.$set(this.diceArray, side, 0)
            }
    }
  },
  
  props: {
  },

  computed: {
    semiTotal(){
      if (this.straightCheck.length != 0){
      return this.straightCheck.reduce(reducer);
      }
      return 0;
    },

    objectValues(){
      return Object.values(this.diceArray);
    },

      containsFullHouse() {

      if(this.objectValues.includes(3) && this.objectValues.includes(2)){
        return 25;
      }  
        return 0;
    },

    containsThreeOfAKind() {
      if (this.objectValues.includes(3)){
        return this.semiTotal;
      }
        return 0;
    },

    containsFourOfAKind() {
      if (this.objectValues.includes(4)){
        return this.semiTotal;
      } 
        return 0;
    },

    getSmStraightScore() {
        let joinDices = this.straightCheck.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        return (/1234|2345|3456/.test(repDices))? 30 : 0;

    },

    getLgStraightScore() {
        let joinDices = this.straightCheck.join('');
        let repDices = joinDices.toString().replace(/(.)\1/,'$1');
        return (/12345|23456/.test(repDices))? 40 : 0;
    },

    containsYahtzee() {
      if (this.objectValues.includes(5)){
        return 50;
      } 
        return 0;
    
    },


    getChance() {
      return this.semiTotal;
    },

    getTotal(){
      return this.semiTotal + this.containsThreeOfAKind + this.containsFullHouse + this.containsFourOfAKind + this.containsYahtzee + this.getSmStraightScore + this.getLgStraightScore;
    },
  },

  mounted() {
    this.resetThrows()
  }
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
