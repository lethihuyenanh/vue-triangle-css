<template>
  <h1>CSS Triangle <br class="u-sm-max">Generator by Vue.js</h1>
  <div class="box">
    <div class="box-triggle">
      <div class="box-triggle__inner">
        <div class="box-triggle__btns">
          <button @click="onSelectedType('top')" :class="typeIsSelected === 'top' ? 'is-selected' : ''"
            :style="preStyleList.top" class="top">top</button>
          <button @click="onSelectedType('right')" :class="typeIsSelected === 'right' ? 'is-selected' : ''"
            :style="preStyleList.right" class="right">right</button>
          <button @click="onSelectedType('bottom')" :class="typeIsSelected === 'bottom' ? 'is-selected' : ''"
            :style="preStyleList.bottom" class="bottom">bottom</button>
          <button @click="onSelectedType('left')" :class="typeIsSelected === 'left' ? 'is-selected' : ''"
            :style="preStyleList.left" class="left">left</button>
          <button @click="onSelectedType('top-left')" :class="typeIsSelected === 'top-left' ? 'is-selected' : ''"
            :style="preStyleList.topLeft" class="topLeft">top-left</button>
          <button @click="onSelectedType('top-right')" :class="typeIsSelected === 'top-right' ? 'is-selected' : ''"
            :style="preStyleList.topRight" class="topRight">top-right</button>
          <button @click="onSelectedType('bottom-left')" :class="typeIsSelected === 'bottom-left' ? 'is-selected' : ''"
            :style="preStyleList.bottomLeft" class="bottomLeft">bottom-left</button>
          <button @click="onSelectedType('bottom-right')" :class="typeIsSelected === 'bottom-right' ? 'is-selected' : ''"
            :style="preStyleList.bottomRight" class="bottomRight">bottom-right</button>
        </div>
        <div class="form-control" :class="{ invalid: !width.isValid }">
          <label class="form-control__head" for="width">Width:</label>
          <div class="form-control__body">
            <input type="number" id="width" v-model.number="width.val" @blur="validateValue(width)">
            <p class="text-error" v-if="!width.isValid">Width must be greater than 2</p>
          </div>
        </div>
        <div class="form-control" :class="{ invalid: !height.isValid }">
          <label class="form-control__head" for="height">Height:</label>
          <div class="form-control__body">
            <input type="number" id="height" v-model.number="height.val" @blur="validateValue(height)">
            <p class="text-error" v-if="!height.isValid">Height must be greater than 2</p>
          </div>
        </div>
        <div class="color-pickers">
          <ColorPicker v-model:pureColor="color" is-widget pickerType="chrome" :debounce="10" :disableHistory="true"
            :disableAlpha="true" @pureColorChange="onChangeColor" />
        </div>
      </div>
    </div>
    <div class="box-result">
      <div class="box-result__inner" v-if="!!typeIsSelected && formIsValid">
        <div class="box-result__css">
          <div v-for="style in resultStyles" :key="style.index">{{ style }};</div>
        </div>
        <div class="box-result__shape">
          <div :style="resultCSS"></div>
        </div>
      </div>
    </div>
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
        val: 100,
        isValid: true,
      },
      height: {
        val: 100,
        isValid: true,
      },
      color: 'rgb(0,0,0)',
      typeIsSelected: '',
      formIsValid: true,
      preStyleList: [],
    }
  },
  computed: {
    resultStyles() {
      let styles = [];

      //setup value for triangle
      const width = this.width.val;
      const height = this.height.val;
      const halfWidth = width / 2;
      const halfHeight = height / 2;
      const color = this.color;

      //Count value and generate source
      switch (this.typeIsSelected) {
        case 'top':
          styles = [...this.styleDefault].concat([
            `border-width: 0 ${halfWidth}px ${height}px ${halfWidth}px`,
            `border-color: transparent transparent ${color} transparent`,
          ]);
          break;
        case 'right':
          styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px 0 ${halfHeight}px ${width}px`,
            `border-color: transparent transparent transparent ${color}`,
          ]);
          break;
        case 'bottom':
          styles = [...this.styleDefault].concat([
            `border-width: ${height}px ${halfWidth}px 0 ${halfWidth}px`,
            `border-color: ${color} transparent transparent transparent`,
          ]);
          break;
        case 'left':
          styles = [...this.styleDefault].concat([
            `border-width: ${halfHeight}px ${width}px ${halfHeight}px 0`,
            `border-color: transparent ${color} transparent transparent`,
          ]);
          break;
        case 'top-left':
          styles = [...this.styleDefault].concat([
            `border-width: ${height}px ${width}px 0 0`,
            `border-color: ${color} transparent transparent transparent`,
          ]);
          break;
        case 'top-right':
          styles = [...this.styleDefault].concat([
            `border-width: 0 ${width}px ${height}px 0`,
            `border-color: transparent ${color} transparent transparent`,
          ]);
          break;
        case 'bottom-left':
          styles = [...this.styleDefault].concat([
            `border-width: ${height}px 0 0 ${width}px`,
            `border-color: transparent transparent transparent ${color}`,
          ]);
          break;
        case 'bottom-right':
          styles = [...this.styleDefault].concat([
            `border-width: 0 0 ${height}px ${width}px`,
            `border-color: transparent transparent ${color} transparent`,
          ]);
          break;
        default:
          break;
      }
      return styles;
    },
    resultCSS() {
      // generate to style CSS
      let result = '';
      for (let item of this.resultStyles) {
        result += item + ';';
      }
      return result;
    },
  },
  created() {
    this.onPreStyleList();
  },
  beforeMount() {
    this.width.val = 100;
    this.height.val = 100;
    this.typeIsSelected = null;
    this.color = 'rgb(0,0,0)';
  },
  methods: {
    onChangeColor(event) {
      this.color = event;
    },
    // clearValidity(input) {
    //   input.isValid = true;
    // },
    validateValue(input) {
      input.isValid = true;
      this.formIsValid = true;
      if (!this.width.val || this.width.val < 2) {
        this.width.isValid = false;
        this.formIsValid = false;
      }
      if (!this.height.val || this.height.val < 2) {
        this.height.isValid = false;
        this.formIsValid = false;
      }
    },
    onSelectedType(type) {
      this.typeIsSelected = type;
    },
    onPreStyleList() {
      this.width.val = 30;
      this.height.val = 30;

      this.color = 'red';
      this.typeIsSelected = 'top';
      this.preStyleList.top = this.resultCSS;

      this.color = '#ffdd00';
      this.typeIsSelected = 'right';
      this.preStyleList.right = this.resultCSS;

      this.color = '#0095ff';
      this.typeIsSelected = 'bottom';
      this.preStyleList.bottom = this.resultCSS;

      this.color = '#bb00ff';
      this.typeIsSelected = 'left';
      this.preStyleList.left = this.resultCSS;

      this.color = '#ff00bb';
      this.typeIsSelected = 'top-left';
      this.preStyleList.topLeft = this.resultCSS;

      this.color = '#ff7700';
      this.typeIsSelected = 'top-right';
      this.preStyleList.topRight = this.resultCSS;

      this.color = '#6600ff';
      this.typeIsSelected = 'bottom-left';
      this.preStyleList.bottomLeft = this.resultCSS;

      this.color = '#00ff66';
      this.typeIsSelected = 'bottom-right';
      this.preStyleList.bottomRight = this.resultCSS;

      // console.log(this.preStyleList);
    }
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

h1 {
  text-align: center;
  margin: 1.5rem 0;
  font-size: 2rem;
}

@media screen and (min-width: 768px) {
  h1 {
    margin: 2rem 0;
    font-size: 2rem;
  }
}

.box {
  max-width: 1440px;
  width: 100%;
  padding-left: 20px;
  padding-right: 20px;
  margin-left: auto;
  margin-right: auto;
}

.box-triggle {
  display: flex;
  justify-content: center;
  border-bottom: 1px solid #ccc;
  padding-bottom: 20px;
  margin-bottom: 20px;
}

.box-triggle__inner {
  width: 320px;
  max-width: 100%;
  background-color: beige;
  padding: 50px 20px 20px;
}

.box-triggle__btns {
  position: relative;
  width: 90px;
  height: 90px;
  background: pink;
  margin: 0 auto 50px;
}

.box-triggle__btns button {
  background-color: transparent;
  cursor: pointer;
  outline: none;
  font-size: 0;
  line-height: 0;
  text-indent: 99999em;
  transition: transform 0.3s ease-out;
  position: absolute;
}

.box-triggle__btns button:hover {
  transform: scale(1.5);
  z-index: 2;
}

.box-triggle__btns button.is-selected {
  transform: scale(1.5);
  z-index: 2;
  opacity: 1;
}

.box-triggle__btns .top {
  bottom: 100%;
  left: 50%;
  margin-left: -15px;
  /* transform-origin: 50% 100%; */
}

.box-triggle__btns .right {
  left: 100%;
  top: 50%;
  margin-top: -15px;
  /* transform-origin: 0 50%; */
}

.box-triggle__btns .bottom {
  top: 100%;
  left: 50%;
  margin-left: -15px;
  /* transform-origin: 50% 0; */
}

.box-triggle__btns .left {
  right: 100%;
  top: 50%;
  margin-top: -15px;
  /* transform-origin: 100% 50%; */
}

.box-triggle__btns .topLeft {
  top: 0;
  left: 0;
  /* transform-origin: 0 0; */
}

.box-triggle__btns .topRight {
  top: 0;
  right: 0;
  /* transform-origin: 100% 0; */
}

.box-triggle__btns .bottomLeft {
  bottom: 0;
  left: 0;
  /* transform-origin: 0 100%; */
}

.box-triggle__btns .bottomRight {
  bottom: 0;
  right: 0;
  /* transform-origin: 100% 100%; */
}

.form-control {
  display: flex;
  align-items: center;
}

.form-control+.form-control {
  margin-top: 0.5rem;
}

.form-control .form-control__head {
  width: 4rem;
}

.form-control .form-control__body {
  width: calc(100% - 4rem);
}

.form-control input {
  display: block;
  height: 38px;
  border-radius: 4px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  border: 1px solid #ccc;
  padding: 0 0.5rem;
  font-size: 1rem;
  font-family: inherit;
  color: inherit;
}

.color-pickers {
  margin-top: 1rem;
}

.box-result {
  padding-bottom: 20px;
}

.box-result__css {
  text-align: left;
  font-family: monospace, sans-serif;
  background-color: beige;
  padding: 20px;
  font-size: 90%;
  margin-bottom: 20px;
}

.box-result__shape {
  padding: 20px;
  background: url(./assets/images/bg01.jpg);
}

.form-control.invalid input {
  border: 1px solid red;
  color: red;
}

.text-error {
  color: red;
  padding: 0.25em 0;
}

@media screen and (min-width: 768px) {
  .u-sm-max {
    display: none;
  }

  .box {
    display: flex;
    padding-bottom: 20px;
  }

  .box-triggle {
    border-right: 1px solid #ccc;
    border-bottom: none;
    padding-bottom: 0;
    margin-bottom: 0;
    padding-right: 20px;
  }

  .box-result {
    width: calc(100% - 320px);
    padding-left: 20px;
    padding-bottom: 0;
  }

  .box-result__inner {
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  .box-result__shape {
    flex-grow: 1;
  }
}</style>
