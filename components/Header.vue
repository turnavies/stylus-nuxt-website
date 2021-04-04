<template>
    <header class="header">
        <nav class="nav">
            <NuxtLink to="/">
                <h1>{{ config.title }}</h1>
            </NuxtLink>
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
