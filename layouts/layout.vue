<template>
    <section class="layout">
        <Header :page-type="getPageType" />
        <div class="content">
            <nuxt />
        </div>
    </section>
</template>

<script>
import Header from '~/components/Header.vue'

    export default {
        components: {
            Header
        },
        computed: {
            getPageType() {
                return this.$nuxt._route.name
            }
        },
    }
</script>
