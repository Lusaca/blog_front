<template>
  <div class="posts-index">
    <h1>All Posts</h1>
    <div v-if="posts != null" class="posts">
      <div v-for="post in posts" :key="post.name">
        <div class="post">
          <div class="date">
            {{ post.date }}
          </div>
          <nuxt-link :to="'/posts/' + post.name" class="title-link">
            <h2>
              {{ post.title }}
            </h2>
          </nuxt-link>
          <TagsList :tags="post.tags"></TagsList>
        </div>
      </div>
    </div>
    <div v-if="next_page != null">
      <button class="lmp" @click="loadPosts()">Load More Posts</button>
    </div>
  </div>
</template>

<style lang="scss">
.posts-index {
  width: calc(100% - 40px);
  max-width: 1020px;
  padding: 20px;
  margin-left: auto;
  margin-right: auto;
  background: #fff;
  border-radius: 4px;
  margin-top: 20px;
  h1 {
    border-bottom: var(--border-color) dashed 1px;
    display: inline-block;
    font-size: 48px;
    margin-bottom: 15px;
  }
  .post {
    border-bottom: var(--border-color) dashed 1px;
    padding-bottom: 20px;
    .title-link {
      color: #000;
      text-decoration: none;
      &:hover {
        color: #0366d6;
        text-decoration: underline;
      }
    }
    h2 {
      font-size: 28px;
      margin-top: 10px 0;
    }
    .date {
      font-family: var(--logo-font-family);
      color: var(--sub-color);
      margin-top: 20px;
    }
  }
  .lmp {
    width: 100%;
    border-radius: 4px;
    border: none;
    background: var(--accent);
    color: #fff;
    font-weight: bold;
    padding: 10px 20px;
    font-size: 20px;
    margin-top: 20px;
    &:focus {
      outline: none;
    }
  }
}
</style>

<script>
import Vue from "vue";
export default Vue.extend({
  data() {
    return {
      posts: null,
      next_page: null,
    };
  },
  head() {
    return {
      title: "All Posts - The Lusaca Blog",
    };
  },
  methods: {
    loadPosts() {
      fetch(`http://localhost:3000/posts?page=${this.next_page}`)
        .then((response) => {
          return response.json();
        })
        .then((json) => {
          let now_posts = this.posts;
          this.posts = now_posts.concat(json.posts);
          this.next_page = json.next_page;
        });
    },
  },
  created() {
    fetch("http://localhost:3000/posts")
      .then((response) => {
        return response.json();
      })
      .then((json) => {
        this.posts = json.posts;
        this.next_page = json.next_page;
      });
  },
});
</script>