<template>
<div>
  <header-component></header-component>
    <div class="col-sm-9">
      <div class="row">
        <div class="col-sm-8">
          <div class="tr-content">
            <ad-component :type="0"></ad-component>
            <div class="tr-section bg-transparent">
            <div class="section-title">
              <h1><span><a href="#"><!-- Section title --></a></span></h1>
            </div>
            <div class="row" v-for="chunk in chunkPosts">
              <div class="col-md-6 medium-post" v-for="post in chunk">
                <div class="tr-post">
                  <div class="entry-header">
                    <div class="entry-thumbnail" v-if="post.image">
                      <a :href="baseUrl+post.slug+'/'">
                      <img class="img-responsive" :src="imgBaseUrl+post.image" :alt="post.title"></a>
                    </div>
                  </div>
                  <div class="post-content">
                    <div v-bind:class="[(post.image) ? 'crop' : 'crop-no-img']">
                          <a :href="baseUrl+keyword+'/source/'+post.category_id.Slug+'/'" v-if="post.category_id.Thumbnail">
                          <img class="img-responsive circle-img" :src="imgBaseUrl+post.category_id.Thumbnail" :alt="post.category_id.Title"></a>
                        </div>
                        <div class="entry-meta">
                          <ul>
                            <li>By <a :href="baseUrl+keyword+'/source/'+post.category_id.Slug+'/'">{{ post.category_id.Title }}</a></li>
                            <li>{{ post.date | formatDate }}</li>
                          </ul>
                        </div>
                        <h2 class="entry-title">
                        <a :href="baseUrl+post.slug+'/'">{{ post.title }}</a> 
                        <div v-bind:class="[(post.sentiment >= 0) ? 'sentiment-pos' : 'sentiment-neg']" v-if="post.sentiment">[{{ post.sentiment }}]</div></h2>
                        <p c-if="post.summary">{{ post.summary }}</p>
                  </div>
                </div>
              </div>
            </div>
            <paginator-component v-once :pages="calcPages"></paginator-component>            
          </div>
        </div>
      </div>

      <div class="col-sm-4 tr-sidebar">
        <div>
          <ad-component :type="1"></ad-component>

          <div class="tr-section tr-widget tr-ad ad-before">
            <popular-posts></popular-posts>
          </div>
        </div>
      </div>
    </div>
  </div>
<footer-component></footer-component>
</div>
</template>

<script>
import axios from 'axios'
import vars from '../variables.js'
import Header from './Header.vue'
import Footer from './Footer.vue'
import Paginator from './Paginator.vue'
import Popular from './PopularSidebar.vue'
import chunk from '../chunk'
import Ads from './Ads.vue'

export default {
  name: 'posts',
  data () {
    return {
      posts: this.posts,
      baseUrl: vars.baseUrl,
      keyword: vars.keyword,
      imgBaseUrl: vars.imgBaseUrl
    }
  },
  methods: {
    fetchData () {
      axios.get('/posts/' + this.$route.params.page + '/').then(response => {
        this.posts = response.data
      }).catch(e => {
        console.log(e)
      })
    }
  },
  props: {
    page: {
      type: Number,
      required: true
    }
  },
  components: {
    'header-component': Header,
    'footer-component': Footer,
    'popular-posts': Popular,
    'paginator-component': Paginator,
    'ad-component': Ads
  },
  computed: {
    chunkPosts () {
      return chunk(this.posts, 2)
    },
    calcPages () {
      const pages = Math.floor(this.posts[0].total_posts / 20)
      return pages <= 250 ? pages : 250
    }
  },
  mounted () {
    this.fetchData()
  }
}
</script>

<style>
</style>
