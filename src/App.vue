<template>
  <div id="app">
    <img src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import JsonApi from 'devour-client'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  mounted() {
      const jsonApi = new JsonApi({apiUrl: 'https://live-contentacms.pantheonsite.io/api'});
/* Get Normal trae los datos en un objeto Arr de objetos planos.
     jsonApi.define('recipe', {
      title: ''
  });
   jsonApi.findAll('recipes')
       .then(res => {
       console.log(res);
  });*/
// Get con include, hay que definir primero ambos modelos y luego hacer la peticion. El included viene en cada elemento como una key mas.
      jsonApi.define('recipe', {
          title: '',
          difficulty: '',
          category: {
              jsonApi: 'hasOne',
              type: 'categories'
          }
      })

      jsonApi.define('category', {
          name: ''
      });
jsonApi.findAll('recipes', {include: 'category'})
          .then((res) => {
         console.log(res);
      });
      jsonApi.create('recipe', {
          title: 'hello',
          difficulty: 'easy',
      }).then((res) => {
          console.log(res);
      });
      console.log(jsonApi);

  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
