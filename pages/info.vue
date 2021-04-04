<template>
        <section class="info_blurb">
            <div v-html="data.description"></div>
            <div v-html="data.cta"></div>
            <ul>
                <li>
                    <p>
                        <a :href="`mailto:${data.contact.email}`">Email: {{ data.contact.email }}</a>
                    </p>
                </li>
            </ul>
        </section>
</template>

<script>
    export default {
        layout: 'layout',
        computed: {
            isInfoPage() {
                return this.$nuxt._route.name === "info" && true
            }
        },
        async asyncData() {
            try {
                const data = await import(`~/content/data/info.json`);
                return {
                data
                }
            } catch(err) {
                return false
            }
        },
        head() {
            return {
                bodyAttrs: {
                    style: this.isInfoPage && `background-color: ${this.data.background_color}; color: ${this.data.text_color}`
                }
            }
        }
    }
</script>
