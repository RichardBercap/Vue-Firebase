<template>
  <div id="app" class="container">
    <h1>Vue and FireBase</h1>

    <div class="card">
      <div class="card-header">
        <h3>Add A Link</h3>
        <div class="card-body">
        <!-- eventos de vuejs que se ejecute el evento addlink-->
          <form v-on:submit.prevent="addLink"  class="form-inline">

            <div class="form-group">
              <label for="">Title</label>
              <input
                placeholder="Title"
                v-model="newLink.title"
                type="text" class="form-control mx-sm-4"/>
            </div>
            
            <div class="form-group">
              <label for="">Author</label>
              <input
                placeholder="Author"
                v-model="newLink.author"
                type="text" class="form-control mx-sm-4"/>
            </div>
            
            <div class="form-group">
              <label for="">Url</label>
              <input
                placeholder="Title"
                v-model="newLink.url"
                type="text" class="form-control mx-sm-4"/>
            </div>

            <input type="submit" class="btn btn-success" value="Add Link">
          </form>
        </div>
      </div> 
    </div>

    <hr>

  <div class="card">
        <div class="card-header">
          <h3 class="card-title">Links List</h3>
        </div>
        <div class="card-block">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Title</th>
                <th>User</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="link in links">
                <td>
                  <a v-bind:href="link.url">
                    {{ link.title }}
                  </a>
                </td>
                <td>{{ link.author }}</td>
                <td>
                  <button 
                    v-on:click="deleteLink(link)"
                    class="btn btn-danger">
                    <i class="fa fa-trash-o" aria-hidden="true"></i>
                  </button>                 
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  </div>


  </div>


</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase';


//configuracion firebase desde la consola del proyecto
let config ={
    apiKey: "AIzaSyCogTcGZbVf0hDoNLKp-CwmPGQ9xutsjxo",
    authDomain: "vuejsfire-a39b8.firebaseapp.com",
    databaseURL: "https://vuejsfire-a39b8.firebaseio.com",
    projectId: "vuejsfire-a39b8",
    storageBucket: "vuejsfire-a39b8.appspot.com",
    messagingSenderId: "726584704011"

};
//inicializar la aplicacion
let app = Firebase.initializeApp(config);
let db = app.database();//traer la bd del proyecto
import toastr from 'toastr';//importamos toastr

let linksRef = db.ref('links');//si no existe firebase lo creara

export default {
  name: 'app',
  firebase:{
    links: linksRef
  },
  data(){
    return {
      newLink:{
        title:'',
        author:'',
        url:''
      }
    }
  },
  methods:{
    //creamos el evento addlink
    addLink:function(){
      linksRef.push(this.newLink);
      this.newLink.title = '';
      this.newLink.author = '';
      this.newLink.url = '';
    },
    deleteLink:function(link){
      //remover el link
      linksRef.child(link['.key']).remove();
      toastr.success('Link removed');
    }
  }
}
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
