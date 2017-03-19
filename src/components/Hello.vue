<template>
  <div class="row">
  
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
  
    <div class="col-md-4">
      <input type="text" class="form-control" v-model="newShoppingItem" />
    </div>
  
    <button class="btn btn-primary" @click="AddShoppingItem()">Add</button>
  
    <ul class="list-group" style="margin-top: 30px;">
      <li style="height: 50px; width: 1000px;" class="list-group-item" v-for="item in shoppinglist">
        <div class="col-md-5">
          <div v-if="!editMode">
            {{item.value}}
          </div>
          <div v-else="editMode">
            <input type="text" v-model="item.value" @change="changeShoppingItem(item)" />
          </div>
        </div>
        <div class="col-md-2">
          <button style="float: right;" class="btn btn-primary" @click="changeEdit()">edit</button>
          <button style="float: right;" class="btn btn-danger" @click="removeShoppingItem(item['.key'])">remove</button>
        </div>
      </li>
    </ul>
  
  </div>
</template>

<script>
  import Vue from 'vue';
  import VueFire from 'vuefire';
  import firebase from 'firebase';

  import * as config from './../config';
  
  // explicit installation required in module environments
  Vue.use(VueFire)
  
  var firebaseApp = firebase.initializeApp({
    apiKey: "AIzaSyBBeAqd9qBd_H_JkU5b3YVvWRIFqewqiE0", // Auth / General Use
    authDomain: "sharedlist-40f8d.firebaseapp.com", // Auth with popup/redirect
    databaseURL: "https://sharedlist-40f8d.firebaseio.com"
  }); // Realtime Database
  
  firebase.auth().signInWithEmailAndPassword(config.email, config.password).catch(function(error) {
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
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        newShoppingItem: "",
        editMode: false,
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
      removeShoppingItem: function(key) {
        console.log("remove " + key);
        shoppingRef.child(key).remove();
      },
      changeShoppingItem: function(item) {
        var postData = {
          value: item.value
        }

        var updates = {};
        updates['/sharedList/shoppingList/' + item['.key']] = postData;
        firebase.database().ref().update(updates);
      },
      changeEdit: function(){
        if(this.editMode){
          this.editMode = false;
        } else {
          this.editMode = true;
        }
      }
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  
</style>
