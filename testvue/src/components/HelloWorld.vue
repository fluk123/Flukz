<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
import * as firebase from 'firebase'

var firebaseConfig = {
    apiKey: 'AIzaSyAl0ayME29LLCfyhyB1o2fAoMriLiedCJY',
    authDomain: 'testvue-f686e.firebaseapp.com',
    databaseURL: 'https://testvue-f686e.firebaseio.com',
    projectId: 'testvue-f686e',
    storageBucket: 'testvue-f686e.appspot.com',
    messagingSenderId: '490102155967',
    appId: '1:490102155967:web:782c8a973d3f0482c4340a',
    measurementId: 'G-XZW1P1XTX8'
  }
  firebase.initializeApp(config)

  var database = firebase.database()
  var contactRef = database.ref('/contacts')
export default {
  name: 'HelloWorld',
  data(){
    return{
        contacts:{},
        name: '',
        lastname: '',
        updateLastName: '',
        updateName: '',
        updateKey: ''
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
   setUpdateContact (key,contact){
     this.updateKey = key
     this.updateLastName = contact.lastname
     this.updateName = contact,name
   },
   updateContact(name ,lastname){
     contactRef.child(this.updateKey).update({
       name: name,
       lastname: lastname
     })
     this.updateKey = ''
     this.updateName = ''
     this.updateLastName = ''
   },
   deleteContact(key){
  contactRef.child(key).remove()
  }
},
mounted(){
  contactRef.on('value',(snapshot) => {
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
