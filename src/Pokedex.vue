<template>
  <div class="pokedex">
    <a href="/"><img  class="titleImg" src="./assets/Poketitle.png" alt="" ></a>
    
    <RangeSlider @range-updated="recive"></RangeSlider>
    <div class="filter-section">
      <label for="tipo">Filtrar por tipo:</label>
      <select class="select" id="tipo" v-model="tipoSeleccionado">
        <option value="todos" >todos</option>
        <option v-for="tipo in Object.keys(pokemonsPorTipo)" :value="tipo" :key="tipo">{{ tipo }}</option>

      </select>
    </div>
    <h2>Componentes Pokemon:</h2>
    <div class="pokemon-grid">
      <Pokemon v-for="pokemon in pokemonsFiltrados" @ISLoad="recive2" :key="pokemon" :pokemonNumber="pokemon"  onchange="" @BtnTrigger="recive3" />
    </div>
  </div>
  <div class="btn" @click="Pokefav = true"> <img src="https://static.overlay-tech.com/assets/7082ecf7-2d2a-446f-bf53-4de72085fe84.svg" alt=""> </div>
  <div class="btn2" @click="Poketeam = true"> 
    <p class="x-5">{{ equipo.length }}/{{ MAX_EQUIPO }}</p>
    
    </div>
</template>

<script>
import Pokemon from './Pokemon.vue';
import RangeSlider from './slider.vue';
export default {
  name: 'Pokedex',
  components: {
    Pokemon, RangeSlider
  },
  data() {
    return {
      pokemonComponents: [],
      equipo: [], // Array de nombres de Pokémon para el equipo
      Poketeam: false,
      Pokefav: false,
      favoritos: [], // Array de nombres de Pokémon favoritos
      MAX_EQUIPO: 6,
      tipoSeleccionado: 'todos',
      pokemonsPorTipo: {}, // Objeto para almacenar los IDs de Pokémon por tipo
      to:0,
      from:151,
    };
  },
  mounted() {
  setTimeout(() => {
    //this.handlePokemonUpdated();
    // console.log(this.pokemonsPorTipo);
  }, 12000);
  
},
  methods: {
    recive(data)
    {
      this.to = data.to;
      this.from = data.from;      
    },
    recive2(data)
    {
      // console.log(data.pokemonId);  
      // console.log(data.tipo);  
      this.agregarPokemonPorTipo(data.tipo,data.pokemonId);  
    },

    recive3(data) {
  const pokemonId = data.pokemonId;
  if (data.btn === 1) {
    // Comprobar si el pokemonId está en la lista de favoritos
    if (this.favoritos.includes(pokemonId)) {
      // Si está en la lista, eliminarlo
      const index = this.favoritos.indexOf(pokemonId);
      this.favoritos.splice(index, 1);
    } else {
      // Si no está en la lista, agregarlo
      this.favoritos.push(pokemonId);
    }
    console.warn("FAV");
  } else {
    // Comprobar si el pokemonId está en el equipo
    if (this.equipo.includes(pokemonId)) {
      // Si está en el equipo, eliminarlo
      const index = this.equipo.indexOf(pokemonId);
      this.equipo.splice(index, 1);
    } else {
      // Si no está en el equipo, comprobar si el equipo está lleno
      if (this.equipo.length < this.MAX_EQUIPO) {
        // Si el equipo no está lleno, agregar el pokemonId
        this.equipo.push(pokemonId);
      } else {
        // Si el equipo está lleno, mostrar un alert dialog
        // Aquí debes implementar la lógica para mostrar el alert dialog
        console.warn("Equipo lleno");
      }
    }
    console.warn("TEAM");
  }
},

  //   handlePokemonUpdated() {
  //   // const { tipo, pokemonId } = pokemonData;
  //   // console.log(tipo);

  //   // Recorrer el array de componentes Pokemon y obtener los tipos de cada uno
  //   this.pokemonComponents.forEach((pokemonComponent) => {
  //     const tipoPokemon = pokemonComponent.pokemonTypes;
  //     // Hacer lo que necesites con los tipos del componente Pokemon
  //     pokemonComponent.pokemonTypes.forEach(element => {
        
  //      console.log(element);
  //     });
      
  //   });
  // },
  agregarPokemonPorTipo(tipo, pokemonId) {
  if (!this.pokemonsPorTipo.hasOwnProperty(tipo)) {
    // Si el tipo aún no existe en el objeto, crear un nuevo array
    this.pokemonsPorTipo[tipo] = [pokemonId];
  } else {
    // Si el tipo ya existe, comprobar si el ID del Pokémon ya está en el array
    if (!this.pokemonsPorTipo[tipo].includes(pokemonId)) {
      // Agregar el ID del Pokémon al array solo si no existe
      this.pokemonsPorTipo[tipo].push(pokemonId);
    }
  }
},


  },
  computed: {
    pokemonsFiltrados() {
      if(this.Poketeam)
      {
        return this.equipo;
      }
      else if(this.Pokefav)
      {
        return this.favoritos;
      }
      else{
        if (this.tipoSeleccionado === 'todos') {
      // Si se selecciona el tipo 'Todos', devolver todos los Pokémon
      let map = Array.from({ length: this.to }, (_, index) => ( index + 1 ));
      console.log(map);
      return map;
    } else {
      // Si se selecciona un tipo específico
      console.warn("Array");
      console.log(this.pokemonsPorTipo[this.tipoSeleccionado]);
      
        // console.warn("Array mod");
        // console.log(map);
        return this.pokemonsPorTipo[this.tipoSeleccionado];
      }
      }
    
  },

  },
};
</script>

