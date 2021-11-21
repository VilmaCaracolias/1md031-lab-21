<template>

  <div class="burgers" >

    <h3> {{ burger.name }}</h3>

    <img class="burgers" v-bind:src="burger.url" style="height: 180px">

    <ul>
      <li> {{burger.kCal}} kCal</li>
      <span v-if="burger.meat==true"> <li> Contains <span class="ingredients"> meat</span></li></span>
      <span v-if="burger.lactose==true"> <li>  Contains <span class="ingredients"> lactose  </span> </li> </span>
      <span v-if="burger.gluten==true"> <li>  Contains <span class="ingredients"> gluten  </span> </li> </span>
      <li>  Price {{burger.price }} kr </li>

    </ul>
    <div >
      Amount:{{amountOrdered}}
     <button v-on:click= "decreaseOrder"> - </button>
      <button v-on:click= "increaseOrder"> + </button>

    </div>


  </div>


</template>
<div>

</div>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object,


  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
      methods: {

      increaseOrder: function () {
        this.amountOrdered += 1
        this.$emit('orderedBurger', { name:   this.burger.name,
              amount: this.amountOrdered
            }
        );
        console.log(  this.amountOrdered, this.burger.name)
      },


      decreaseOrder: function () {
        if (this.amountOrdered>0) {
          this.amountOrdered -= 1
          console.log(  this.amountOrdered, this.burger.name)
        }
        this.$emit('orderedBurger', { name:   this.burger.name,
              amount: this.amountOrdered
            }
        );
      }


      }


  }



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ingredients{
  color: white;
  font-weight: bold;
}
#orderButton{



}

</style>
