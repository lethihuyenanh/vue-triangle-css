# vue-triangle-css

## Demo page
[https://lethihuyenanh.github.io/vue-triangle-css/](https://lethihuyenanh.github.io/vue-triangle-css/)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Picker color
```
npm install vue3-colorpicker
```
See 
[https://www.npmjs.com/package/vue3-colorpicker](https://www.npmjs.com/package/vue3-colorpicker)

[https://github.com/aesoper101/vue3-colorpicker](https://github.com/aesoper101/vue3-colorpicker).

### Guide to deploy Github
```
https://learnvue.co/articles/deploy-vue-to-github-pages

Step 1:
vue.config.js
module.exports = {
  publicPath:
    process.env.NODE_ENV === "production" ? "/vue-triangle-css/" : "/",
});

Step 2:
npm run build

Step 3. Run git add dist && git commit -m 'adding dist subtree'
git add dist
git commit -m 'adding dist subtree'

Note: Make sure that dist is not included in your .gitignorefile!

Step 4. 
git subtree push --prefix dist origin gh-pages
```