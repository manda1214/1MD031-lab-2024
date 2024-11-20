<template> <!--vanlig html-->
<div>
  <header>
    <div class="headercontent">
      <img src="/img/backgroundheader.jpg" id="backgroundheader" alt="Header Image" title="Burger Bar">
      <h1 id="headertext">Order Burgers Online</h1>
    </div>
  </header>
        
  <main>
    <section id="menu">
      <div class="wrapper">
        <Burger v-for="burger in burgers"
        v-bind:burger="burger"
        v-bind:key="burger.name"></Burger>
      </div>
    </section>

    <section id="customerinformation">
      <h3>Customer Information</h3>
      <p>
          Please add information below for delivery. 
      </p>
      <h4>Delivery Information</h4>
      <form>
        <p>
          <label for="name">Full Name</label><br>
          <input type="text" id="name" v-model="fn" required="required" placeholder="First and Last name">
        </p>
        <p>
          <label for="email">E-mail</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>
        <p>
          <label for="streetname">Street</label><br>
          <input type="text" id="streetname" v-model="st" required="required" placeholder="Street name">
        </p>
        <p>
          <label for="streetno">Street number</label><br>
          <input type="number" id="streetno" v-model="no" required="required" placeholder="Street number">
        </p>
        <p>
          <label for="gender">Gender</label><br>
          <input type="radio" id="male" value="Male" v-model="gender">
          <label for="gender">Male</label><br>
          <input type="radio" id="female" value="Female" v-model="gender">
          <label for="gender">Female</label><br>
          <input type="radio" id="donotwishtoprovide" value="Do not wish to provide" v-model="gender" checked="checked">
          <label for="gender">Do not wish to provide</label><br>
        </p>
        <p>
          <label for="payment">Payment Method</label><br>
          <select id="payment" v-model="pmm">
            <option>Credit card</option>
            <option>Swish</option>
            <option>Klarna</option>
            <option>Cash</option>
          </select>
        </p>
        <div id="picture">
          <div id="map" v-on:click="addOrder">
            click here
          </div>
        </div>
      </form>
      <button class="sendbutton" type="submit" v-on:click="console.log(this.fn, this.em, this.st, this.no, this.gender, this.pmm, this.orderedBurgers)">
        Place order 
        <img src="/img/sendbutton.png" style="height:15px">
      </button>
    </section>
  </main>

  <hr> <!-- Linjen längst ner innan copyright -->
  <footer>
    &copy;2024 BurgerOnline
  </footer>
</div>
</template>

<script> //JS
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

/*
 
function MenuItem(name, url, kCal, gluten, lactose) {
  this.name = name;
  this.img = url;
  this.kCal = kCal
  this.gluten = gluten;
  this.lactose = lactose;
}

const originalBurger = new MenuItem('Original Burger', '/img/originalburger.jpg', 400, true, true);
const chickenBurger = new MenuItem('Chicken Burger', '/img/chickenburger.webp', 500, true, true);
const halloumiBurger = new MenuItem('Halloumi Burger', '/img/halloumiburger.jpg', 450, true, true);
*/

const originalBurger = menu[0];
const chickenBurger = menu[1];
const halloumiBurger = menu[2];

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ originalBurger, 
                 chickenBurger, 
                 halloumiBurger
               ],
      fn: '',
      em: '',
      st: '',
      no: '',
      gender: 'Do not wish to provide',
      pmm: 'Credit card',
      orderedBurgers:{},
      location: { x: 0,
            y: 0
          },
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    background: url("/img/polacks.jpg");
    width: 1920px;
    height: 1078px;
  }

  #picture {
    width: 100%;
    height: 50vh;
    overflow: scroll;
  }

  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@600&family=Pacifico&family=Satisfy&display=swap');
  body {
    font-size: 14pt;
  }

  /*
  p {
    color: #333333;
  }*/

  h1 {
    font-family: 'Lora', serif;
    font-size: 40pt;
  }

  .headercontent {
    margin: 1em;
    height: 200px;
    overflow: hidden;
  }

  #backgroundheader {
    opacity: 0.8; 
    width: 100%;
    height: auto;
  }
  #headertext {
    position: absolute;
    margin-top: -45%;
    text-align: center;
    width: 100%;
  }

  .allergener { /*för class*/
    font-weight: bold;
  }
  
  #customerinformation { /*för id*/
    padding: 1em;
    background-color: #F5E6CC;
  }

  #menu {
    background-color: #FAF8F1;
  }

  section#menu {
    color: #333333;
    margin: 1em;
    padding: 1em;
    border: double;
    border-color: #556B2F;
    border-width: 10px;
  }

  section#customerinformation {
    color: #4E3629;
    margin: 1em;
    padding: 1em;
    border: double;
    border-color: #556B2F;
    border-width: 10px;
  }

  .wrapper {
    display: grid;
    grid-gap: 0.5em;
    grid-template-columns: 14em 14em 14em;
  }
  .burgeritem {
    margin: 0.5em;
    padding: 0.5em;
    /*border-style: double;
    border-color: #556B2F;
    border-width: 10px;
    width: 12em;
    height: 20em;*/
  }

  .sendbutton {
    margin-top: 1em;
  }
  button:hover {
    background-color: #556B2F; /* changes color when hover over */
    color: white;
  }
</style>