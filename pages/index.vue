// the posts are passed down to the Project List component to be rendered

<template>
  <div>
    <ProjectList :posts="posts" />
  </div>
</template>

<script>
    import ProjectList from '~/components/ProjectList.vue'

    export default {
      components: {
        ProjectList
      },
      layout: 'layout',
      async asyncData() {
          // create context via webpack to map over all project posts
          const allPosts = await require.context("~/content/projects/", true, /\.md$/)
          const posts =  allPosts.keys().map((key) => {
            // give back the value of each post context
            return allPosts(key)
          });
          return {
            posts
          }
      }
    }
</script>
