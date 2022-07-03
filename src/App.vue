<script>  
import pokemon from 'pokemontcgsdk';
pokemon.configure({apiKey: '123abc'});

export default {
  data(){
    return {
      playerDeck: [],
      opponentDeck: [],
      searchResult: [],
      searchPage: 1
    }
  },
  methods: {
    viewMore(){
      this.searchPage += 1;
      this.searchCard();
    },
    searchCard(){
      pokemon.card.where({ q: `name:"${document.getElementById('cardsearchtext').value}"`, pageSize: 10, page: this.searchPage })
      .then(result => {
          this.searchResult = result.data;
      })
    }
  },
  mounted(){
    console.log("loaded");
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
        
        <div class="row">
          <div class="col-lg-6">
            <input id="cardsearchtext" type="text" class="form-control" placeholder="Card name">
          </div> 
          <div class="col-lg-4">
            <select class="form-control" name="" id="">
              <option value="">Your</option>
              <option value="">Opponent</option>
            </select>
          </div>
          <div class="col-lg-2">
            <button class="btn btn-success" @click="searchCard">Search</button>
            <button class="btn btn-warning" @click="viewMore">View More</button>
          </div>
          <br /><br />
          <div class="col-lg-2" v-for="card of searchResult">
            <img :src="card.images.small" alt="">
          </div>
        </div>

      </div>
      <div>
        <div class="card deck">
          <h4>Opponent's Deck</h4>
        </div>
        <div class="card deck">
          <h4>Your Deck</h4>
        </div>
      </div>
      
    </div>
</template>

<style>
  .deck{
    margin: 1.5rem;
    padding: 1rem;
  }
</style>
