<template>
  <div id="app">
    <div>
      <img alt="pochlib_logo" src="./assets/pochlib_logo.png">
    </div>

    <h1>Ma Poch'liste</h1>
    <BookList :books="savedBooks"/>

    <button @click="showForm = !showForm" depressed rounded dark>Ajouter un livre</button>
    <div v-if="showForm" class="query">
      <form @submit.prevent="search">
        <div>
          <input v-model="titleSearch" placeholder="Titre du livre..." class="input-text">
          <input v-model="authorSearch" placeholder="Auteur..." class="input-text">
          <br>
          <br>
          <button>Rechercher</button>
          <button @click="clearSearch" type="button" depressed rounded dark>Annuler</button>
        </div>
      </form>
    </div>
    <div class="content">
      <div class="loading" v-if="loadState == 'loading'"></div>
      <BookList v-if="loadState == 'success'" :books="books"/>
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
      showForm: false,
      books: [],
      savedBooks: [],
      titleSearch: '',
      authorSearch: '',
      orderBy: 'relevance',
      maxResults: '20',
      loadState: ''
    }
  },
  methods: {
    search() {
      this.loadState = 'loading'

      let keyword = ''
      if (this.titleSearch !== undefined && this.titleSearch !== '') {
        keyword = this.titleSearch
        if (this.authorSearch !== undefined && this.authorSearch !== '') {
          keyword += `+inauthor:${this.authorSearch}`
        }
      } else {
        keyword = this.authorSearch
      }

      axios
        .get(
          `https://www.googleapis.com/books/v1/volumes?q=${
            keyword
          }&orderBy=${this.orderBy}&maxResults=${this.maxResults}`
        )
        .then(response => {
          console.log(response.data.items)
          this.books = response.data.items
          this.loadState = 'success'
        })
    },
    clearSearch() {
      this.titleSearch = ''
      this.authorSearch = ''

      this.loadState = ''
      this.books = []
    },
    addToMyPochList() {
      const savedBooks = []

      //const savedBooks = [
      //  {
      //    id: "531513",
      //    volumeInfo: {
      //      authors: ["Anthony Drogon"],
      //      title: "Pippi Longstocking",
      //      description: "Some description",
      //      imageLinks: {
      //        thumbnail: ''
      //      }
      //    }
      //  }
      //]

      const savedBooksAsString = JSON.stringify(savedBooks)

      sessionStorage.setItem('savedBooks', savedBooksAsString)
    }
  },
  mounted(){
    const savedBooks = sessionStorage.getItem('savedBooks')

    const savedBooksAsJSON = JSON.parse(savedBooks)

    this.savedBooks = savedBooksAsJSON
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

  img {
      height: 160px;
      width: auto;
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

  .input-text {
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

  .search-button {
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

  .search-button:hover {
    background: rgb(18, 119, 94);
    color: #fff;
  }
</style>