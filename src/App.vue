<template>
  <div class="home">
    
    <section class="hero is-primary">
  <div class="hero-body">
    <!-- <p class="title"> -->

      <div class="container">
      
      <p class="title">
        Article
      </p>
      
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

            
<div v-if="isloading" class="loadingio-spinner-spinner-qqf9pv1wftq"><div class="ldio-mre0kqwph9c">
<div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div>
</div></div>

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
      <div v-if="isloadingDel" style="margin: 0 auto;" class="loadingio-spinner-spinner-xaofdxnrom"><div class="ldio-r34yt6996o">
<div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div>
</div></div>
      <div v-else class="column is-one-third" v-for="data in datas" :key="data.id">
        
        
        
        <div class="card">
          <header class="card-header">
            {{ data.id }}
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
            <p @blur="updatePost(data.id)" :id="'body'+data.id" :contenteditable="data.id === editing">{{ data.body }}</p>
            </div>
          </div>
          <footer class="card-footer">
            <span class="card-footer-item cp" @click="deletePost(data.id)">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M3 6v18h18v-18h-18zm5 14c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm5 0c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm5 0c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm4-18v2h-20v-2h5.711c.9 0 1.631-1.099 1.631-2h5.315c0 .901.73 2 1.631 2h5.712z"/></svg>
            </span>
            <span class="card-footer-item cp" @click="editPost(data.id)">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M18 14.45v6.55h-16v-12h6.743l1.978-2h-10.721v16h20v-10.573l-2 2.023zm1.473-10.615l1.707 1.707-9.281 9.378-2.23.472.512-2.169 9.292-9.388zm-.008-2.835l-11.104 11.216-1.361 5.784 5.898-1.248 11.103-11.218-4.536-4.534z"/></svg>
            </span>
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
      datasFil: [],
      postTitle: null,
      postBody: null,
      isloading: false,

      isloadingDel: false,
      isloadingEd: false,

      edit: false,
      editing: null

    }
  },

  methods:{
    createPosts(){

      this.isloading = true

      axios.post(apiUri, {
          title: this.postTitle,
          body: this.postBody
      })
      .then(response => {

        this.isloading = false

        this.datas.unshift(response.data)

        

        swal("succès!", "Votre article a ete ajouter!", "success");

        this.postTitle = '';
        this.postBody = ''

        // this.datas = response.data
        console.log(response)

      }).catch(error => {

        this.isloading = false

        swal({
          title: "Erreur lors de l'ajout",
          dangerMode: true
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

      // this.isloadingEd = true

      this.isloadingDel = true
      
      
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

      // this.isloadingEd = false

      this.isloadingDel = false

        console.log(post)

        axios.get(apiUri)
        .then(response => {
          // this.datas = response.data
          // console.log(response.data)

          this.datas = response.data.sort(function(a,b){
            return b.id - a.id
          })
        })

        swal("succès!", "Votre article a ete Modifer!", "success");


        console.log(response)
        document.getElementById('title'+id).style.border = 'none'
        document.getElementById('body'+id).style.border = 'none'
      })

    },

    deletePost(id){
      swal({
        title: "êtes-vous sûr ?",
        text: "Une fois supprimé, vous ne pourrez plus récupérer cet article !",
        icon: "warning",
        buttons: ["Annuler", true],
        dangerMode: true,
      })
      .then((willDelete) => {
        if (willDelete) {

          this.isloadingDel = true;
          
          axios.delete(apiUri + `/${id}`)
          .then(response => {

          this.isloadingDel = false;


            console.log(response)

            this.datas.splice(this.datas.findIndex(curr => curr.id == id), 1)

            swal("Pouf ! Votre article a été supprimé !", {
              icon: "success",
            });

          })
          .catch(error => {
            console.log(error)
          })
        } 
      });
    },

    // deletePostConfirm(id){

    //   console.log(id)

    //   const post = this.datas.find(post => post.id === id )


      

    //   axios.delete(apiUri + `/${id}`)
    //   .then(response => {
    //     console.log(response)

    //     this.datas.splice(this.datas.findIndex(curr => curr.id == id), 1)

    //     alert(`article n ${post.id} supprimer avec succes`)

    //   })
    //   .catch(error => {
    //     console.log(error)
    //   })
    // }
  },

  components: {
  },

  mounted(){

    axios.get(apiUri)
    .then(response => {
      this.datas = response.data
      // console.log(response.data)

      this.datas = response.data.sort(function(a,b){
        return b.id - a.id
      })

      // console.log(newData)
    })
    .catch(error => console.log(error))


    // console.log(this.datas)

    
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




@keyframes ldio-mre0kqwph9c {
  0% { opacity: 1 }
  100% { opacity: 0 }
}
.ldio-mre0kqwph9c div {
  left: 47px;
  top: 24px;
  position: absolute;
  animation: ldio-mre0kqwph9c linear 1s infinite;
  background: #ffffff;
  width: 6px;
  height: 12px;
  border-radius: 3px / 6px;
  transform-origin: 3px 26px;
}.ldio-mre0kqwph9c div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -0.9166666666666666s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -0.8333333333333334s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.75s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.6666666666666666s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.5833333333333334s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.5s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.4166666666666667s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.3333333333333333s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.25s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.16666666666666666s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.08333333333333333s;
  background: #ffffff;
}.ldio-mre0kqwph9c div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
  background: #ffffff;
}
.loadingio-spinner-spinner-qqf9pv1wftq {
  width: 51px;
  height: 51px;
  display: inline-block;
  overflow: hidden;
  background: none;
}
.ldio-mre0kqwph9c {
  width: 100%;
  height: 100%;
  position: relative;
  transform: translateZ(0) scale(0.51);
  backface-visibility: hidden;
  transform-origin: 0 0; /* see note above */
}
.ldio-mre0kqwph9c div { box-sizing: content-box; }
/* generated by https://loading.io/ */



















@keyframes ldio-r34yt6996o {
  0% { opacity: 1 }
  100% { opacity: 0 }
}
.ldio-r34yt6996o div {
  left: 94px;
  top: 48px;
  position: absolute;
  animation: ldio-r34yt6996o linear 1s infinite;
  background: #000000;
  width: 12px;
  height: 24px;
  border-radius: 6px / 12px;
  transform-origin: 6px 52px;
}.ldio-r34yt6996o div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -0.9166666666666666s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -0.8333333333333334s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.75s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.6666666666666666s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.5833333333333334s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.5s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.4166666666666667s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.3333333333333333s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.25s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.16666666666666666s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.08333333333333333s;
  background: #000000;
}.ldio-r34yt6996o div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
  background: #000000;
}
.loadingio-spinner-spinner-xaofdxnrom {
  width: 200px;
  height: 200px;
  display: inline-block;
  overflow: hidden;
  background: none;
}
.ldio-r34yt6996o {
  width: 100%;
  height: 100%;
  position: relative;
  transform: translateZ(0) scale(1);
  backface-visibility: hidden;
  transform-origin: 0 0; /* see note above */
}
.ldio-r34yt6996o div { box-sizing: content-box; }
/* generated by https://loading.io/ */




</style>
