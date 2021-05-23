<template>
  <div id="app">
    <div>
    <img alt="pochlib_logo" src="./assets/pochlib_logo.png">
  </div>
   <div class="btnAjouter"> 
    <button depressed rounded dark> Ajouter un livre </button> 
 </div> 
 <div class="query">
      <form @submit.prevent= "search">
        <div>
          <input type="text" v-model= "keyword" placeholder="Titre du livre..." class="input-title" required>
          <input type="submit" value="Rechercher" class="button-title">
          <br> 
          <br>
          <input type="text" v-model= "keyword" placeholder="Auteur..." class="input-author" required>
          <input type="submit" value="Rechercher" class="button-author">
        </div>
      </form>
    </div>
    <div class="content">
      <div class="loading" v-if= "loadState == 'loading'"></div>
      <BookList v-if= "loadState == 'success'" :books= "books"/>
    </div>
  </div>  
</template>

<script>
import BookList from '@/components/BookList'
import axios from 'axios'
//import BookItemVue from '@/components/BookItem.vue'

export default {
    data() {
    return {
      books: [],
      keyword: '',
      orderBy: 'relevance',
      maxResults: '20',
      loadState: ''
    }
  },
methods: {
    search() {
      this.loadState = 'loading'
      axios
        .get(
          `https://www.googleapis.com/books/v1/volumes?q=${
            this.keyword
          }&orderBy=${this.orderBy}&maxResults=${this.maxResults}`
        )
        .then(response => {
          console.log(response.data.items)
          this.books = response.data.items
          this.loadState = 'success'
        })
    }
  },
  components: {
    BookList,
    //BookItemVue
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

html, body {
    font-family: 'Roboto', sans-serif;
  }

  #app {
    font-family: 'Roboto', sans-serif;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

#pochlib-logo {
width: 350px;
height: 350px;
}

button {
    display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: rgb(102, 187, 162);
    border: 1px solid rgb(26, 167, 131);
    border-radius: 3px;
    color: whitesmoke;
    font-family: "Quicksand", sans-serif;
    font-size: 1em;
    font-weight: 700;
    letter-spacing: 0.02em;
    line-height: 1;
    text-decoration: none;
    text-transform: uppercase;
    cursor: pointer;
    transition: 0.3s;
  }

  button:hover {
    background: rgb(18, 119, 94);
    color: #fff;
  }

.input-title {
  display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: transparent;
    border: 1px solid rgb(61, 65, 64);
    border-radius: 3px;
    color: black;
    font-family: "Quicksand", sans-serif;
    font-size: 1em;
    font-weight: 700;
    letter-spacing: 0.02em;
    line-height: 1;
}
.button-title {
    display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: rgb(102, 187, 162);
    border: 1px solid rgb(26, 167, 131);
    border-radius: 3px;
    color: whitesmoke;
    font-family: "Quicksand", sans-serif;
    font-size: 1em;
    font-weight: 700;
    letter-spacing: 0.02em;
    line-height: 1;
    text-decoration: none;
    text-transform: uppercase;
    cursor: pointer;
    transition: 0.3s;
}
.button-title:hover {
    background: rgb(18, 119, 94);
    color: #fff;
  }
.input-author {
  display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: transparent;
    border: 1px solid rgb(61, 65, 64);
    border-radius: 3px;
    color: black;
    font-family: "Quicksand", sans-serif;
    font-size: 1em;
    font-weight: 700;
    letter-spacing: 0.02em;
    line-height: 1;
}
.button-author {
    display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: rgb(102, 187, 162);
    border: 1px solid rgb(26, 167, 131);
    border-radius: 3px;
    color: whitesmoke;
    font-family: "Quicksand", sans-serif;
    font-size: 1em;
    font-weight: 700;
    letter-spacing: 0.02em;
    line-height: 1;
    text-decoration: none;
    text-transform: uppercase;
    cursor: pointer;
    transition: 0.3s;
}
.button-author:hover {
    background: rgb(18, 119, 94);
    color: #fff;
  }
</style>