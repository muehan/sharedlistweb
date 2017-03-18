<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    
    <ul>
      <li v-for="item in shoppinglist">
        {{item['.value']}}
      </li>
    </ul>

  </div>
</template>

<script>


import Vue from 'vue';
import VueFire from 'vuefire';
import firebase from 'firebase';

// explicit installation required in module environments
Vue.use(VueFire)

var firebaseApp = firebase.initializeApp({  
  apiKey: "AIzaSyBBeAqd9qBd_H_JkU5b3YVvWRIFqewqiE0",             // Auth / General Use
  authDomain: "sharedlist-40f8d.firebaseapp.com",                // Auth with popup/redirect
  databaseURL: "https://sharedlist-40f8d.firebaseio.com"});      // Realtime Database

var db = firebaseApp.database();

var sharedList = db.ref('sharedList');
var shoppingRef = db.ref('sharedList/shoppingList');

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',

    }
  },
  firebase: {
    list: sharedList,
    shoppinglist: shoppingRef,
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
