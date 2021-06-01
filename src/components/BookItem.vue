<template>
  <div>
    <template v-if= "info.imageLinks">
      <img :src= "info.imageLinks.thumbnail" :alt="info.title">
    </template>
    <template v-else>
      <img
        src="https://s3-eu-west-1.amazonaws.com/course.oc-static.com/projects/Salesforce_P1_FR/unavailable.png"
        :alt= "unavailable"
        width="128"
      >
    </template>

    <h1>{{ info.title }}</h1>

    <h2 class="author">
      <span v-if="!info.authors">Information manquante</span>
      <span v-else>{{ info.authors[0] }}</span>
    </h2>

    <span>{{ book.id }}</span>

    <h3 class="description">
      <span v-if="!info.description">Information manquante</span>
      <span v-else>{{ info.description.substring(0, 200) }}</span>
    </h3>


    <img @click="onAdd" src="../assets/bookmark-regular.svg" alt="bookmark" id="onAddBookmark"> 
    <img @click="onRemove" src="../assets/trash-alt-regular.svg" alt="trashcan" id="onRemoveTrashcan">
     
  </div>
</template>

<script>
export default {
  props: {
    book: {
      type: Object,
      required: true
    }
  },
  methods: {
    onAdd() {
      this.$emit('bookAdded')
    },
    onRemove(){
      this.$emit('bookDeleted')
    }
  },
  computed: {
    info(){
      return this.book.volumeInfo
    }
  }
}
</script>

<style scoped>
ul {
  padding: 0;
}

ul li {
  display: inline-block;
}

ul li:first-child {
  list-style: none;
}
.author {
  font-size: 14px;
}
</style>

