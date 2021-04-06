// this is a dynamically created template

<template>
  <article class="project">
      <figure class="project__hero">
          <img :src="post.attributes.hero_image" :alt="post.attributes.title">
      </figure>
    <div class="project__info" >
      <h1>{{ post.attributes.title }}</h1>
      <ProjectTypeList :types="post.attributes.projecttype" />
      <p>{{ post.attributes.description }}</p>
      <dl>
        <div v-for="(value, name) in renderedProperties" :key="name">
          <dt>{{ name }}</dt>
          <dd>{{ value }}</dd>
        </div>
      </dl>
      <!-- <p>{{ post.attributes.jaar_uitvoering }}<span v-if="post.attributes.location != ''"> &mdash; {{ post.attributes.location }}</span></p> -->
    </div>
    <div class="project__body" v-html="post.html"></div>
    <div class="project__footer">
      <h2>Fotografie door {{ post.attributes.photographer }}</h2>
      <NuxtLink :to="`/projecten/${nextProjectPath}`">
        <svg xmlns="http://www.w3.org/2000/svg"  version="1.1" x="0px" y="0px" viewBox="0 0 26 26" enableBackground="new 0 0 26 26" >
          <path d="M23.021,12.294l-8.714-8.715l-1.414,1.414l7.007,7.008H2.687v2h17.213l-7.007,7.006l1.414,1.414l8.714-8.713  C23.411,13.317,23.411,12.685,23.021,12.294z"/>
        </svg>
      </NuxtLink>
    </div>
  </article>
</template>

<script>
  import ProjectTypeList from '~/components/ProjectTypeList.vue'
  import projectDefinition from '~/.forestry/front_matter/templates/project.yml'

  export default {
    components: {
      ProjectTypeList
    },
    layout: "layout",
    // get the slug as a param to import the correct md file
    async asyncData({ params }) {
      try {
        const currentPath = params.slug
        // get current post data
        const post = await import(`~/content/projects/${params.slug}.md`);
        // get all post data for next route
        const allPosts = await require.context("~/content/projects/", true, /\.md$/)
        const posts =  allPosts.keys().map((key) => {
          return allPosts(key)
        });
        const sortedPosts = posts.sort((a,b) => {
          const dateA = new Date(a.attributes.date);
          const dateB = new Date(b.attributes.date);
          if (dateA < dateB) {
            return 1;
          }
          if (dateA > dateB) {
            return -1;
          }
            return 0;
        })
        const sortedPaths = []
        sortedPosts.map(post => {
          // clean up the path - split by /
          let relPath = post.attributes._meta.resourcePath.split('/')
          // get the end of the path, remove '.md'
          relPath = relPath[relPath.length - 1].slice(0, -3)
          return sortedPaths.push(relPath)
        })
        return {
          sortedPaths,
          post,
          currentPath
        }
      } catch(err) {
        return false
      }
    },
    computed: {
      formattedDate() {
        return new Date(this.post.attributes.date).toDateString().slice(4)
      },
      nextProjectPath() {
        const firstProjectPath = this.sortedPaths[0]
        // if there's no 'next' path, return the first path
        const nextPath = isNull(this.sortedPaths[this.sortedPaths.indexOf(this.currentPath) + 1]) ? firstProjectPath : this.sortedPaths[this.sortedPaths.indexOf(this.currentPath) + 1]
        function isNull(item) {
          return item === null || item === undefined
        }
        return nextPath
      },
      renderedProperties() {
        // load properties as defined in CMS yml - specific to Forestry.io
        const properties = projectDefinition.fields.filter(obj => {
          return obj.name === 'properties'
        })[0].fields

        // load current project properties
        const projectProperties = this.post.attributes.properties
        for (const propName in projectProperties){
          if (projectProperties[propName] === "" || projectProperties[propName] === null) {
            delete projectProperties[propName]
          } else if (typeof projectProperties[propName] === 'number' && propName === 'budget') {
            projectProperties[propName] = projectProperties[propName].toLocaleString('fr-BE', { style: 'currency', currency: 'EUR', minimumFractionDigits: 0, maximumFractionDigits: 0 })
          }
        }

        const propertiesToRender = {}

        properties.map(obj => { return obj.name }).forEach((prop, i) => {
          propertiesToRender.[properties.map(p => { return p.label })[i]] = projectProperties[prop]
        });

        return propertiesToRender
      }
    }
  }
</script>
