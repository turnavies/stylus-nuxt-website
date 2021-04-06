<template>
    <section class="projectFilter__wrapper">
        <label for="Alle projecten">
          <input id="Alle projecten" type="radio" name="catfilter" value="Alle projecten" checked @change="filterPosts('')" />
          <span>Alle projecten</span>
        </label>
        <label v-for="filter in catFilter" :key="filter" :for="filter">
          <input :id="filter" type="radio" name="catfilter" :value="filter" @change="filterPosts($event)"/>
          <span>{{filter}}</span>
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
                return Array.from(new Set([].concat.apply([], this.posts.map(e => e.attributes.projecttype))))
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
