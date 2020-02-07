<template>
  <div class="hello">
    <div>
      <input type="text" v-model="name" placeholder="NAME">
      <input type="text" v-model="lastname" placeholder="LASTNAME">
      <input type="number" v-model="money" placeholder="MONEY">
      <button @click="insertToContact(name,lastname)">Add</button>
    </div>
    <hr>
    <ul :key="key" v-for="(contact,key) in contacts">
      <li v-if="updateKey === key">
        <input type="text" v-model="updateName" placeholder="NAME">
        <input type="text" v-model="updateLastName" placeholder="LASTNAME">
        <input type="number" v-model="updateMoney" placeholder="MONEY">
        <button @click="updateContact(updateName,updateLastName,updateMoney)"> Save</button>
      </li>
      <li v-else>
        {{contact.name}} : {{contact.lastname}}
        <button @click="setUpdateContact(key,contact)">Update</button>
        <button @click="deleteContact(key)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import * as firebase from 'firebase'

var config = {
  aapiKey: "AIzaSyAl0ayME29LLCfyhyB1o2fAoMriLiedCJY",
    authDomain: "testvue-f686e.firebaseapp.com",
    databaseURL: "https://testvue-f686e.firebaseio.com",
    projectId: "testvue-f686e",
    storageBucket: "testvue-f686e.appspot.com",
    messagingSenderId: "490102155967",
    appId: "1:490102155967:web:782c8a973d3f0482c4340a",
    measurementId: "G-XZW1P1XTX8"
}
firebase.initializeApp(config)

var database = firebase.database();
var contactRef = database.ref('/contacts')
export default {
  name: 'HelloWorld',
  data() {
    return {
      contacts: {},
      name: '',
      lastname: '',
      updateLastName: '',
      updateName: '',
      updateKey: '',
    }
  },
  methods: {
    insertToContact(name,lastname){
      let data = {
        name: name,
        lastname: lastname
      }
      contactRef.push(data)
      this.name = ''
      this.lastname = ''
    },
    setUpdateContact(key, contact){
      this.updateKey = key
      this.updateName = contact.name
      this.updateLastName = contact.lastname
    },
    updateContact (name,lastname){
      contactRef.child(this.updateKey).update({
        name: name,
        lastname: lastname
      })
      this.updateKey = ''
      this.updateName = ''
      this.updateLastName = ''
    },
    deleteContact (key){
      contactRef.child(key).remove()
    }
  },
  mounted() {
    contactRef.on('value', (snapshot) => {
      this.contacts = snapshot.val()
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>