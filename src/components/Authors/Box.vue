<template>
  <div class="col author text-center">
    <router-link :to="{ name: 'Author', params: { id: author.id } }">
      <img :src="image"></img>
      <p>{{ author.name }}</p>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'Box',
  props: ['author'],
  data () {
    return {
      image: `http://via.placeholder.com/200x200?text=${encodeURIComponent(this.author.name)}`
    }
  },
  created () {
    this.pullImage()
  },
  methods: {
    async pullImage () {
      const response = await fetch(`http://api.duckduckgo.com/?q=${encodeURIComponent(this.author.name)}&format=json&pretty=1`)
      const json = await response.json()
      if (json.Image) {
        this.image = json.Image
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.author {
  min-width: 260px;

  img {
    object-fit: cover;
    width: 100%;
    height: 260px;
  }
}
</style>
