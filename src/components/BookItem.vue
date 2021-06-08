<template>
  <div class="book">
    <div v-if="showAddBookmark">
      <img @click="onAdd"
           src="../assets/bookmark-regular.svg"
           alt="bookmark"
           class="actionIcon">
    </div>
    <div v-if="showTrashcan">
      <img @click="onRemove"
           src="../assets/trash-alt-regular.svg"
           alt="trashcan"
           class="actionIcon">
    </div>
    <div>
      <div class="thumbnail">
        <img v-if= "info.imageLinks"
            :src= "info.imageLinks.thumbnail"
            :alt="info.title">
        <img v-else
            src="https://s3-eu-west-1.amazonaws.com/course.oc-static.com/projects/Salesforce_P1_FR/unavailable.png"
            alt= "Unavailable">
      </div>
    
      <div class="details">
        <h1>{{ info.title }}</h1>

        <h2 class="author">
          <span v-if="!info.authors">Information manquante</span>
          <span v-else > Auteur / Autrice: {{ info.authors[0] }} </span>
        </h2>

        <p> ID: {{ book.id }}</p>

        <h3 class="description">
          <p>
          <span v-if="!info.description">Information manquante</span>
          <span v-else>{{ info.description.substring(0, 200) }}</span>
          </p>
        </h3>
      </div>
    </div>
  </div>
</template>

<script>
export default {
   props: {
    book: {
      type: Object,
    },
    showAddBookmark: {
      type: Boolean,
    },
    showTrashcan: {
      type: Boolean,
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

.book {
  background: #fff;
  border-radius: 2px;
  display: inline-block;
  margin: 1rem;
  position: relative;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
  text-align: left;
}

.thumbnail {
  float: left;
  padding: 16px;
}

.details {
  float: left;
}

ul {
  padding: 0;
}

ul li {
  display: inline;
}

ul li:first-child {
  list-style: none;
}

.author {
  font-size: 14px;
}

.description {
  max-width: 40%;
  display: inline-block;
} 

.actionIcon {
  height: 40px;
  width: auto;
  border: none;
}
</style>

