<template>
  <div class="logs-table">
    <div
      class="log"
      v-for="(log, index) in logs"
      :key="index"
      :style="{ backgroundColor: index % 2 === 0 ? color1 : color2 }"
    >
      {{ log }}
    </div>
  </div>
</template>

<script>
  export default {
    name: "ActionsLog",
    props: {
      logs: Array,
    },
    data() {
      return {
        color1: "rgb(50, 200, 50)",
        color2: "rgb(200, 50, 50)",
      };
    },
  };
</script>

<style scoped>
  .logs-table {
    width: 100%;
    padding: 0 2rem;
    margin: 0.1rem 0;
    display: flex;
    flex-direction: column;
    text-align: center;
    justify-content: center;
    box-sizing: border-box;
  }

  .log {
    width: 100%;
    margin: 0.2rem 0;
    padding: 0.5rem;
    box-shadow: 3px 3px 0 rgb(10, 30, 30);
    box-sizing: border-box;
  }
</style>
