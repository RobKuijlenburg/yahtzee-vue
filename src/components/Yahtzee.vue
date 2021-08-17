<template>
  <div class="hello">
    <button @click="throwDice">Click me!!!!</button>
    <div>
      <p>
        You threw:
      </p>
      <table>
        <tr v-for="(count, die) in counter" :key="die">
          <td>
            {{die}}
          </td>
          <td>
            {{count}}
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

export default {
  name: 'Yahtzee',


  data() {
    return {
      thrownDice:[],
    };
  },
  
  methods: {
    throwDice() {
      this.thrownDice = [];
      for (let i = 0 ; i < 5 ; i++) {
          const dice = Math.floor(Math.random()*DICE_SIDES) + 1;
          this.thrownDice.push(dice);
      }
      this.thrownDice.sort();      
    },
  },
  
  computed: {
    semiTotal(){
      return this.thrownDice.reduce(reducer, 0);
    },

    counter(){
      const counter = {}
      for (let side = 1; side <= DICE_SIDES; side++) {
        counter[side] = this.thrownDice.filter(die => die === side).length
      }
      return Object.values(counter);
    },

    containsFullHouse() {
      if(this.counter.includes(3) && this.counter.includes(2)){
        return 25;
      }  
      return 0;
    },

    containsThreeOfAKind() {
      if (this.counter.includes(3)){
        return this.semiTotal;
      }
      return 0;
    },

    containsFourOfAKind() {
      if (this.counter.includes(4)){
        return this.semiTotal;
      } 
      return 0;
    },

    getSmStraightScore() {
        const joinDices = this.thrownDice.join('');
        const repDices = joinDices.toString().replace(/(.)\1/,'$1');
        return (/1234|2345|3456/.test(repDices)) ? 30 : 0;

    },

    getLgStraightScore() {
      const joinDices = this.thrownDice.join('');
      const repDices = joinDices.toString().replace(/(.)\1/,'$1');
      return (/12345|23456/.test(repDices)) ? 40 : 0;
    },

    containsYahtzee() {
      if (this.counter.includes(5)){
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
