<template>
  <div>
    <BookItem v-for="book in books" :key="book.id" :book="book"
      @bookAdded="onBookAdded(book)"
      @bookDeleted="onBookDeleted(book)"
    />
  </div>
</template>

<script>
import BookItem from "./BookItem"

export default {
  props: {
    books: {
      type: Array,
      required: true
    }
  },
  methods: {
    onBookAdded(book) {
      this.$emit('bookAdded', book)
    },
    onBookDeleted(book) {
      this.$delete('bookDeleted', book)
    }
  },
  components: {
    BookItem
  }
}
</script>

<style>
</style>