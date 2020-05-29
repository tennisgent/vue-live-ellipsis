<template>
  <div id="app">
    <img width="10%" src="./assets/logo.png">
    <p>The text will automatically rerender whenever the container resizes. Use standard css rules to control the size of the container and the content will be automatically truncated to fit within it.</p>
    <p>Use the controls below to adjust the rendered output:</p>
    <div class="controls">
      <table>
        <tbody>
          <tr>
            <td>Max Width:</td>
            <td>
              <input type="range" min="20" max="500" step="10" v-model="width" class="stretch">
            </td>
          </tr>
          <tr>
            <td>Max Height:</td>
            <td>
              <input
                v-if="multiLine"
                type="range"
                min="20"
                max="300"
                step="10"
                v-model="height"
                class="stretch"
              >
              <code v-if="!multiLine">"auto"</code>
            </td>
          </tr>
          <tr>
            <td>Font Size:</td>
            <td>
              <input type="range" min="12" max="28" step="1" v-model="fontSize" class="stretch">
            </td>
          </tr>
          <tr>
            <td>Position:</td>
            <td>
              <div class="flex spacing">
                <div>
                  <input type="radio" id="pos1" v-model="position" value="start">
                  <label for="pos1">Start</label>
                </div>
                <div>
                  <input type="radio" id="pos2" v-model="position" value="middle">
                  <label for="pos2">Middle</label>
                </div>
                <div>
                  <input type="radio" id="pos3" v-model="position" value="end">
                  <label for="pos3">End</label>
                </div>
              </div>
            </td>
          </tr>
          <tr>
            <td>Text:</td>
            <td>
              <textarea rows="10" v-model="text" class="stretch"></textarea>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <h3>
        <code>
          &lt;div :style="{{ styles }}"&gt;
          <br>
          &nbsp;&nbsp;&nbsp;
          &lt;Ellipsis :text="&lt;text_here&gt;" :position="{{ position }}" /&gt;
          <br>&lt;/div&gt;
        </code>
      </h3>
      <div class="boundary" :style="{ width: width + 'px', height: height + 'px' }">
        <div class="container" :style="styles">
          <Ellipsis :text="text" :position="position" :multi-line="multiLine"/>
        </div>
      </div>
    </div>

    <p>NOTE: Any css changes that don't cause the container to change size will not trigger a rerender. For example, lowering the font-size will not cause a rerender if the height is set to something greater.</p>
  </div>
</template>

<script>
import Ellipsis from "./components";

export default {
  name: "App",
  components: { Ellipsis },
  data() {
    return {
      width: 450,
      height: 180,
      multiLine: true,
      position: "end",
      fontSize: 18,
      text: `Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.`
    };
  },
  computed: {
    styles: function() {
      return {
        maxWidth: `${this.width}px`,
        maxHeight: this.multiLine ? `${this.height}px` : "auto",
        fontSize: `${this.fontSize}px`
      };
    }
  }
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 40px;
  display: flex;
  flex-direction: column;
}
.container {
  overflow: hidden;
  border: 1px solid black;
}
textarea {
  margin: 16px;
  width: 500px;
}
.flex {
  display: flex;
  align-items: center;
}
.spacing {
  width: 400px;
  justify-content: space-between;
}
.stretch {
  width: 100%;
}
.boundary {
  font-size: 0;
  border: 1px dashed cornflowerblue;
}
</style>
