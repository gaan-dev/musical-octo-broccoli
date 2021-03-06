<template>
  <div class="h-screen flex flex-col items-center justify-center ">
    <div class="flex" v-if="!dead">
      {{ this.lives }} Lives | Score: {{ this.score }}
    </div>
    <div v-else>
      YOU LOSE!
    </div>
    <div>
      Highest Score: {{ this.highestScore }}
    </div>
    <div class="rounded border border-solid border-black shadow-lg my-4 p-4 relative w-64 h-128 flex items-center justify-center">
      <div class="absolute left-0 top-0 p-2 font-bold flex flex-col" :style="{ color: color }">
        {{ topCard.value }}
      </div>
      <div class="absolute right-0 bottom-0 p-2 font-bold" :style="{ color: color }">
        {{ topCard.value }}
      </div>
      <div class="font-bold text-2xl" :style="{ color: color }">
        {{ topCard.suit }}
      </div>
    </div>
    <div class="mt-2 mb-4">
      Will the next card be higher or lower?
    </div>
    <div class="flex">
      <button class="bg-blue-200 rounded px-4 py-2 mr-1" v-if="!dead" @click.prevent="higher">Higher</button>
      <button class="bg-red-200 rounded px-4 py-2 ml-1" v-if="!dead" @click.prevent="lower">Lower</button>
    </div>
    <div class="my-2">
      <button class="bg-orange-200 rounded px-4 py-1 text-sm" @click.prevent="reset">Shuffle Deck (Reset)</button>
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
        cards: [],
        lives: 3,
        score: 0,
        highScore: 0,
        icons:{
          diamonds:{
            icon: 'spades.png',
            color: 'red'
          },
          hearts:{
            icon: 'hearts.png',
            color: 'red'
          },
          clubs: {
            icon: 'clubs.png',
            color: 'black',
          },
          spades: {
            icon: 'spades.png',
            color: 'black'
          }
        }
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
      },

      highestScore(){
        if(this.highScore === null){
          return 0;
        }
        return this.score > this.highScore ? this.score : this.highScore;
      },

      dead(){
        return this.lives == 0;
      },

      color(){
        if(this.topCard.suit !== ''){
          return this.icons[this.topCard.suit].color;
        }
        return 'black';
      },

      icon(){
        if(this.topCard.suit !== ''){
          return this.icons[this.topCard.suit].icon;
        }
        return null;
      }

    },

    watch:{
      highestScore(oldVal, newVal){
        window.localStorage.setItem('high_score', this.highestScore);
      }
    },

    methods:{
      convertValue(card){
        let valueMap = {'A': 1, 'J':11, 'Q': 12, 'K': 13};
        if(!isNaN(parseInt(card.value))){
          return parseInt(card.value);
        }
        return valueMap[card.value];
      },

      higher(){
        let a = this.convertValue(this.topCard);
        let b = this.convertValue(this.nextCard);

        if(a >= b){
          this.lives--;
        }else{
          this.score++;
        }

        this.cycleTopToBot();
      },

      lower(){
        let a = this.convertValue(this.topCard);
        let b = this.convertValue(this.nextCard);

        if(a <= b){
          this.lives--;
        }else{
          this.score++;
        }
        
        this.cycleTopToBot();
      },

      cycleTopToBot(){
        let c = this.cards.shift(this.cards);
        this.cards.push(c);
      },

      reset(){
        this.cards = shuffle(this.cards);
        this.score = 0;
        this.lives = 3;
      },
    },

    mounted(){
      axios.get('https://higher-lower.code23.com/api/deck')
        .then(response => {
          this.cards = shuffle(response.data);
        });
      this.highScore = window.localStorage.getItem('high_score');
      // window.localStorage.getItem('high_score') ? window.localStorage.getItem('high_score') : 0;
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
