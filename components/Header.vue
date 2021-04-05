<template>
    <header class="header">
        <nav class="nav">
            <NuxtLink to="/">
                <h1>{{ config.title }}</h1>
            </NuxtLink>
            <label for="Alle projecten">Alle projecten
              <input id="Alle projecten" type="radio" name="catfilter" value="Alle projecten" @change="filterPosts('')"/>
            </label>
            <label v-for="filter in catFilter" :key="filter" :for="filter">{{filter}}
              <input :id="filter" type="radio" name="catfilter" :value="filter" @change="filterPosts($event)"/>
            </label>
            <div>
                <h1>
                    <NuxtLink :to="infoRoute">{{ isInfoPage ? "close" : "info" }}</NuxtLink>
                </h1>
            </div>
        </nav>

    </header>
</template>

<script>
    export default {
        props: {
            isInfoPage: {
                type: Boolean,
                required: false
            }
        },
        data() {
          return {
            config: require("../content/data/config.json"),
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
            infoRoute() {
                return this.isInfoPage ? "/" : "/info"
            },
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
