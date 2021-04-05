<template>
    <section>
        <label for="Alle projecten">Alle projecten
          <input id="Alle projecten" type="radio" name="catfilter" value="Alle projecten" @change="filterPosts('')"/>
        </label>
        <label v-for="filter in catFilter" :key="filter" :for="filter">{{filter}}
          <input :id="filter" type="radio" name="catfilter" :value="filter" @change="filterPosts($event)"/>
        </label>
    </section>
</template>

<script>
    export default {
        data() {
          return {
            posts: []
          }
        },
        async fetch() {
            // create context via webpack to map over all project posts
            const allPosts = await require.context("~/content/projects/", true, /\.md$/)
            this.posts =  allPosts.keys().map((key) => {
              // give back the value of each post context
              return allPosts(key)
            });
        },
        computed: {
            catFilter() {
                return Array.from(new Set(Array.from(new Set(this.posts.map(e => e.attributes.projecttype))).flat()))
            }
        },
        methods: {
          filterPosts(event) {
            if (event === '') {
              this.$root.$emit('filterPosts', '')
            } else {
              this.$root.$emit('filterPosts', event.target.value)
            }
          }
        }
    }
</script>
