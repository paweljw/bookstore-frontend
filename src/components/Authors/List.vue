<template>
  <div>
    <div class="page-header">
      <div class="float-right">
        <input type="text" name="search" v-model="search" placeholder="Search..." class="form-control"></input>
      </div>
      <h1>All authors</h1>
    </div>
    <div class="row">
      <Box v-for="author in authors" :key="author.id" :author="author" v-show="searchMatch(author.name)"></Box>
    </div>
  </div>
</template>

<script>
import Box from './Box'

export default {
  name: 'List',
  data () {
    return {
      search: '',
      authors: []
    }
  },
  created () {
    this.$http.get('/authors')
        .then(request => this.buildAuthorList(request.data))
        .catch(() => { alert('Something went wrong!') })
  },
  methods: {
    buildAuthorList (data) {
      this.authors = data
    },
    searchMatch (authorName) {
      return authorName.toLowerCase().match(this.searchRegExp)
    }
  },
  computed: {
    searchRegExp () {
      return new RegExp(`(.*)${this.search}(.*)`)
    }
  },
  components: {
    Box
  }
}
</script>