<template>
  <div>
    <div class="page-header">
      <div class="float-right">
        <input type="text" name="search" v-model="search" placeholder="Search..." class="form-control"></input>
      </div>
      <h1>Books by {{ author.name }}</h1>
    </div>
    <div class="row">
      <div class="row col">
        <Box v-for="book in books" :author="author" :book="book" :key="book.id"
          v-show="searchMatch(book.title)" v-on:click.native="setBook(book)"></Box>
      </div>
      <Full :book="currentBook" v-if="currentBook" v-on:close="closeBook"></Full>
    </div>
  </div>
</template>

<script>
import Box from './Box'
import Full from './Full'

export default {
  name: 'List',
  props: ['author'],
  data () {
    return {
      search: '',
      books: [],
      currentBook: null
    }
  },
  created () {
    this.$http.get(`/authors/${this.author.id}/books`)
        .then(request => { this.books = request.data })
        .catch(() => { alert('Something went wrong!') })
  },
  methods: {
    searchMatch (bookTitle) {
      return bookTitle.toLowerCase().match(this.searchRegExp)
    },
    setBook (book) {
      this.currentBook = book
    },
    closeBook () {
      this.currentBook = null
    }
  },
  computed: {
    searchRegExp () {
      return new RegExp(`(.*)${this.search}(.*)`)
    }
  },
  components: {
    Box,
    Full
  }
}
</script>