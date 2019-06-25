<template>
  <div id="app" class="container">
     <h1 class="is-size-3">Colorado Springs MTB Trail Conditions</h1>
     <Trail>
        </Trail>
  </div>
</template>

<script>

import Trail from './components/Trail'
export default {
   name: "app",
   components: {
      Trail
   },
   data() {
      return {
        
      };
   },


};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Oswald:400,500,600|Roboto+Slab:300,400&display=swap");

body {
   font-family: "Roboto Slab", serif;
   font-size: 18px;
}

h1,
h2,
h3,
h4,
h5,
h6,
th {
   font-family: "Oswald", sans-serif;
   text-transform: uppercase;
}
</style>
