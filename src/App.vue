<template lang="PT-BR">
  
  <div id="app">

    <div class="container">
      <div class="row text-center">
        <div class="col-12">
          <img src="./assets/pokedex_logo.png" class="img-fluid w-50" alt="Pokemon Logo">
        </div>
      </div>
    
   
   <div class="container text-center">
    <div class="row form-outline my-5 p-4">
      <div class="col-12">
        <input v-model="search" type="text" class="form-control border border-warning" placeholder="Pesquisar Pokemon" aria-label="Search" />
      </div>
      </div>
   </div>

 <div class="container">
   <div class="row row-cols-3 row-cols-md-4 row-cols-lg-5 g-4">
      <div class="col" v-for="(pokemon, index) in filter_pokemon" :key="index">
        <div class="card shadow-sm bg-light text-center border border-warning" @click="open(get_id(pokemon))">
          <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/${get_id(pokemon)}.png`"  class="card-img-fluid mx-auto w-50 h-50" :alt='pokemon.name'>
          <div class="card-body">
            <h5 class="card-title">{{pokemon.name}}</h5>
           </div>
        </div>
      </div>
   </div>
  </div>

  <div class="container" v-if="showModal">
    <div  class="modal fade show d-block" tabindex="-1" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
       <div class="modal-content shadow-lg p-3 mb-5 bg-body rounded border border-2 border-warning">
         <div class="modal-header">
            <div class="container">
              <div class="row">
                <button @click="close()" type="button" class="btn-close" data-mdb-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="row d-flex align-items-center">
                <div class="col-5">
                    <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selectpokemon.id}.png`"
                  class="img-thumbnail rounded border border-warning" width="200">
                </div>
                <div class="col-7">
                  <h1 class="modal-title">{{selectpokemon.name}}</h1>
                 <div>
                    <label class="fs-6">Experience:</label> <span class="badge bg-warning text-dark mx-1 p-2">{{selectpokemon.base_experience}}</span>
                </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-body">
            <div class="container">
              <div class="row">
                <label>Habilidades:</label>
                <div class="col-12">
                  <span v-for="(item, index) in selectpokemon.abilities" :key="index" class="badge bg-warning text-dark m-2 p-2">{{item.ability.name}}</span>
                 </div>
              </div>
              <hr>
              <div class="row">
                <label class="mb-3">Movimentos:</label>
             
              </div>

              <div class="list-group">
                <a href="#" v-for="(item in selectpokemon.moves" :key="item.move.name" class="list-group-item list-group-item-action list-group-item-light">
                  {{item.move.name}}</a>
              </div>

            </div>
           
          </div>
          <div class="modal-footer">
            <button @click="close()" type="button" class="btn btn-danger mx-auto">Fehar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {},

data ()  {
  return {
  
  search: '',
  listpokemons: [],
  selectpokemon: [],
  showModal: false

  
  }
},

 async mounted(){
  // GET request using axios with async/await
  await  axios.get('https://pokeapi.co/api/v2/pokemon/?limit=1125')
  .then((response) => {
    this.listpokemons = response.data.results;
    //console.log(response.data.results);
  });
},

methods: {
  get_id(listpokemons){
   return (listpokemons.url.split('/')[6]);
  },

 open(id){
    axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
  .then((response) => {
    this.selectpokemon = response.data;
    console.log(response.data);
  });
  this.showModal = !this.showModal
},

close(){
  this.showModal = !this.showModal
}

},

computed: {
  filter_pokemon(){
    return this.listpokemons.filter((item) =>{
    return item.name.includes(this.search);
    })
  }
}

};
</script>

<style scoped>
#app {

  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}


.card{
  cursor: pointer;
  border-radius: 10px;
  transition: transform .2s;
}

.card:hover{
  transform: scale(1.05); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */

}


</style>