<template>
  <div class="section">
    <div class="section__top">
      <div class="section__top-left">
        <div class="section__top-name">
          {{ title }}
        </div>
      </div>
      <div class="section__top-right">
        <Button className="section__top-add" @click.native="addNewTask(id)">
          +
        </Button>
        <Button className="section__top-add" @click.native="removeSection(id)">
          -
        </Button>
      </div>
    </div>
    <div class="section__bot" ref="scrollArea">
      <Task 
        :item="item"
        v-for="item in items"
        :key="item.id"
      />
    </div>
  </div>
</template>

<script>
import Button from "../Button";
import Task from "../Task";
import IconPlus from "@/assets/img/icon/icon-plus.svg";
console.log(IconPlus);

export default {
  name: "Section",
  components: {
    Task,
    Button,
    IconPlus
  },
  props: {
    id: {
      type: Number,
      default: null
    },
    title: {
      type: String,
      default: null
    },
    items: {
      type: Array,
      default: null
    }
  },
  methods: {
    addNewTask (getId) {
      const refSectionDataArr = this.getSectionData;
      const getItems = refSectionDataArr[getId].items;
      getItems.push({
        id: getItems.length,
        title: "New Task Item",
        date: "24 Şub",
        count: 0
      });

      this.$store.commit('updateSectionData', [
        ...refSectionDataArr
      ])

      const scrollAreaRef = this.$refs.scrollArea;
      this.$nextTick(() => {
        scrollAreaRef.scrollTo({
          top: scrollAreaRef.scrollHeight,
          behavior: 'smooth',
        });
      })
    },
    removeSection (getId ) {
      const refSectionDataArr = this.getSectionData;
      refSectionDataArr.splice(getId, 1);
      console.log(refSectionDataArr);

      this.$store.commit('updateSectionData', [
        ...refSectionDataArr
      ])
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
  .section {
    width: 304px;
    height: 100vh;
    user-select: none;
    display: flex;
    flex-direction: column;
    &__top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 15px 0;
    }
    &__bot {
      margin-top: 20px;
      overflow-y: auto;
      padding: 0 15px;
    }
  }
</style>
