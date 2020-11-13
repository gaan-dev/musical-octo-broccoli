<template>
  <div class="h-screen flex flex-col items-center justify-center ">
    <div>
      {{ topCard.value }}
      {{ topCard.suit }}
    </div>
    <div class="flex">
      <button class="bg-blue-200 rounded px-4 py-2" @click.prevent="higher">Higher</button>
      <button class="bg-red-200 rounded px-4 py-2" @click.prevent="lower">Lower</button>
    </div>
    <div>
      <button class="bg-orange-200 rounded px-4 py-2" @click.prevent="shuffle">Shuffle Deck</button>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import shuffle from 'lodash/shuffle';

  export default {
    
    name: 'CardGame',

    data(){
      return {
        cards: []
      }
    },

    computed:{
      topCard(){
        if(this.cards.length > 0){
          return this.cards[0];
        }
        return {suit: '', value: ''};
      },
      nextCard(){
        if(this.cards.length > 0){
          return this.cards[1];
        }
        return {suit: '', value: ''};
      }
    },

    methods:{
      higher(){

      },

      lower(){

      },

      shuffle(){
        this.cards = shuffle(this.cards);
      }
    },

    mounted(){
      axios.get('https://higher-lower.code23.com/api/deck')
        .then(response => {
          this.cards = shuffle(response.data);
        });
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
