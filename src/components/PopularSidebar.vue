<template>
<div class="tr-section tr-widget">
    <div class="widget-title">
        <span>Popular this month</span>
    </div>
    <ul class="medium-post-list">
        <li class="tr-post" v-for="post in posts">
			    <div class="post-content">
			      <div class="catagory">
				      <a :href="baseUrl+'source/'+post.category_id.Slug+'/'">{{ post.category_id.Title }} [{{ post.hits }}]</a>
				    </div>
			      <h2 class="entry-title">
				      <a :href="baseUrl+post.slug+'/'">{{ post.title }}</a>
				    </h2>
			    </div>
		    </li>
        <li class="tr-post">
			    <div class="post-content">
			      <div class="catagory">
				      More...
				    </div>
			      <h2 class="entry-title">
				      <a :href="baseUrl+keyword+'/popular/'">All popular posts</a>
				    </h2>
			    </div>
		    </li>
	  </ul>
</div>
</template>

<script>
import axios from 'axios'
import vars from '../variables.js'

export default {
  name: 'popular',
  data () {
    return {
      posts: this.posts,
      baseUrl: vars.baseUrl,
      keyword: vars.keyword
    }
  },
  methods: {
    fetchPosts () {
      axios.get('/most_popular/').then(response => {
        this.posts = response.data
      }).catch(e => {
        console.log(e)
      })
    }
  },
  mounted () {
    this.fetchPosts()
  }
}
</script>

<style></style>