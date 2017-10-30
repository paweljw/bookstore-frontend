<template>
  <div class="row">
    <div class="col-12 col-md-3 border-right">
      <Box :author="author" v-if="author"></Box>
    </div>
    <div class="col-12 col-md-9">
      <List :author="author" v-if="author"></List>
    </div>
  </div>
</template>

<script>
import Box from './Box'
import List from '@/components/Books/List'

export default {
  name: 'Single',
  data () {
    return {
      author: null
    }
  },
  created () {
    this.$http.get(`/authors/${this.$route.params.id}`)
        .then(request => { this.author = request.data })
        .catch(() => { alert('Something went wrong!') })
  },
  components: {
    Box,
    List
  }
}
</script>

<style lang="scss" scoped>
.border-right {
  border-right: 1px #eee solid;
}
</style>