<style lang="scss" scoped>
@import "index.scss";
.btn { 
  position:fixed;
  right: 50px;
  bottom: 50px;
  height: 80px;
  line-height: 80px;  
  width: 80px;  
  font-size: 2em;
  font-weight: bold;
  border-radius: 50%;
  background-color: $electric;
  color: white;
  text-align: center;
  cursor: pointer;

  }
  .x-5 {
  height: 12px;
 
  color: $dark-slate-gray;
  // display: flex;
  align-items: center;
  text-align: center;
  margin-bottom: 5px;
  margin-bottom: 10px;
  font-family: "Inner";

}
  .btn2 { 
  position:fixed;
  right: 50px;
  bottom: 200px;
  height: 80px;
  line-height: 80px;  
  width: 80px;  
  font-size: 2em;
  font-weight: bold;
  border-radius: 50%;
  background-color: $electric;
  color: white;
  text-align: center;
  cursor: pointer;
}
.titleImg
{
  margin-left: auto;
  margin-right: auto;
  display: block;
  margin-top: 40px;
}
h1, h2 , label
{
  color: $electric;
}
.pokedex {
  background-color: $dark-slate-gray;
  width: max-content;
}

.filter-section {
  margin-bottom: 16px;
}

.pokemon-grid {
  width: 100%;  
  display: grid;
  grid-template-columns: repeat(3, auto);
  grid-gap: 10px;
  margin-bottom: 10%;
}
:root {
  --select-border: #777;
  --select-focus: #00f;
  --select-arrow: var(--select-border);
}
select {
  appearance: none;
  background-color: transparent;
  border: none;
  padding: 0 1em 0 0;
  margin: 0;
  width: 100%;
  font-family: inherit;
  font-size: inherit;
  cursor: inherit;
  line-height: inherit;
  z-index: 1;
  outline: none;
}

.select {
  display: grid;
  grid-template-areas: "select";
  align-items: center;
  position: relative;
  min-width: 15ch;
  max-width: 30ch;
  border: 1px solid var(--select-border);
  border-radius: 0.25em;
  padding: 0.25em 0.5em;
  font-size: 1.25rem;
  cursor: pointer;
  line-height: 1.1;
  background-color: #fff;
  background-image: linear-gradient(to top, #f9f9f9, #fff 33%);
}
.select select,
.select::after {
  grid-area: select;
}
.select:not(.select--multiple)::after {
  content: "";
  justify-self: end;
  width: 0.8em;
  height: 0.5em;
  background-color: var(--select-arrow);
  clip-path: polygon(100% 0%, 0 0%, 50% 100%);
}
select:focus + .focus {
  position: absolute;
  top: -1px;
  left: -1px;
  right: -1px;
  bottom: -1px;
  border: 2px solid var(--select-focus);
  border-radius: inherit;
}
select[multiple] {
  padding-right: 0;
  /*
   * Safari will not reveal an option
   * unless the select height has room to 
   * show all of it
   * Firefox and Chrome allow showing 
   * a partial option
   */
  height: 6rem;
  /* 
   * Experimental - styling of selected options
   * in the multiselect
   * Not supported crossbrowser
   */
}
select[multiple] option {
  white-space: normal;
  outline-color: var(--select-focus);
}
.select--disabled {
  cursor: not-allowed;
  background-color: #eee;
  background-image: linear-gradient(to top, #ddd, #eee 33%);
}
label {
  font-size: 1.125rem;
  font-weight: 500;
}
.select + label {
  margin-top: 2rem;
}
</style>
