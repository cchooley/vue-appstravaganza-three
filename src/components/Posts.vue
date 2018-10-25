<template>
  <div class="hello m-3">
    <h2>Posts</h2>
      <ul class="list-unstyled">
        <li v-for="post in posts" class="media m-3">
          <img class="mr-3" :src="post.data.thumbnail" alt="Generic placeholder image">
          <div class="media-body">
            <h4 class="mt-0 mb-1">
              <a :href="createUrl(post.data.permalink)" target="_blank">
                {{post.data.title}}
              </a>
            </h4>
            <h5>{{post.data.ups}} ⬆️</h5>
            <span>Created {{formatDate(post.data.created)}} ago by {{post.data.author}}</span><br>
            <span class="badge badge-pill badge-secondary">{{post.data.num_comments}} comments</span><br>
            <button @click="post.showImage = !post.showImage" type="button" class="btn btn-primary">
              {{post.showImage ? 'Hide' : 'Show'}} Image
            </button>
            <div class="mt-2" v-if="post.showImage">
              <img :src="post.data.url">
            </div>
          </div>
        </li>
      </ul>
  </div>
</template>

<script>
import {parse, distanceInWords} from 'date-fns'

export default {
  name: 'Posts',
  data() {
    return {
      posts: []
    }
  },
  mounted() {
    this.load() //call the load fn when mounted
  },
  methods: {
    load() {
      // fetch the posts
      const url = 'https://www.reddit.com/r/rarepuppers.json'
      fetch(url)
        .then(response => response.json())
        .then((result) => {
          result.data.children.forEach(child => {
            child.showImage = false;
          })
          this.posts = result.data.children
        })
    },
    formatDate(date) {
      return distanceInWords(parse(date * 1000), new Date())
    },
    createUrl(path) {
      return `https://www.reddit.com${path}`
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
