<template>


  <header>
    <div id="header" >

      <img id="headerpic" src="https://thatsup.se/content/img/article/11/apr/uppsalas-b%C3%A4sta-barer.jpg">
      <h1 id="headertext"> Welcome to BurgerOnline </h1>
    </div>
  </header>
  <main>




    <section id="burgers" >
      <div class="burgershead">
        <h3>Select burgers </h3>
        <h4> This is where you execute burger selection</h4>
      </div>


      <div class="wrappers">
        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>

      </div>

    </section>


    <section  id="contact" >
      <div id="form">
        <h3> Costumer information</h3>
        <h4>This is where you provide necessary information</h4>
        <h3>Delivery information</h3>


        <form >
          <div>
            <label for="name">Full name</label><br>
            <input type="text" id="name" v-model="fn" required="required" placeholder="First and last name">
<!--            <span>typed: {{ fn }}</span>-->
          </div>

          <div>
            <label for="email">E-mail</label><br>
            <input type= "email" id="email" v-model="em" required="required" placeholder="E-mail address">
<!--            <span>typed: {{ em }}</span>-->
          </div>
<!--          <div>
            <label for="street">Street</label><br>
            <input type="text" id="street" v-model="st" required="required" placeholder="street Name">
            <span>typed: {{st }}</span>
          </div>
          <div>
            <label for="number">Number</label><br>
            <input type="number" id="number" v-model="nu" required="required" placeholder="House number">
            <span>typed: {{ nu }}</span>
          </div>-->
          <div>
            <label> payment</label><be>
            <select  v-model="selected" id="payment" >

              <option>Card</option>
              <option>Swish</option>
              <option selected="selected">  Invoice </option>
              <option>Klarna</option>


            </select> </be>
<!--            <span>Selected: {{ selected }}</span>-->



          </div>

          <div>
            <label >Gender</label><br>
            <input type="radio" id="female" v-model="gender" value="Female">
            <label for="female">Female </label><br>
            <input type="radio" id="male" v-model="gender" value="Male">
            <label for="male">Male </label><br>
            <input type="radio" id="nonbinary" v-model="gender" value="Non-binary">
            <label for="nonbinary">Non-binary </label><br>
            <input checked="checked" type="radio" id="notprovide" v-model="gender" value="Do not wish to provide" >
            <label for="notprovide">Do not wish to provide </label><br>
<!--            <span>Picked: {{ gender }}</span>-->
          </div>

        </form>
      </div>

      <div class="scrollingmap">

      <div id="map" v-on:click="setLocation">
        <div id="dots">
          <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" >
            T
          </div>
        </div>



      </div>

      </div>




    </section>


    <button v-on:click="submitorder(key)">
      <img src="https://cdn.pixabay.com/photo/2016/03/31/14/37/check-mark-1292787__480.png" style="width: 20px;">
      Submit the order
    </button>


  </main>
  <hr>
  <footer>
    <h7> &copy;</h7>
  </footer>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();

/*function MenuItem(name, url, kCal, gluten, lactose, price) {
  this.name=name;
  this.url=url;
  this.kCal=kCal;
  this.gluten=gluten;
  this.lactose=lactose;
  this.price=price;

}
const burger1= new MenuItem( 'Burger 1', "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR3nEQwZzCdMrsnYce1-ZS7IGc1I08lkWEIRg&usqp=CAU", 300, true, true, 145);
const burger2= new MenuItem( 'Burger 2', "https://gastrogate.com/thumbs/2000/files/31555/bc-burger-highres-10.jpg", 250, true, false, 125);
const burger3= new MenuItem( 'Burger 3', "https://joyfoodsunshine.com/wp-content/uploads/2017/04/black-bean-sweet-potato-veggie-burgers-1x1-1.jpg", 200, false, true, 110);


const allBurgers=[burger1, burger2, burger3]

console.log(allBurgers)*/

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      selected: "Invoice",
      gender:"Do not wish to provide",
      fn:"",
      em:"",
      st:"",
      nu:"",
     orderedBurgers: {},
      location: { x: 0,
        y: 0
      }


      /*send:""*/



    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addToOrder: function (event) {
      return this.orderedBurgers[event.name] = event.amount;
    },
    /*addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"],

                              }
                 );


    },*/
    submitorder: function(){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
            details: { x: this.location.x, y: this.location.y},
            orderItems: this.orderedBurgers,
        form: {selected: this.selected,
        gender: this.gender,
        fn: this.fn,
        em: this.em,
        st: this.st,
        nu: this.nu,}

          }
      );
      console.log(this.selected, this.gender, this.fn, this.em, this.st, this.nu, this.orderedBurgers)
      // this.gender="", this.fn="", this.em="", this.st="", this.nu=""

    },
    setLocation: function (event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      this.location= {x:event.clientX-10-offset.x, y: event.clientY - 10 - offset.y};

    }



  }

}
</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;

   background: url("/img/polacks.jpg");


  }
 .scrollingmap{
    width: 400px;
    height: 400px;
    overflow:scroll;

  }
  body {
    font-family:"fantasy";
    font-size: 18px;

  }


  #burgers{
    background-color: darksalmon;
    color: white;
    border-radius: 25px;

  }

  button{
    margin-left: 10px;
    margin-top: 5px;
  }
  button:hover {
    background-color: green;
    cursor: grabbing;

  }
  section{
    margin-left: 5px;
    margin-right: 5px;
  }
  .wrappers{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
    grid-template-rows: 100%;
    padding-left: 4.5em;

  }
  .amountButton{
    height: 25px;
    width: 50px;
    background-color: white;

  }
  .burgershead{
    grid-column: 1 /span 3;
    grid-row: 1;
    margin: 10px;
  }

  #header{
    overflow:hidden;
  }
  #headerpic{
    width: 100%;
    height: 300px;
    opacity: 60%;

  }
  #headertext{
    position: absolute;
    margin-top: -120px;
  }
  #contact{
    background-color: blanchedalmond;
    border-radius: 25px;
    border: 2px dotted darksalmon;
  /* display:grid;
    grid-template-rows: 50% 50%;
    grid-template-columns: 100%; */

  }
  #form{
    margin-left: 10px;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background: url(/img/polacks.jpg);
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  #dots div {
    position: relative;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>
