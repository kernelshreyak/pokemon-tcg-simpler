<script>  
import pokemon from 'pokemontcgsdk';
pokemon.configure({apiKey: '123abc'});

export default {
  data(){
    return {
      types: [
        "Colorless",
        "Darkness",
        "Dragon",
        "Fairy",
        "Fighting",
        "Fire",
        "Grass",
        "Lightning",
        "Metal",
        "Psychic",
        "Water"
      ],
      playerDeck: [],
      opponentDeck: [],
      searchResult: [],
      searchPage: 1,
      yourPokemon=null,
      opponentPokemon=null
    }
  },
  methods: {
    startSearch(){
      this.searchPage = 1;
      this.searchCard();
    },
    viewMore(){
      this.searchPage += 1;
      this.searchCard();
    },
    addCardToDeck(card){
      const whichdeck = document.getElementById('whichdeck').value;
      if(whichdeck == 1){
        this.playerDeck.push(card);
      }
      else this.opponentDeck.push(card);

      console.log(this.playerDeck,this.opponentDeck);
    },
    searchCard(){
      pokemon.card.where({ q: `name:"${document.getElementById('cardsearchtext').value}"`, pageSize: 10, page: this.searchPage })
      .then(result => {
          this.searchResult = result.data;
          console.log(result.data);
      })
    },
    getRandomEnergy(opponent=false){
      pokemon.card.where({ q: `name:"${this.types[Math.floor(Math.random()*this.types.length)]} Energy"`, pageSize: 1, page: 1 })
      .then(result => {
          opponent === true ? this.opponentDeck.push(result.data[0]) : this.playerDeck.push(result.data[0]);
      })
    }
  }
}
</script>

<template>
    <div class="container-fluid">
      <div class="card">
        <h3 align=center>Pokemon TCG Simpler</h3>
      </div>
      <div class="card deck">
        <h4>Build Deck</h4>
        
        <div class="row mt-4">
          <div class="col-lg-6">
            <input id="cardsearchtext" type="text" class="form-control" placeholder="Card name">
          </div> 
          <div class="col-lg-4">
            <select id="whichdeck" class="form-control">
              <option value="1">Your</option>
              <option value="2">Opponent</option>
            </select>
          </div>
          <div class="col-lg-2 mb-4">
            <button class="btn btn-success btn-sm" @click="startSearch">Search</button>
            <button class="btn btn-warning btn-sm" @click="viewMore">View More</button>
            <button class="btn btn-danger btn-sm" @click="() => {searchResult = []}">Clear</button>
          </div>
          
          <div class="col-lg-2" v-for="(card,card_index) of searchResult" :key="card_index">
            <img @click="() => {addCardToDeck(card)}" :src="card.images.small" alt="">
          </div>
        </div>

      </div>
      <div>
        <div class="row">
          <div class="col-lg-3 deck">
            <h4>Opponent's Deck</h4>
            <button @click="getRandomEnergy(true)" class="btn btn-primary btn-sm mb-3">Get random Energy</button>
            <div class="deck-cards">
              <div @click="" v-for="(card,card_index) of opponentDeck" :key="card_index">
                  <img @click="" :src="card.images.small" alt="">
              </div>
            </div>
          </div>
          <div class="col-lg-3">
            <h4>Opponent's Pokemon</h4>
            <div>
              <img @click="" :src="opponentPokemon.images.small" alt="" />
            </div>
          </div>
          <div class="col-lg-3">
            <h4>Your Pokemon</h4>
            <div>
              <img @click="" :src="yourPokemon.images.small" alt="" />
            </div>
          </div>
          <div class="col-lg-3 deck">
            <h4>Your Deck</h4>
            <button @click="getRandomEnergy" class="btn btn-primary btn-sm mb-3">Get random Energy</button>
            <div class="deck-cards">              
              <div @click="" v-for="(card,card_index) of playerDeck" :key="card_index">
                <img @click="" :src="card.images.small" alt="">
              </div>
            </div>
            
          </div>
        </div>
      </div>
      
    </div>
</template>

<style>
  .deck{
    margin: 1.5rem;
    padding: 1rem;
  }
  .deck-cards{
    width: 300px;
    max-height: 500px;
    overflow-y: auto;
  }
</style>
