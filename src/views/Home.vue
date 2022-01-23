<template>
  <div class="sections" ref="scrollArea">
    <Section
      v-for="(item, index) in getSectionData"
      :key="item.id"
      :title="item.title"
      :index="index"
      :items="item.items"
    />
    <div class="sections__add">
      <Button @click.native="addSection">
        + Add Section
      </Button>
    </div>
  </div>
</template>

<script>
import Section from '@/components/Section'
import Button from '@/components/Button'

export default {
  name: 'Home',
  components: {
    Button,
    Section
  },
  methods: {
    addSection () {
      const refSectionDataArr = this.getSectionData;
      refSectionDataArr.push({
        id: refSectionDataArr.length,
        title: "New Section",
        items: []
      });

      this.$store.commit('updateSectionData', [
        ...refSectionDataArr
      ])

      const scrollArea = document.querySelector('html');
      this.$nextTick(() => {
        scrollArea.scrollTo({
          left: scrollArea.scrollWidth,
          top: 0,
          behavior: 'smooth',
        });
      })
    }
  },
  computed: {
    getSectionData () {
      return this.$store.getters.getSectionData
    }
  }
}
</script>

<style lang="scss" scoped>
  .sections {
    display: flex;
    overflow: auto;
    width: fit-content;
    &__add {
      flex-shrink: 0;
      padding: 20px 15px 0;
      .button {
        font-size: 16px;
        color: #2c3e50;
      }
    }
  }
</style>
