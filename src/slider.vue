<template>
  <div class="range_container">
    <div class="sliders_control">
      <input id="fromSlider" type="range" value="0" min="0" max="1008" @input="handleRangeChange" />      
      <input id="toSlider" type="range" value="151" min="0" max="1008" @input="handleRangeChange" />
      
    </div>
    
    
    <div class="form_control">
      <div class="form_control_container">
        <div style="color: rgba(247, 208, 44, 1);" class="form_control_container__time">Min</div>
        <input class="form_control_container__time__input" type="number" id="fromInput" value="0" min="0" max="1008"
          @input="handleInput" />
      </div>
      <div class="form_control_container">
        <div style="color: rgba(247, 208, 44, 1);" class="form_control_container__time">Max</div>
        <input class="form_control_container__time__input" type="number" id="toInput" value="151" min="0" max="1008"
          @input="handleInput" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RangeSlider",
  data() {
    return {
      timeoutId: null,
      fromSliderValue: 10,
      toSliderValue: 151,

    };
  },
  methods: {
    handleRangeChange() {
      // console.warn(this.timeoutId);
      this.toSliderValue = parseInt(document.getElementById("toInput").value);
      this.fromSliderValue = parseInt(document.getElementById("fromSlider").value);
      clearTimeout(this.timeoutId);
      this.timeoutId = setTimeout(() => {
        this.updateParent();
      }, 2000);
    },
    handleInput() {
this.toSliderValue = parseInt(document.getElementById("toSlider").value);
      this.fromSliderValue = parseInt(document.getElementById("fromInput").value);
      clearTimeout(this.timeoutId);
      this.timeoutId = setTimeout(() => {
        this.updateParent();
      }, 2000);
    },
    updateParent() {

      this.$emit("range-updated", {
        from: this.fromSliderValue,
        to: this.toSliderValue,
      });
    },
  },
  mounted() {

    this.updateParent();
    function controlFromInput(fromSlider, fromInput, toInput, controlSlider) {
      const [from, to] = getParsed(fromInput, toInput);
      fillSlider(fromInput, toInput, '#C6C6C6', '#25daa5', controlSlider);
      if (from > to) {
        fromSlider.value = to;
        fromInput.value = to;
      } else {
        fromSlider.value = from;
      }
    }

    function controlToInput(toSlider, fromInput, toInput, controlSlider) {
      const [from, to] = getParsed(fromInput, toInput);
      fillSlider(fromInput, toInput, '#C6C6C6', '#25daa5', controlSlider);
      setToggleAccessible(toInput);
      if (from <= to) {
        toSlider.value = to;
        toInput.value = to;
      } else {
        toInput.value = from;
      }
    }

    function controlFromSlider(fromSlider, toSlider, fromInput) {
      const [from, to] = getParsed(fromSlider, toSlider);
      fillSlider(fromSlider, toSlider, '#C6C6C6', '#25daa5', toSlider);
      if (from > to) {
        fromSlider.value = to;
        fromInput.value = to;
      } else {
        fromInput.value = from;
      }
    }

    function controlToSlider(fromSlider, toSlider, toInput) {
      const [from, to] = getParsed(fromSlider, toSlider);
      fillSlider(fromSlider, toSlider, '#C6C6C6', '#25daa5', toSlider);
      setToggleAccessible(toSlider);
      if (from <= to) {
        toSlider.value = to;
        toInput.value = to;
      } else {
        toInput.value = from;
        toSlider.value = from;
      }
    }

    function getParsed(currentFrom, currentTo) {
      const from = parseInt(currentFrom.value, 10);
      const to = parseInt(currentTo.value, 10);
      return [from, to];
    }

    function fillSlider(from, to, sliderColor, rangeColor, controlSlider) {
      const rangeDistance = to.max - to.min;
      const fromPosition = from.value - to.min;
      const toPosition = to.value - to.min;
      controlSlider.style.background = `linear-gradient(
        to right,
        ${sliderColor} 0%,
        ${sliderColor} ${(fromPosition / rangeDistance) * 100}%,
        ${rangeColor} ${((fromPosition / rangeDistance) * 100)}%,
        ${rangeColor} ${(toPosition / rangeDistance) * 100}%, 
        ${sliderColor} ${(toPosition / rangeDistance) * 100}%, 
        ${sliderColor} 100%)`;
    }

    function setToggleAccessible(currentTarget) {
      const toSlider = document.querySelector('#toSlider');
      if (Number(currentTarget.value) <= 0) {
        toSlider.style.zIndex = 2;
      } else {
        toSlider.style.zIndex = 0;
      }
    }

    const fromSlider = document.querySelector('#fromSlider');
    const toSlider = document.querySelector('#toSlider');
    const fromInput = document.querySelector('#fromInput');
    const toInput = document.querySelector('#toInput');
    fillSlider(fromSlider, toSlider, '#C6C6C6', '#25daa5', toSlider);
    setToggleAccessible(toSlider);

    fromSlider.oninput = () => controlFromSlider(fromSlider, toSlider, fromInput);
    toSlider.oninput = () => controlToSlider(fromSlider, toSlider, toInput);
    fromInput.oninput = () => controlFromInput(fromSlider, fromInput, toInput, toSlider);
    toInput.oninput = () => controlToInput(toSlider, fromInput, toInput, toSlider);
  }
}
</script>


<style scoped>
.range_container {
  display: flex;
  flex-direction: column;
  width: 80%;
  margin: 15% auto;
}

.sliders_control {
  position: relative;
  min-height: 50px;
}

.form_control {
  position: relative;
  display: flex;
  justify-content: space-between;
  font-size: 24px;
  color: #635a5a;
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  pointer-events: all;
  width: 24px;
  height: 24px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 0 0 1px #C6C6C6;
  cursor: pointer;
}

input[type=range]::-moz-range-thumb {
  -webkit-appearance: none;
  pointer-events: all;
  width: 24px;
  height: 24px;
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 0 0 1px #C6C6C6;
  cursor: pointer;
}

input[type=range]::-webkit-slider-thumb:hover {
  background: #f7f7f7;
}

input[type=range]::-webkit-slider-thumb:active {
  box-shadow: inset 0 0 3px #387bbe, 0 0 9px #387bbe;
  -webkit-box-shadow: inset 0 0 3px #387bbe, 0 0 9px #387bbe;
}

input[type="number"] {
  color: #8a8383;
  width: 50px;
  height: 30px;
  font-size: 20px;
  border: none;
}

input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
  opacity: 1;
}

input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  height: 2px;
  width: 100%;
  position: absolute;
  background-color: #C6C6C6;
  pointer-events: none;
}

#fromSlider {
  height: 0;
  z-index: 1;
}
datalist {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  writing-mode: vertical-lr;
 
}



</style>
