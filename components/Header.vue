<template>
    <header class="header">
        <nav class="nav__wrapper">
            <header class="nav__header">
              <NuxtLink to="/">
                  <h1>{{ config.title[0] }}<br />{{ config.title[1] }}</h1>
              </NuxtLink>
            </header>
            <ProjectFilter v-if="pageType !== 'info'" />
            <footer class="nav__footer">
                <NuxtLink :to="infoRoute">{{ pageType === 'info' ? "close" : "info" }}</NuxtLink>
            </footer>
        </nav>

    </header>
</template>

<script>
    import ProjectFilter from '~/components/ProjectFilter.vue'

    export default {
        components: {
            ProjectFilter
        },
        data() {
          return {
            config: require("../content/data/config.json")
          }
        },
        computed: {
            pageType() {
                return this.$nuxt._route.name
            },
            infoRoute() {
                return this.pageType === 'info' ? "/" : "/info"
            }
        }
    }
</script>
