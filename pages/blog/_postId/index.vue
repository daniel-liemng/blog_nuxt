<template>
  <div id="post" v-editable="blok">
    <div class="post-thumbnail" :style="{ backgroundImage: 'url(' + image + ')' }"></div>
    <div class="post-content">
      <h1>{{ title }}</h1>
      <p>{{ content }}</p>
    </div>
  </div>
</template>

<script>
export default {
  asyncData(context) {
    return context.app.$storyapi
      .get("cdn/stories/blog/" + context.params.postId, {
        version: context.isDev ? "draft" : "published"
      })
      .then(res => {
        // console.log(res);
        return {
          // live preview in storyblok
          blok: res.data.story.content,
          image: res.data.story.content.thumbnail,
          title: res.data.story.content.title,
          content: res.data.story.content.content
        };
      });
  },
  mounted() {
    this.$storybridge.on(["input", "published", "change"], event => {
      if (event.action == "input") {
        if (event.story.id === this.story.id) {
          this.story.content = event.story.content;
        }
      } else {
        window.location.reload();
      }
    });
  }
};
</script>

<style scoped>
.post-thumbnail {
  width: 100%;
  height: 300px;
  background-size: cover;
  background-position: center;
}

.post-content {
  width: 80%;
  min-width: 500px;
  margin: auto;
}

.post-content p {
  white-space: pre-line;
}
</style>
