<template>
  <div class="top-page-posts">
    <section class="posts">
      <div v-if="posts != null">
        <div v-for="post in posts" :key="post.title" class="post">
          <div v-if="post.thumbnail != ''">
            <img
              v-bind:src="post.thumbnail"
              alt="thumbnail image"
              loading="lazy"
              class="thumbnail"
            />
          </div>
          <div class="date">
            {{ post.date }}
          </div>
          <nuxt-link :to="'/posts/' + post.name" class="post-title">
            <h2>
              {{ post.title }}
            </h2>
          </nuxt-link>
          <TagsList :tags="post.tags"></TagsList>
        </div>
      </div>
      <div v-else>
        <Loading />
      </div>
    </section>
    <section class="top-page-search-area">
      <SearchForm />
    </section>
  </div>
</template>

<style lang="scss">
.top-page-posts {
  .posts {
    width: calc(100% - 40px);
    max-width: 1020px;
    padding: 20px;
    margin-left: auto;
    margin-right: auto;
    background: #ffffff;
    margin-top: 20px;
    border-radius: 4px;
    .post {
      border-bottom: var(--border-color) dashed 1px;
      padding-bottom: 20px;
      .thumbnail {
        width: 100%;
        border-radius: 4px;
      }
      .post-title {
        color: #000;
        text-decoration: none;
        &:hover {
          color: #0366d6;
          text-decoration: underline;
        }
      }
      h2 {
        font-size: 35px;
        margin: 10px 0;
      }
      .date {
        font-family: var(--logo-font-family);
        color: var(--sub-color);
        margin-top: 20px;
      }
    }
    .post:last-child {
      border-bottom: none;
    }
  }
  .now_loading {
    color: var(--sub-color);
    font-family: var(--logo-font-family);
    text-align: center;
    font-weight: bold;
  }
}
.top-page-search-area {
  width: calc(100% - 40px);
  max-width: 1020px;
  padding: 20px;
  margin-left: auto;
  margin-right: auto;
  background: #ffffff;
  margin-top: 20px;
  border-radius: 4px;
}
</style>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  data() {
    return {
      posts: null,
    };
  },
  created() {
    fetch("http://localhost:3000")
      .then((response) => {
        return response.json();
      })
      .then((json) => {
        this.posts = json;
      })
      .catch((err) => {
        this.posts = null;
      });
  },
});
</script>