<template>
    <header class="header">
        <nav class="nav__wrapper">
            <header class="nav__header">
              <NuxtLink to="/">
                  <h1>{{ config.title }}</h1>
              </NuxtLink>
            </header>
            <ProjectFilter v-if="!isInfoPage"/>
            <footer class="nav__footer">
                <NuxtLink :to="infoRoute">{{ isInfoPage ? "close" : "info" }}</NuxtLink>
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
        props: {
            isInfoPage: {
                type: Boolean,
                required: false
            }
        },
        data() {
          return {
            config: require("../content/data/config.json")
          }
        },
        computed: {
            infoRoute() {
                return this.isInfoPage ? "/" : "/info"
            }
        }
    }
</script>
