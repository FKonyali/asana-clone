<template>
  <div class="sections">
    <Section
      v-for="item in getSectionData"
      :key="item.id"
      :title="item.title"
      :id="item.id"
      :items="item.items"
    />
  </div>
</template>

<script>
import Section from '@/components/Section'

export default {
  name: 'Home',
  components: {
    Section
  },
  computed: {
    getSectionData () {
      return this.$store.getters.getSectionData
    }
  }
}
</script>

<style type="scss" scoped>
  .sections {
    display: flex;
    overflow: auto;
    width: fit-content;
  }
</style>
