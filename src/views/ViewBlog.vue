<template>
  <main class="post-view" v-if="currentBlog">
    <div class="container quillWrapper">
      <h1>{{ this.currentBlog[0].blogTitle }}</h1>
      <h4>
        Posté le :
        {{
          new Date(this.currentBlog[0].blogDate).toLocaleString("fr-fr", {
            dateStyle: "long",
          })
        }}
      </h4>
      <img :src="this.currentBlog[0].blogCoverPhoto" alt="" />
      <div
        class="post-content ql-editor"
        v-html="this.currentBlog[0].blogHTML"
      ></div>
      <div class="post-content-author">
        {{ this.currentBlog[0].blogAuthor }}
      </div>
      <div v-if="user" class="author" ref="profile">
        <p>
          {{ this.$store.state.profileFirstName }}
        </p>
      </div>
    </div>
  </main>
</template>

<script>
import "firebase/auth";
export default {
  name: "ViewBlog",
  data() {
    return {
      currentBlog: null,
      currentAuthor: "",
    };
  },
  async mounted() {
    this.currentBlog = await this.$store.state.blogPosts.filter((post) => {
      return post.blogID === this.$route.params.blogid;
    });
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    admin() {
      return this.$store.state.profileAdmin;
    },
  },
};
</script>

<style lang="scss">

.post-view {
  h4 {
    font-weight: 400;
    font-size: 14px;
    margin-bottom: 24px;
  }

  img {
    height: 250px;
    object-fit: contain;
    max-height: 250px;
  }

}
 .post-content{ p::first-letter {
    color: blue;
    font-size: 400%;
    font-weight: 900;
  }}

.author {
  margin: 30px 0;
  font-style: italic;
}
</style>
