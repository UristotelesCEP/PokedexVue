<template>
    <div class="frame-32" :style="{ backgroundColor: backgroundColor, boxShadow: boxShadowColor }">
      <p class="electric">{{ tipo }}</p>
    </div>
  </template>
  
  <script>
  import {
    normal,
    fire,
    water,
    electric,
    grass,
    ice,
    fighting,
    poison,
    ground,
    flying,
    psychic,
    bug,
    rock,
    ghost,
    dragon,
    dark,
    steel,
    fairy
  } from "./colores";
  
  export default {
    name: "Tipo",
    props: {
      tipo: {
        type: String,
        required: true,
        default: "fighting"
      },
    },
    data() {
      return {
        backgroundColor: electric,
        boxShadowColor: "",
      };
    },
    mounted() {
      this.cambiarColorFondo(this.tipo);
    },
    watch: {
      tipo: function (newTipo) {
        this.cambiarColorFondo(newTipo);
      },
    },
    methods: {
      cambiarColorFondo(color) {
        this.backgroundColor = eval(color) || electric;
        this.boxShadowColor = `0 0 12px 7px ${this.backgroundColor.replace("1)", "0.5)")}`;
      },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  @import "index.scss";
  .frame-32 {
    overflow: hidden;
    border-radius: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 110px;
  }
  .electric {
    text-transform: capitalize;
    
    font-family: "Dhurjati";
    font-size: 20px;
    font-weight: 400;
    line-height: normal;
    color: $black;
    
    align-items: center;
    text-align: center;
  }
  </style>
  