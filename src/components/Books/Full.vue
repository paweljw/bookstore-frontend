<template>
  <div class="col book text-center">
    <button class="btn btn-light btn-close" v-on:click="close">&times; close</button>
    <img :src="image"></img>
    <p>
      <strong>{{ book.title }}</strong>
      - {{ price }}
    </p>
    <button class="form-control btn btn-lg btn-primary">Add to cart</button>
  </div>
</template>

<script>
export default {
  name: 'Full',
  props: ['author', 'book'],
  data () {
    return {
      image: `http://via.placeholder.com/220x320?text=${encodeURIComponent(this.book.title)}`
    }
  },
  created () {
    this.pullImage()
  },
  updated () {
    this.pullImage()
  },
  methods: {
    async pullImage () {
      this.image = `http://via.placeholder.com/220x320?text=${encodeURIComponent(this.book.title)}`
      const response = await fetch(`http://api.duckduckgo.com/?q=${encodeURIComponent(this.book.title)}&format=json&pretty=1`)
      const json = await response.json()
      if (json.Image) {
        this.image = json.Image
      }
    },
    close () {
      this.$emit('close')
    }
  },
  computed: {
    price () {
      return `${this.book.price_cents / 100} ${this.book.price_currency}`
    }
  }
}
</script>

<style lang="scss" scoped>
.book {
  min-width: 220px;
  max-width: 400px;

  img {
    object-fit: cover;
    width: 100%;
    height: 520px;
  }

  border-left: 1px #eee solid;

  animation: fadein 0.6s;
}

@keyframes fadein {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.btn-close {
  float: right;
  margin-bottom: 5px;
  cursor: pointer;
}
</style>
