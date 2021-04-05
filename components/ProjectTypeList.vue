<template>
  <section class="projectTypeList">
    <ul>
      <li v-for="projecttype in types" :key="projecttype">{{ projecttype }}</li>
    </ul>
  </section>
</template>

<script>
    export default {
        props: {
          types: {
            type: Array,
            required: true
          }
        }
    }
</script>
