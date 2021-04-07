<template>
    <section>
        <ul class="list">
            <NuxtLink
                v-for="post in posts"
                :key="post.attributes.title"
                :to="`/projecten/${formatSlug(post.attributes.title)}`"
            >
                <li>
                    <div class="hero_image">
                        <img :src="post.attributes.hero_image" :alt="post.attributes.title">
                    </div>
                    <div class="projectList__info">
                        <h2>{{ post.attributes.title }}</h2>
                        <p class="projectList__subtitle">{{ post.attributes.properties.year }}</p>
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
            }
        }
    }
</script>

/*
TODO -- i would love to figure out how to show the md in the summary...
right now its just plaintext not sure how to target the loader to parse this
 */
