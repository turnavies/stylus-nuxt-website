<template>
    <section class="projectFilter__wrapper">
        <label for="Alle projecten">
          <input id="Alle projecten" v-model="selectedFilter" type="radio" name="catfilter" value="Alle projecten" @change="filterPosts($event)" />
          <span>Alle projecten</span>
        </label>
        <label v-for="filter in catFilter" :key="filter" :for="filter">
          <input :id="filter" v-model="selectedFilter" type="radio" name="catfilter" :value="filter" @change="filterPosts($event)"/>
          <span>{{filter}}</span>
        </label>
    </section>
</template>

<script>
    export default {
        data() {
            return {
                selectedFilter: "Alle projecten",
                posts: []
            }
        },
        async fetch() {
            // create context via webpack to map over all project posts
            const allPosts = await require.context("~/content/projects/", true, /\.md$/)
            this.posts = allPosts.keys().map((key) => {
                // give back the value of each post context
                return allPosts(key)
            })
        },
        computed: {
            catFilter() {
                return Array.from(new Set([].concat.apply([], this.posts.map(e => e.attributes.projecttype))))
            },
            pageType() {
                return this.$nuxt._route.name
            }
        },
        methods: {
            filterPosts(event) {
                this.$root.$emit('filterPosts', event.target.value)
                if (this.pageType !== 'index') {
                    this.$router.push({path: '/'})
                }
            }
        }
    }
</script>
