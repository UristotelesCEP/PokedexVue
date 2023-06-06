<template>
  <div :style="cardStyle" class="frame-18">
    <div class="frame-11">
      <p class="n-134">Nº {{ pokemonNumber }}</p>
    </div>
    <div class="frame-10">
      <p class="n-134">{{ pokemonName }}</p>
    </div>
    <img alt="" class="image-1" :src="pokemonImage" />

    <div class="tipos">
      <Tipo v-for="tipo in pokemonTypes" :key="tipo" :tipo="tipo" />
    </div>

    <div class="frame-53">
      <button class="frame-5" @click="notifyParent2(1)">
        <img alt="" class="vector"
          src="https://static.overlay-tech.com/assets/7082ecf7-2d2a-446f-bf53-4de72085fe84.svg" />
      </button>
      <button class="frame-6" @click="notifyParent2(2)" >        
        <div class="icon-1">
          <img alt="" class="vector-two"
            src="https://static.overlay-tech.com/assets/ed2ec964-cae9-47a4-aec6-36340a1f068c.svg" />
        </div>
      </button>
    </div>
  </div>
</template>
  
<script>
import Tipo from "./baddage.vue";
import * as colores from "./colores";
import axios from "axios";
export default {
  name: "Pokemon",

  data() {
    return {
      pokemonName: "",
      pokemonTypes: ["water"],
      pokemonImage: "",
    };
  },
  components: {
    Tipo,
  },

  props: {
    Ntotal: { type: String, default: "5" },
    NActual: { type: String, default: "0" },
    pokemonNumber: { type: Number, default: 37 }
  },
  watch: {
    
  },
  methods: {
    notifyParent() {      
      let cont = this;
     
      this.pokemonTypes.forEach((type) => {
        // console.log(cont);
        cont.$emit('ISLoad', {
          tipo: type,
          pokemonId: this.pokemonNumber
        });

      });

    },
    notifyParent2(num) {          
      this.$emit('BtnTrigger', {   
          btn: num,       
          pokemonId: this.pokemonNumber
        });
      

    },

    async fetchPokemonData() {
      const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemonNumber}`);
      const pokemonData = response.data;
      this.pokemonName = pokemonData.name;
      this.pokemonTypes = pokemonData.types.map((type) => type.type.name);
      this.pokemonImage = pokemonData.sprites.other['official-artwork'].front_default;
      this.notifyParent();
    },
  },
  created() {
    this.fetchPokemonData();
  },
  computed: {
    cardStyle() {
      const numTipos = this.pokemonTypes.length;
      ;
      if (numTipos === 1) {
        const tipo1Color = colores[`${this.pokemonTypes[0]}`];

        return {
          background: `linear-gradient(180deg, ${tipo1Color} 0%, ${colores[`darkslategray`]} 24%, ${colores[`transparentdarkslategray`]} 47%, ${colores[`darkslategray`]} 72%, ${tipo1Color} 100%)`,
        };
      } else if (numTipos >= 2) {

        const tipo1Color = colores[`${this.pokemonTypes[0]}`];
        const tipo2Color = colores[`${this.pokemonTypes[1]}`];
        // console.log(`linear-gradient(180deg, ${tipo1Color} 0%, ${tipo2Color} 24%, ${colores[`transparentdarkslategray`]} 47%, ${tipo2Color} 72%, ${tipo1Color} 100%)`)
        return {
          background: `linear-gradient(180deg, ${tipo1Color} 0%, ${tipo2Color} 24%, ${colores[`transparentdarkslategray`]} 47%, ${tipo2Color} 72%, ${tipo1Color} 100%)`,

        };
      }

      // Si no se especifica ningún tipo, se puede agregar un estilo de fondo predeterminado aquí
      return {
        background: "linear-gradient(180deg, $water 0%, $dark-slate-gray 24%, $transparent-dark-slate-gray 47%, $dark-slate-gray 72%, $water 100%)",
      };
    },
  },
};
</script>
  
<style lang="scss" scoped>
/* Estilos del componente */
</style>
  
  
<style lang="scss" scoped>
@import "index.scss";

.frame-18 {
max-width: 419px;
max-height: 684px;
  overflow: hidden;
  border-radius: 20px;
  padding: 15px 30px;
  // box-shadow: 0 0 122px 4px $yellow;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid $black;
}

// .frame-18:hover
// {
//   width: 400px;
// }
.tipos {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
  /* Espacio entre los elementos */
  margin-bottom: 20px;
}

.frame-11 {
  background-color: $dark-slate-gray;
  margin-bottom: 24px;
  overflow: hidden;
  border-radius: 20px;
  padding: 0 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 31px;
  width: 100px;
}

.n-134 {
  color: $white-smoke;
  text-align: center;
  font-family: "Inner";
}

.frame-10 {
  text-transform: capitalize;
  background-color: $dark-slate-gray;
  margin-bottom: 24px;
  overflow: hidden;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 31px;
  width: 193px;
}

.image-1 {
  width: 356px;
  height: 356px;
  margin-bottom: 24px;
}

.frame-14 {
  background-color: $water;
  margin-bottom: 24px;
  overflow: hidden;
  border-radius: 20px;
  padding: 17px 111px;
  box-shadow: 0 0 12px 7px $water;
  height: -7px;
  width: -105px;
  position: relative;
  font-family: "Dhurjati";
  font-size: 20px;
  font-weight: 400;
  line-height: normal;
  color: $black;
}

.water {
  max-width: 42px;
  font-family: "Dhurjati";
  font-size: 20px;
  font-weight: 400;
  line-height: normal;
  color: $black;
  text-align: center;
  position: absolute;
  left: 37.5px;
  top: -5px;
}

.frame-53 {
  padding: 13px 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.frame-5 {
  background-color: $dark-slate-gray;
  margin-right: 29px;
  overflow: hidden;
  border-radius: 20px;
  padding: 0 23px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 110px;
  height: 62px;
}

.vector {
  width: 40.83px;
  height: 38.79px;
}

.frame-6 {
  background-color: $dark-slate-gray;
  overflow: hidden;
  border-radius: 20px;
  padding: 0 35px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 110px;
  height: 62px;
}



.icon-1 {
  padding: 0.25px 0;
  display: flex;
  align-items: center;
}

.vector-two {
  flex: 1;
  align-self: stretch;
  object-fit: cover;
}</style>
  