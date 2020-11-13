<template>
  <div class="h-screen flex items-center justify-center ">
    Card game
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    
    name: 'CardGame',

    data(){
      return {
        cards: []
      }
    },

    methods:{
      //https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array
      shuffleCards(cards){
        for (let i = cards.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [cards[i], cards[j]] = [cards[j], cards[i]];
        }
        return cards;
      }
    },

    mounted(){
      axios.get('https://higher-lower.code23.com/api/deck')
        .then(response => {
          this.cards = this.shuffleCards(response.data);
        });
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
