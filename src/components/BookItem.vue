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
      <span v-if="!info.authors">No authors to display</span>
      <span v-else>{{ info.authors[0] }}</span>
    </h2>

    <span>{{ book.id }}</span>

    <h3>{{ description }}</h3>
  </div>
</template>

<script>

// BOOK OBJECT
//{
//  id: String,
//  volumeInfo: {
//    title: String,
//    authors: String[],
//    description: String,
//    imageLinks: {
//      thumbnail: String
//    }
//  }
//}

export default {
  props: {
    book: {
      type: Object,
      required: true
    }
  },
  computed: {
    info(){
      return this.book.volumeInfo
    },
    description(){
      return this.info.description.substring(0, 200)
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
