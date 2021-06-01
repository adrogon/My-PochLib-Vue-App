<template>
  <div id="app">
    <div>
      <img alt="pochlib_logo" src="./assets/pochlib_logo.png" id="logo">
    </div>
    <h1>Nouveau livre </h1>
    <BookList :books="savedBooks" @bookDeleted="removeFromPochListe"/>
    <button @click="showForm = !showForm" depressed rounded dark>Ajouter un livre</button>
    <br>
    <br>
    <div v-if="showForm" class="query">
      <form @submit.prevent="search">
        <div>
          <input v-model="titleSearch" placeholder="Titre du livre..." class="input-text">
          <br>
          <br>
          <input v-model="authorSearch" placeholder="Auteur..." class="input-text">
          <br>
          <br>
          <button>Rechercher</button>
          <button @click="clearSearch" type="button" id="clear-search-btn" depressed rounded dark>Annuler</button>
        </div>
      </form>
    <h1>Ma Poch'liste</h1>
    <h1>Résultat de recherche</h1>
    </div>
    <div class="content">
      <div class="loading" v-if="loadState == 'loading'"></div>
      <BookList v-if="loadState == 'success'" :books="books" @bookAdded="addToMyPochList"/>
    </div>
  </div>
</template>

<script>
import BookList from '@/components/BookList'
import axios from 'axios'

export default {
  data() {
    return {
      showForm: false,
      books: [],
      savedBooks: [],
      titleSearch: '',
      authorSearch: '',
      orderBy: 'relevance',
      maxResults: '10',
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
          if (this.response == '') {
            this.$alert("Aucun livre n’a été trouvé");  
          }
          else
            {
          console.log(response.data.items)
          this.books = response.data.items
          this.loadState = 'success'
          }
        })
    },

    clearSearch() {
      this.titleSearch = ''
      this.authorSearch = ''
      this.loadState = ''
      this.books = []
      this.showForm = ''
    },

    addToMyPochList(book) {
      if (!sessionStorage.getItem('savedBooks')) {
        sessionStorage.setItem('savedBooks', '[]')
      }

      const currentSavedBooks = sessionStorage.getItem('savedBooks')

      const currentSavedBooksAsJSON = JSON.parse(currentSavedBooks)

      currentSavedBooksAsJSON.push(book)

      const savedBooksAsString = JSON.stringify(currentSavedBooksAsJSON)

      sessionStorage.setItem('savedBooks', savedBooksAsString)

      this.loadPochListe()
    },

     removeFromPochListe(book) {
       if (!sessionStorage.getItem('savedBooks')) {
        return
      }

      const currentSavedBooks = sessionStorage.getItem('savedBooks')

      const currentSavedBooksAsJSON = JSON.parse(currentSavedBooks)


       const index = currentSavedBooksAsJSON.findIndex(b => b.id === book.id)
      if (index > -1) {
        currentSavedBooksAsJSON.splice(index, 1)

        const savedBooksAsString = JSON.stringify(currentSavedBooksAsJSON)

        sessionStorage.setItem('savedBooks', savedBooksAsString)

      this.loadPochListe()
      }
    }, 

    loadPochListe() {
      let savedBooks = []
      let deletedBooks = []

      if ((sessionStorage.getItem('savedBooks'))  || (sessionStorage.removeItem('deletedBooks'))) {
        savedBooks = JSON.parse(sessionStorage.getItem('savedBooks'))
      }
      this.savedBooks = savedBooks
      this.deletedBooks = deletedBooks
    }
  },
  mounted: () => {
    this.loadPochListe()
  },
  components: {
    BookList
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

  #logo {
    height: 350px;
    width: auto;
    border: none;
  }

  img {
    padding: 30px;
    padding-bottom: 100px;
    border: 0.5px solid #5b5d5f;
    background-color: white;
    height: 250px;
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

  #clear-search-btn {
    display: inline-block;
    margin: 0.5em 0;
    padding: 1em 2em;
    background: rgb(199, 47, 47);
    border: 1px solid rgb(153, 7, 7);
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

  #clear-search-btn:hover {
    background: rgb(153, 7, 7);
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