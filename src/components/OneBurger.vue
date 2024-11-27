<template>
  <div class="burgeritem">
    <h4>{{ burger.name }}</h4>
    <img v-bind:src="burger.url" style="width:200px"><br>
    <ul>
      <li>{{burger.text}}</li>
      <li v-if="burger.lactose">Contains <span class="allergener">lactose</span></li>
      <li v-if="burger.gluten">Contains <span class="allergener">gluten</span></li>
      <li>{{ burger.kCal}} kCal</li>
    </ul>
    <p>Amount to order: {{ amountOrdered }} 
      <button class="amountbutton" v-on:click="addBurger">
        +
      </button>
      <button class="amountbutton" v-on:click="subtractBurger">
        -
      </button>
    </p>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    addBurger: function () {
      this.amountOrdered++;
      this.$emit('orderedBurgers', { name:this.burger.name, amount:this.amountOrdered
      },
    );
    },
    subtractBurger: function () {
      if (this.amountOrdered>0) {
        this.amountOrdered--;
        this.$emit('orderedBurgers', { name:this.burger.name, amount:this.amountOrdered
        });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .burgeritem {
    margin: 0.5em;
    padding: 1em;
    border-style: groove;
    border-color: #5c2c13;
    border-width: 8px;
    align-items: center; 
  }
  .burgeritem h4 {
  text-align: center;
  }
  .allergener { /*för class*/
    font-weight: bold;
  }
  .amountbutton {
    background-color: #556B2F;
    border-color: #5c2c13;
    color: #FAF8F1;
    margin: 0.1em;
    width: 2em;
    height: 2em;
  }
  .amountbutton:hover {
    background-color: #FAF8F1; /* changes color when hover over */
    color:#556B2F;
  }
</style>