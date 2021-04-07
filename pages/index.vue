// the posts are passed down to the Project List component to be rendered

<template>
  <div>
    <ProjectList :posts="filteredPosts" />
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
            posts.sort((a,b) => {
                const dateA = new Date(a.attributes.properties.year);
                const dateB = new Date(b.attributes.properties.year);
                if (dateA < dateB) {
                    return 1;
                }
                if (dateA > dateB) {
                    return -1;
                }
                return 0;
            })
            return {
                posts
            }
        },
        data() {
            return {
                activeFilter: "Alle projecten"
            }
        },
        computed: {
            filteredPosts() {
                if (this.activeFilter === '' || this.activeFilter === 'Alle projecten') {
                    return this.posts
                } else {
                    return this.posts.filter(e => {
                        return e.attributes.projecttype.includes(event.target.value)
                    })
                }
            }
        },
        mounted() {
            this.$root.$on('filterPosts', data => {
                this.activeFilter = data
            });
        }
    }
</script>
