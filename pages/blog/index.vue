<template>
  <div id="posts">
    <PostPreview
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      :excerpt="post.previewText"
      :thumbnailImage="post.thumbnailUrl"
      :id="post.id"
    />
  </div>
</template>

<script>
import PostPreview from "../../components/Blog/PostPreview";

export default {
  components: {
    PostPreview
  },
  asyncData(context) {
    return context.app.$storyapi
      .get("cdn/stories", {
        version: context.isDev ? "draft" : "published",
        starts_with: "blog/"
      })
      .then(res => {
        // console.log(res);
        return {
          posts: res.data.stories.map(bp => {
            return {
              id: bp.slug,
              title: bp.content.title,
              previewText: bp.content.summary,
              thumbnailUrl: bp.content.thumbnail
            };
          })
        };
      });
  }
  // data() {
  //   return {
  //     posts: [
  //       {
  //         title: "A New Beginning",
  //         previewText: "This will be awesome",
  //         thumbnailUrl:
  //           "http://www.healthyfood.co.uk/wp-content/uploads/2015/01/Cherry-tomato-bocc-olive-basil-pasta.jpg",
  //         id: "a-new-beginning"
  //       },
  //       {
  //         title: "A Second Beginning",
  //         previewText: "This will be awesome for the second",
  //         thumbnailUrl:
  //           "http://www.healthyfood.co.uk/wp-content/uploads/2015/01/Cherry-tomato-bocc-olive-basil-pasta.jpg",
  //         id: "a-second-beginning"
  //       }
  //     ]
  //   };
  // }
};
</script>

<style>
#posts {
  padding-top: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
@media (min-width: 35rem) {
  #posts {
    flex-direction: row;
  }
}
</style>
