<template>
  <div class="home">
    
    <section class="hero is-primary">
  <div class="hero-body">
    <!-- <p class="title"> -->

      <div class="container">
        
      Article
    <!-- </p> -->
    <p class="subtitle">
      <!-- Primary subtitle -->

      
    </p>
    <form @submit.prevent action="">
      <!-- <h2>Ajouter un article</h2> -->
      <div class="columns">
        <div class="column is-half">
          <div class="field">
            <label class="label">Titre d'article</label>
            <div class="control is-half">
              <input class="input" v-model="postTitle" type="text" placeholder="Ex: lorem ipsum">
            </div>
          </div>
        </div>
      </div>
      
      <div class="columns">
        <div class="column is-half">
          <div class="field">
            <label class="label">Titre d'article</label>
            <div class="control">
              <input class="input" v-model="postBody" type="text" placeholder="Ex: Dolor ammet ijh">
            </div>
          </div>
        </div>
      </div>

      <div class="field">
        <p class="control">
          <button @click="createPosts" type="submit" class="button is-success">
            Enregistrer
          </button>
        </p>
      </div>

    </form>
      </div>
  </div>
</section>

<div class="container">

    <!-- <form @submit.prevent action="">
      <h2>Ajouter un article</h2>
      <div class="columns">
        <div class="column is-half">
          <div class="field">
            <label class="label">Titre d'article</label>
            <div class="control is-half">
              <input class="input" v-model="postTitle" type="text" placeholder="Ex: lorem ipsum">
            </div>
          </div>
        </div>
      </div>
      
      <div class="columns">
        <div class="column is-half">
          <div class="field">
            <label class="label">Titre d'article</label>
            <div class="control">
              <input class="input" v-model="postBody" type="text" placeholder="Ex: Dolor ammet ijh">
            </div>
          </div>
        </div>
      </div>

      <div class="field">
        <p class="control">
          <button @click="createPosts" type="submit" class="button is-success">
            Enregistrer
          </button>
        </p>
      </div>

    </form> -->


    <div id="parent" class="columns is-multiline" style="margin-top: 40px;" v-if="datas.length > 0">
      <div class="column is-one-third" v-for="data in datas" :key="data.id">

        <div class="card">
          <header class="card-header">
            <p class="card-header-title" @blur="updatePost(data.id)" :id="'title' + data.id" :class="{ editing: data.id === editing }" :contenteditable="data.id === editing">
              {{ data.title }}

            </p>
            <button class="card-header-icon" aria-label="more options">
              <span class="icon">
                <i class="fas fa-angle-down" aria-hidden="true"></i>
              </span>
            </button>
          </header>
          <div class="card-content">
            <div class="content">
            <p @blur="updatePost(data.id)" :id="'body'+data.id" :contenteditable="data.id === editing">{{ data.body.substring(0, 100) + ' ...' }}</p>
            </div>
          </div>
          <footer class="card-footer">
            <span class="card-footer-item has-background-danger has-text-white cp" @click="deletePost(data.id)">Supprimer</span>
            <span class="card-footer-item has-background-info has-text-white cp" @click="editPost(data.id)"> Editer </span>
          </footer>
        </div>


      <!-- <div class="simple-card">
        <div v-if="!edit">
          <div @click="editPost(data.id)" class="edit">
            <span>Editer</span>
          </div>
          <h2 @blur="updatePost(data.id)" :id="'title' + data.id" :class="{ editing: data.id === editing }" :contenteditable="data.id === editing">{{ data.title }} </h2>
          <p @blur="updatePost(data.id)" :id="'body'+data.id" :contenteditable="data.id === editing">{{ data.body }}</p>
        </div>
        <div v-else>
            <div v-if="!edit">
            <div class="close">
              <span @click="deletePost(data.id)">&times;</span>
            </div>
            <div @click="editPost" class="edit">
              <span>Editer</span>
            </div>
            <h2>{{ data.title }} </h2>
            <p>{{ data.body.substring(0, 100) + ' ...' }}</p>
          </div>
        </div>
        
      </div> -->

      </div>
    </div>

</div>
    
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import axios from 'axios'
import swal from 'sweetalert';


const apiUri = 'https://x8ki-letl-twmt.n7.xano.io/api:aGRFFROk/posts';

export default {
  name: 'Home',

  data(){
    return{
      
      datas: [],
      postTitle: null,
      postBody: null,

      edit: false,
      editing: null

    }
  },

  methods:{
    createPosts(){

      axios.post(apiUri, {
          titles: this.postTitle,
          bodys: this.postBody
      })
      .then(response => {

        this.datas.push(response.data)

        swal("Good job!", "Votre article a ete ajouter avec!", "success");

        // this.datas = response.data
        console.log(response)

      }).catch(error => {

        swal({
          title: "Erreur lors de l'ajout"
        });


        console.log(error)
      })

    },

    editPost(id){
      console.log('edit')

      this.editing = id

      console.log(id)

      setTimeout(() => {

        document.getElementById('title'+id).style.border = '1px solid #000'
        document.getElementById('body'+id).style.border = '1px solid #000'

      }, 0);



      // this.editing = id;
      // setTimeout
    },

    updatePost(id){
      
      const post = this.datas.find(post => post.id === id);
      console.log(post)

      const newTitle = document.getElementById('title'+id).innerText
      const newBody = document.getElementById('body'+id).innerText

      axios.post(apiUri+ `/${id}`,{
        id: id,
        title: newTitle,
        body: newBody
      })
      .then(response  => {
        console.log(response)
        document.getElementById('title'+id).style.border = 'none'
        document.getElementById('body'+id).style.border = 'none'
      })

    },

    deletePost(id){

      console.log(id)

      const post = this.datas.find(post => post.id === id )

      axios.delete(apiUri + `/${id}`)
      .then(response => {
        console.log(response)

        this.datas.splice(this.datas.findIndex(curr => curr.id == id), 1)

        alert(`article n ${post.id} supprimer avec succes`)

      })
      .catch(error => {
        console.log(error)
      })
    }
  },

  components: {
  },

  mounted(){

    axios.get(apiUri)
    .then(response => {
      this.datas = response.data
      console.log(response.data)
    })
    .catch(error => console.log(error))
  }
}
</script>

<style>
  /* .simple-card{
    background-color: #dbdbdb;
    border: 1px solid #000;
    margin: 5px;
    padding: 5px;
    width: 350px;
  } */

  /* #parent{
    display: flex;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
  } */

  .close{
    font-size: 25px;
    text-align: right;
    cursor: pointer;
    display: inline-block;
  }

  .close span{
    color: red;
  }

  .edit{
    color: blue;
    display: inline-block;
    margin-left: 7px;
    cursor: pointer;
    
  }

  .cp{
    cursor: pointer;
  }
</style>
