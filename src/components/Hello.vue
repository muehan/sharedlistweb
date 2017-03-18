<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    

    <input type="text" v-model="newShoppingItem" />
    <button @click="AddShoppingItem()">Add</button>

    <ul>
      <li v-for="item in shoppinglist">
        {{item.value}}
        <button @click="removeShoppingItem(item['.key'])">remove</button>
        <input type="text" v-model="item.value" @change="changeShoppingItem(item)" />
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

firebase.auth().signInWithEmailAndPassword("userEmail", "userpassword").catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;

  console.log(errorCode);
  console.log(errorMessage);
  
});

var db = firebaseApp.database();

var sharedList = db.ref('sharedList');
var shoppingRef = db.ref('sharedList/shoppingList');

export default {
  name: 'hello',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newShoppingItem: "",
    }
  },
  firebase: {
    list: sharedList,
    shoppinglist: shoppingRef,
  },
  methods: {
    AddShoppingItem: function() {
      var newItem = {
        value: this.newShoppingItem
      };

      shoppingRef.push(newItem);
    },
    removeShoppingItem: function(key){
      console.log("remove " + key);
      shoppingRef.child(key).remove();
    },
    changeShoppingItem: function(item){
      var postData = {
        value: item.value
      }

      var updates = {};
      updates['/sharedList/shoppingList/' + item['.key']] = postData;
      firebase.database().ref().update(updates);
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
