<template>
  <div class="section">
    <div class="section__top">
      <div class="section__top-left" @click="editSectionName">
        <div class="section__top-name" v-if="!isSectionNameEditAble">
          {{ title }}
        </div>
        <input
          v-else
          type="text" 
          class="section__input" 
          :value="title" 
          @blur="inputBlur" 
          @keypress="changeSectionName(index, $event)"
          ref="input"
        >
      </div>
      <div class="section__top-right">
        <Button className="section__top-add" @click.native="addNewTask(index)">
          +
        </Button>
        <Button className="section__top-remove" @click.native="removeSection(index)">
          -
        </Button>
      </div>
    </div>
    <div class="section__bot" ref="scrollArea">
      <div v-if="items">
        <Task 
          v-for="item in items"
          :key="item.id"
          :item="item"
        />
      </div>
      <div class="section__blank" v-else>
        <Button>
          + Add Task
        </Button>
      </div>
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
  data () {
    return {
      isSectionNameEditAble: false
    }
  },
  props: {
    title: {
      type: String,
      default: null
    },
    index: {
      type: Number,
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
      if (refSectionDataArr[getId].items === undefined) refSectionDataArr[getId].items = []
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
    removeSection (getIndex) {
      const refSectionDataArr = this.getSectionData;
      refSectionDataArr.splice(getIndex, 1);

      this.$store.commit('updateSectionData', [
        ...refSectionDataArr
      ])
    },
    editSectionName () {
      this.isSectionNameEditAble = true;
      this.$nextTick(() => {
        this.$refs.input.focus();
      })
    },
    inputBlur () {
      this.isSectionNameEditAble = false;
    },
    changeSectionName (getIndex, e) {
      console.log(getIndex)
      console.log(e)
      if (e.keyCode === 13) {
        const refSectionDataArr = this.getSectionData;
        refSectionDataArr[getIndex].title = e.target.value;
        this.$store.commit('updateSectionData', [
          ...refSectionDataArr
        ])

        this.isSectionNameEditAble = false;
      } 
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
    flex-shrink: 0;
    &__top {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 15px 0;
      &-left {
        flex: 1;
        padding-right: 15px;
        max-width: calc(100% - 40px);
      }
      &-name {
        padding: 5px 0;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }
    }
    &__bot {
      margin-top: 20px;
      overflow-y: auto;
      padding: 0 15px;
      flex: 1;
    }
    &__blank {
      background: #ECEEF0;
      height: 100%;
      text-align: center;
      user-select: none;
      .button {
        padding: 15px;
      }
    }
    &__input {
      border-radius: 6px;
      background: #FFF;
      padding: 5px;
      width: 100%;
      font-size: 16px;
      color: #2c3e50
    }
  }
</style>
