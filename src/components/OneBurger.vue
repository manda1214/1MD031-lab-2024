<template>
  <div class="burgeritem">
    <h4>{{ burger.name }}</h4>
    <img v-bind:src="burger.url" style="height:200px"><br>
    <ul>
      <li>The classic with a beef patty</li>
      <li v-if="burger.lactose">Contains <span class="allergener">lactose</span></li>
      <li v-if="burger.gluten">Contains <span class="allergener">gluten</span></li>
      <li>{{ burger.kCal}} kCal</li>
    </ul>
    <p>Amount ordered: {{ amountOrdered }}</p>
    <button class="addbutton" v-on:click="addBurger">
      +
    </button>
    <button class="subtractbutton" v-on:click="subtractBurger">
      -
    </button>
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
      this.$emit('orderedBurger', { name:this.burger.name, amount:this.amountOrdered 
      });
    },
    subtractBurger: function () {
      if (this.amountOrdered>0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', { name:this.burger.name, amount:this.amountOrdered
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
    padding: 0.5em;
  }
  .allergener { /*för class*/
    font-weight: bold;
  }
</style>