# vue-live-ellipsis

## Description

`vue-live-ellipsis` is a component for text truncation using an ellipsis that is controlled only by CSS. The component will automatically clamp text to fill whatever container it is placed in. Simply add styles to constrain the size of a containing element, and `vue-live-ellipsis` will calculate the ellipsis position and rerender the component whenever the container size is adjusted. This is very helpful in responsive web apps where content is frequently changing. While there are alternatives, most of them rely on setting a fixed number of lines that you want to clamp. but sometimes that isn't a fixed value and may be difficult to calculate. Instead, this component takes in no sizing props and instead relies upon the size of the component's parent container.

## Installation
```
yarn add vue-live-ellipsis
// or 
npm i --save vue-live-ellipsis
```

## Basic Usage

```html
<template>
  <div class="container">
    <Ellipsis :text="text" :position="position" />
  </div>
</template>
<script>
import Ellipsis from 'vue-live-ellipsis'

export default {
    name: "MyComponent",
    components: { Ellipsis },
    data() {
        return {
            position: 'middle',
            text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
        };
    }
}
</script>
<style scoped>
.container {
    max-width: 300px;
    max-height: 40px; /* approx height of 2 lines */
    font-size: 18px;
}
</style>
```

### Props

| Prop  | Type  | Default  | Description |
|---|---|---|---|
| `text`  | `String`  | `''`  | The text that will be truncated if there is too much text to fit inside the container |
| `position`  | `String`  | `'end'`  | Position for placing the ellipsis character. Must be one of: `'start'`, `'middle'` or `'end'` |

### Alternative Libraries

- [`Luobata/vue-text-ellipsis `](https://github.com/Luobata/vue-text-ellipsis)
- [`Justineo/vue-clamp `](https://github.com/Justineo/vue-clamp)
- [`jypblue/vue-ellipsis `](https://github.com/jypblue/vue-ellipsis)
- [`hyjiacan/vue-ellipsis`](https://github.com/hyjiacan/vue-ellipsis)
- [`Frondor/vue-line-clamp `](https://github.com/Frondor/vue-line-clamp)
