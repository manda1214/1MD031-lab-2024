<template> <!--vanlig html-->
<div>
  <header>
    <div class="headercontent">
      <img src="/img/backgroundheader.jpg" id="backgroundheader" alt="Header Image" title="Burger Bar">
      <h1 id="headertext">Welcome to BurgerOnline!</h1>
    </div>
  </header>
        
  <main>
    <section id="menu">
      <h3>Menu</h3>
      <p>Please enter the burgers you wish to order down below. </p>
      <div class="wrapper">
        <Burger v-for="burger in burgers"
        v-bind:burger="burger"
        v-bind:key="burger.name"
        v-on:orderedBurgers="addToOrder"></Burger> 
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

        <!-- Removing the street bc of the map
        <p>
          <label for="streetname">Street</label><br>
          <input type="text" id="streetname" v-model="st" required="required" placeholder="Street name">
        </p>
        <p>
          <label for="streetno">Street number</label><br>
          <input type="number" id="streetno" v-model="no" required="required" placeholder="Street number">
        </p>
        -->

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
          <div id="map" v-on:click="setLocation">
            <div id='position' v-bind:style="{ left: location.x + 'px', top: location.y + 'px', position: 'absolute'}">
              T
            </div>
          </div>
        </div>
      </form>
      <button class="sendbutton" type="submit" v-on:click="addOrder">
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
const spicyBurger = menu[3];
const fishBurger = menu[4];
const veganBurger = menu[5];

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ originalBurger, 
                 chickenBurger, 
                 halloumiBurger, 
                 spicyBurger,
                 fishBurger,
                 veganBurger
               ],
      fn: '',
      em: '',
      /*st: '',
      no: '', */
      gender: 'Do not wish to provide',
      pmm: 'Credit card',
      orderedBurgers:{},
      location: { x: 0, y: 0 },
      ordernumber: 1,
    }
  },
  methods: {
    getOrderNumber: function () {
      return this.ordernumber++;
    },
    addOrder: function (event) {
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                          name: this.fn,
                                          email: this.em, 
                                          paymentmethod: this.pmm,
                                          gender: this.gender, 
                                         },
                                orderItems: this.orderedBurgers
                              }
                 );
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name]=event.amount;
    },
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - offset.x;
      this.location.y = event.clientY - offset.y;
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@600&family=Pacifico&family=Satisfy&display=swap');
  body {
    font-size: 14pt;
  }
  h1 {
    font-family: 'Lora', serif;
    font-size: 40pt;
  }
  h3 {
    font-size: 20pt;
  }

  /* for the header */
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

  /* for the menu */
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
  .wrapper {
    display: grid;
    grid-gap: 1em;
    grid-template-columns: 15em 15em 15em;
    /*repeat(auto-fit, minmax(250px, 1fr));
    width: 100%;
    box-sizing: border-box; */
  }

  /* for customers to fill in */
  #customerinformation { 
    padding: 1em;
    background-color: #F5E6CC;
  }
  section#customerinformation {
    margin: 1em;
    padding: 1em;
    border: double;
    border-color: #556B2F;
    border-width: 10px;
  }
  .sendbutton {
    margin-top: 1em;
  }
  button:hover {
    background-color: #556B2F; /* changes color when hover over */
    color: white;
  }
  #map {
    position: relative;
    background: url("/img/polacks.jpg");
    width: 1920px;
    height: 1078px;
  }
  #picture {
    position: relative;
    width: 100%;
    height: 50vh;
    overflow: scroll;
  }
  #position {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>