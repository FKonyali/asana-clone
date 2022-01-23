<template>
  <div class="sections" ref="scrollArea">
    <draggable
      class="list-group"
      v-model="getSectionData"
      v-bind="dragOptions"
      @start="drag = true"
      @end="drag = false"
      tag="div"
    >
      <transition-group type="transition" :name="!drag ? 'flip-list' : null">
        <Section
          v-for="(item, index) in getSectionData"
          :key="item.id"
          :title="item.title"
          :index="index"
          :items="item.items"
        />
      </transition-group>
    </draggable>
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
import draggable from 'vuedraggable'

export default {
  name: 'Home',
  components: {
    Button,
    Section,
    draggable
  },
  data() {
    return {
      drag: false
    };
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
    },
    sort() {
      this.list = this.list.sort((a, b) => a.order - b.order);
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
  }
}
</script>

<style lang="scss" scoped>
  .sections {
    display: flex;
    width: fit-content;
    &__add {
      flex-shrink: 0;
      padding: 20px 15px 0;
      .button {
        font-size: 16px;
        color: #2c3e50;
      }
    }
    .list-group {
      span {
        display: flex;
      }
    }
  }

  .flip-list-move {
    transition: transform 0.5s;
  }
  .no-move {
    transition: transform 0s;
  }
  .ghost {
    opacity: 0.5;
    background: #c8ebfb;
  }
  .list-group {
    min-height: 20px;
  }
  .list-group-item {
    cursor: move;
  }
</style>
