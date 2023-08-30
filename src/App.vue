<template>
  <h1>CSS Triangle Generator by Vue.js</h1>
  <div class="box-triggle">
    <button @click="onSelectedType('top')" :class="typeIsSelected === 'top' ? 'is-selected' : ''">top</button>
    <button @click="onSelectedType('right')" :class="typeIsSelected === 'right' ? 'is-selected' : ''">right</button>
    <button @click="onSelectedType('bottom')" :class="typeIsSelected === 'bottom' ? 'is-selected' : ''">bottom</button>
    <button @click="onSelectedType('left')" :class="typeIsSelected === 'left' ? 'is-selected' : ''">left</button>
    <br>
    <button @click="onSelectedType('top-left')" :class="typeIsSelected === 'top-left' ? 'is-selected' : ''">top-left</button>
    <button @click="onSelectedType('top-right')" :class="typeIsSelected === 'top-right' ? 'is-selected' : ''">top-right</button>
    <button @click="onSelectedType('bottom-left')" :class="typeIsSelected === 'bottom-left' ? 'is-selected' : ''">bottom-left</button>
    <button @click="onSelectedType('bottom-right')" :class="typeIsSelected === 'bottom-right' ? 'is-selected' : ''">bottom-right</button>
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
      width: 0,
      height: 0,
      styles: null,
      styleCSS: '',
      typeIsSelected: '',
    }
  },
  methods: {
    onSelectedType(type) {
      this.width = 30;
      this.height = 30;
      const halfWidth = this.width / 2;
      const halfHeight = this.height / 2;
      
      this.typeIsSelected = type;
      //Count value and generate source
      switch (type) {
        case 'top':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 ${halfWidth}px ${this.height}px ${halfWidth}px`,
            `border-color: transparent transparent #007bff transparent`,
          ]);
          break;
        case 'right':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px 0 ${halfHeight}px ${this.width}px`,
            `border-color: transparent transparent transparent #007bff`,
          ]);
          break;
        case 'bottom':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${this.height}px ${halfWidth}px 0 ${halfWidth}px`,
            `border-color: #007bff transparent transparent transparent`,
          ]);
          break;
        case 'left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px ${this.width}px ${halfHeight}px 0`,
            `border-color: transparent #007bff transparent transparent`,
          ]);
          break;
        case 'top-left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${this.height}px ${this.width}px 0 0`,
            `border-color: #007bff transparent transparent transparent`,
          ]);
          break;
        case 'top-right':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 ${this.width}px ${this.height}px 0`,
            `border-color: transparent #007bff transparent transparent`,
          ]);
          break;
        case 'bottom-left':
          this.styles = [...this.styleDefault].concat([
            `border-width: ${this.height}px 0 0 ${this.width}px`,
            `border-color: transparent transparent transparent #007bff`,
          ]);
          break;
        case 'bottom-right':
          this.styles = [...this.styleDefault].concat([
            `border-width: 0 0 ${this.height}px ${this.width}px`,
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
  color: red;
}

.box-generated-css {
  text-align: left;
  font-family: monospace, sans-serif;
}
</style>
