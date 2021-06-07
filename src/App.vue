<template>
  <div id="app">
    <UlList v-bind="{inventory}"/>
  </div>
</template>

<script>
import UlList from "./components/UlList";
//import UlList2 from "./components/UlList2";

export default {
  name: 'App',
  data: () => ({
    inventory: {
      setting: {
        widthColumn: 70,
        heightColumn: 70,
        rows: 7,
        columns: 5,
      },
      items: [
        {key: 2, title: "Яблоко"},
        {key: 8, title: "Банан"},
        {key: 25, title: "Груша"},
        {key: 26, title: "Вишня"},
        {key: 34, title: "Ананас"},
      ]
    }
  }),
  components: {
    UlList,
    //UlList2,
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
</style>
