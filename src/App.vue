<template>
  <h1>CSS Triangle Generator by Vue.js</h1>
  <div class="box-triggle">
    <button @click="onSelectedType('top')" :class="typeIsSelected === 'top' ? 'is-selected' : ''">top</button>
    <button @click="onSelectedType('right')" :class="typeIsSelected === 'right' ? 'is-selected' : ''">right</button>
    <button @click="onSelectedType('bottom')" :class="typeIsSelected === 'bottom' ? 'is-selected' : ''">bottom</button>
    <button @click="onSelectedType('left')" :class="typeIsSelected === 'left' ? 'is-selected' : ''">left</button>
    <br>
    <button @click="onSelectedType('top-left')"
      :class="typeIsSelected === 'top-left' ? 'is-selected' : ''">top-left</button>
    <button @click="onSelectedType('top-right')"
      :class="typeIsSelected === 'top-right' ? 'is-selected' : ''">top-right</button>
    <button @click="onSelectedType('bottom-left')"
      :class="typeIsSelected === 'bottom-left' ? 'is-selected' : ''">bottom-left</button>
    <button @click="onSelectedType('bottom-right')"
      :class="typeIsSelected === 'bottom-right' ? 'is-selected' : ''">bottom-right</button>
    <div class="form-control" :class="{ invalid: !width.isValid }">
      <label for="width">Width:</label>
      <input type="number" id="width"
        v-model.number="width.val" @blur="clearValidity(width)">
      <p class="text-error" v-if="!width.isValid">Width must be greater than 0.</p>
    </div>
    <div class="form-control" :class="{ invalid: !height.isValid }">
      <label for="height">Height:</label>
      <input type="number" id="height"
        v-model.number="height.val" @blur="clearValidity(height)">
      <p class="text-error" v-if="!height.isValid">Height must be greater than 0.</p>
    </div>
  </div>
  <hr>
  <div class="box-result" :style="styleCSS"></div>
  <br>
  <div class="box-generated-css">
    <div v-for="style in styles" :key="style.index">{{ style }};</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      styleDefault: [
        'width: 0',
        'height: 0',
        'border-style: solid',
      ],
      width: {
        val: '',
        isValid: true,
      },
      height: {
        val: '',
        isValid: true,
      },
      styles: null,
      styleCSS: '',
      typeIsSelected: '',
      formIsValid: true,
    }
  },
  methods: {
    clearValidity(input) {
      input.isValid = true;
    },
    onSelectedType(type) {
      //validate input values
      this.formIsValid = true;
      if ( !this.width.val || this.width.val < 0 ) {
        this.width.isValid = false;
        this.formIsValid = false;
      }
      if (!this.height.val || this.height.val < 0) {
        this.height.isValid = false;
        this.formIsValid = false;
      }

      //setup value for triangle
      const width = this.width.val;
      const height = this.height.val;
      const halfWidth = width / 2;
      const halfHeight = height / 2;

      this.typeIsSelected = type;

      //Count value and generate source
      switch (type) {
        case 'top':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 ${halfWidth}px ${height}px ${halfWidth}px`,
            `border-color: transparent transparent #007bff transparent`,
          ]);
          break;
        case 'right':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px 0 ${halfHeight}px ${width}px`,
            `border-color: transparent transparent transparent #007bff`,
          ]);
          break;
        case 'bottom':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${height}px ${halfWidth}px 0 ${halfWidth}px`,
            `border-color: #007bff transparent transparent transparent`,
          ]);
          break;
        case 'left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px ${width}px ${halfHeight}px 0`,
            `border-color: transparent #007bff transparent transparent`,
          ]);
          break;
        case 'top-left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${height}px ${width}px 0 0`,
            `border-color: #007bff transparent transparent transparent`,
          ]);
          break;
        case 'top-right':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 ${width}px ${height}px 0`,
            `border-color: transparent #007bff transparent transparent`,
          ]);
          break;
        case 'bottom-left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${height}px 0 0 ${width}px`,
            `border-color: transparent transparent transparent #007bff`,
          ]);
          break;
        case 'bottom-right':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 0 ${height}px ${width}px`,
            `border-color: transparent transparent #007bff transparent`,
          ]);
          break;
        default:
          break;
      }

      //generate to style CSS
      for (let item of this.styles) {
        this.styleCSS += item + ';';
      }
      // console.log(this.styleCSS);

    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button.is-selected {
  color: blueviolet;
}

.box-generated-css {
  text-align: left;
  font-family: monospace, sans-serif;
}

.form-control.invalid input {
  border: 1px solid red;
  color: red;
}

.text-error {
  color: red;
}

</style>
