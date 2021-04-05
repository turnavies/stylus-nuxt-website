<template>
    <section>
        <ul class="list">
            <NuxtLink
                v-for="post in sortedPosts"
                :key="post.attributes.title"
                :to="`/projects/${formatSlug(post.attributes.title)}`"
            >
                <li>
                    <div class="hero_image">
                        <img :src="post.attributes.hero_image" :alt="post.attributes.title">
                    </div>
                    <div class="projectList__info">
                        <h2>{{ post.attributes.title }}</h2>
                        <p class="projectList__subtitle">{{ post.attributes.properties['Jaar van uitvoering'] }}</p>
                        <ProjectTypeList :types="post.attributes.projecttype" />
                        <p>{{ post.attributes.description }}</p>
                    </div>
                </li>
            </NuxtLink>
        </ul>
    </section>
</template>
<script>
    import ProjectTypeList from '~/components/ProjectTypeList.vue'

    export default {
        components: {
            ProjectTypeList
        },
        props: {
            posts: {
                type: Array,
                required: true
            }
        },
        data() {
          return {
            filteredPosts: this.posts
          }
        },
        computed: {
            sortedPosts() {
                const sortedPosts = this.filteredPosts
                sortedPosts.sort((a,b) => {
                    const dateA = new Date(a.attributes.jaar_uitvoering);
                    const dateB = new Date(b.attributes.jaar_uitvoering);
                    if (dateA < dateB) {
                        return 1;
                    }
                    if (dateA > dateB) {
                        return -1;
                    }
                    return 0;
                })
                return sortedPosts
            }
        },
        mounted() {
            this.$root.$on('filterPosts', data => {
                this.filterPosts(data);
            });
        },
        methods: {
            formatDate(date) {
                return new Date(date).toDateString().slice(4)
            },
            formatExcerpt(body) {
                return body.slice(0 , 200).trimEnd()
            },
            formatSlug(title) {
                const regex = / /gi;
                return title.toLowerCase().trim().replace(regex, "-")
            },
            filterPosts(filter) {
                if (filter === '') {
                  this.filteredPosts = this.posts
                } else {
                  this.filteredPosts = this.posts.filter(e => {
                    return e.attributes.projecttype.includes(event.target.value)
                  })
                }
            }
        }
    }
</script>

/*
TODO -- i would love to figure out how to show the md in the summary...
right now its just plaintext not sure how to target the loader to parse this
 */
