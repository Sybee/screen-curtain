<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
html {
  height: 100%;
  width: 100%;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  height: 100%;
  background: #1b1b1b;
  overflow: hidden;
}

* {
  user-select: none;
}

#app {
  height: 100%;
}

// .container {
  // display: flex;
  // justify-content: center;
  // overflow: hidden;
  // user-select: none;
  // touch-action: none;
// }
</style>
