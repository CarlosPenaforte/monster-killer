<template>
  <div id="app">
    <GameTitle />
    <GameScreen />
  </div>
</template>

<script>
  import GameTitle from "./components/GameTitle.vue";
  import GameScreen from "./components/GamesScreen.vue";

  export default {
    name: "App",
    components: {
      GameTitle,
      GameScreen,
    },
  };
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700;1,900&display=swap");

  #app {
    font-family: "Source Sans Pro", sans-serif;
    padding: 0;
    margin: 0;
    width: 100vw;
    min-height: 100vh;
    background-color: rgb(20, 60, 60);
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
