<template>
  <div class="section">
    <div class="section__top js-drag-area">
      <div class="section__top-left" @click="editSectionName">
        <div class="section__top-name" v-if="!isSectionNameEditAble">
          {{ title }}
        </div>
        <input
          v-else
          type="text" 
          class="section__input" 
          :value="title" 
          @blur="inputBlur(index, $event)" 
          @keypress="changeSectionName(index, $event)"
          ref="input"
        >
      </div>
      <div class="section__top-right">
        <Button className="section__top-add" @click.native="addNewTask(index)">
          <IconPlus width="12px" />
        </Button>
        <Button className="section__top-remove" @click.native="removeSection(index)">
          <IconTrash width="12px" />
        </Button>
      </div>
    </div>
    <div class="section__bot" ref="scrollArea">
      <div class="section__bot-group" v-if="items.length > 0">
        <draggable
          class="list-group"
          v-model="replicaItems"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
          tag="div"
          handle=".js-task-drag-area"
          group="items"
        >
          <transition-group type="transition" :name="!drag ? 'flip-list' : null">
            <Task 
              v-for="item in replicaItems"
              :key="item.id"
              :item="item"
            />
          </transition-group>
        </draggable>
        <div class="section__remaining js-drag-area"></div>
      </div>
      <div class="section__blank js-drag-area" v-else>
        <Button @click.native="addNewTask(index)">
          + Add Task
        </Button>
        <draggable
          class="list-group"
          v-model="replicaItems"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
          tag="div"
          handle=".js-task-drag-area"
          group="items"
        >
          <transition-group type="transition" :name="!drag ? 'flip-list' : null">
            <Task 
              v-for="(item) in replicaItems"
              :key="item.id"
              :item="item"
            />
          </transition-group>
        </draggable>
        <div class="section__remaining js-drag-area"></div>
      </div>
    </div>
  </div>
</template>

<script>
import IconTrash from "@/assets/img/icon/icon-trash.svg";
import IconPlus from "@/assets/img/icon/icon-plus.svg";
import draggable from 'vuedraggable'
import Button from "../Button";
import Task from "../Task";

export default {
  name: "Section",
  components: {
    Task,
    Button,
    IconPlus,
    draggable,
    IconTrash
  },
  data () {
    return {
      isSectionNameEditAble: false,
      drag: false,
      replicaItems: []
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
  mounted () {
    this.replicaItems = this.items
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
    inputBlur (getIndex, e) {
      if (e.target.value.length > 0) {
        const refSectionDataArr = this.getSectionData;
        refSectionDataArr[getIndex].title = e.target.value;
        this.$store.commit('updateSectionData', [
          ...refSectionDataArr
        ])
      }

      this.isSectionNameEditAble = false;
    },
    changeSectionName (getIndex, e) {
      if (e.keyCode === 13 && e.target.value.length > 0) {
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
    getSectionData: {
      get () {
        return this.$store.getters.getSectionData
      },
      set (value) {
        this.$store.commit('updateSectionData', value)
      }
    },
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },
  watch: {
    'replicaItems': function () {
      const getSectionIndex = this.index;
      const getReplicaItems = this.replicaItems;
      const refSectionDataArr = this.getSectionData;

      refSectionDataArr[getSectionIndex].items = getReplicaItems;

      this.$store.commit('updateSectionData', [
        ...refSectionDataArr
      ])
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
        max-width: calc(100% - 55px);
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
      &-group {
        display: flex;
        flex-direction: column;
        height: 100%;
      }
      .list-group {
        height: 100%;
        span {
          display: block;
          height: 100%;
        }
      }
    }
    &__remaining {
      flex: 1
    }
    &__blank {
      background: #ECEEF0;
      height: 100%;
      text-align: center;
      user-select: none;
      .button {
        padding: 15px;
        width: 100%;
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
