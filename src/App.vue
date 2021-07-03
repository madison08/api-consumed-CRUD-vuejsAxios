<template>
  <div class="home">

    <form @submit.prevent action="">
      <h2>Ajouter un article</h2>

      <input v-model="postTitle" type="text" placeholder="titre de l'article">
      <input v-model="postBody" type="text" placeholder="corps de l'article">
      <button @click="createPosts" type="submit">Enregistrer</button>

    </form>


    <div id="parent" v-if="datas.length > 0">
      <div v-for="data in datas" :key="data.id">

        
      <div class="simple-card">
        <div v-if="!edit">
          <div class="close">
            <span @click="deletePost(data.id)">&times;</span>
          </div>
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
        
      </div>

      </div>
    </div>

    
    
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import axios from 'axios'

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
          title: this.postTitle,
          body: this.postBody
      })
      .then(response => {

        this.datas.push(response.data)

        alert('utilisateur ajouter avec succes');
        // this.datas = response.data
        console.log(response)

      })

    },

    editPost(id){
      console.log('edit')

      this.editing = id

      console.log(id)

      setTimeout(() => {

        document.getElementById('title'+id).style.border = '1px solid red'
        document.getElementById('body'+id).style.border = '1px solid red'

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
  .simple-card{
    background-color: #dbdbdb;
    border: 1px solid #000;
    margin: 5px;
    padding: 5px;
    width: 350px;
  }

  #parent{
    display: flex;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
  }

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
</style>
