<template>
  <div class="task">
    <div class="task__top">
      <div class="task__top-left">
        <div 
          class="task__top-color"
          :style="`${item.color ? 'background: ' + item.color : ''}`"
        >
        </div>
      </div>
      <div class="task__top-right">
        <div class="task__top-edit"></div>
      </div>
    </div>
    <div class="task__mid">
      <div class="task__title">
        {{ item.title }}
      </div>
    </div>
    <div class="task__bot">
      <div class="task__bot-date">
        {{ item.date }}
      </div>
      <div class="task__bot-badge">
        <div class="task__bot-badge-count">
          {{ item.count }}
        </div>
        <div class="task__bot-badge-icon">
          icon
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    item: {
      type: Object,
      default: null
    }
  }
}
</script>

<style lang="scss" scoped>
  .task {
    border: 1px solid #e4e9eb;
    border-radius: 6px;
    padding: 15px 10px;
    background: #FFF;
    box-shadow: 0px 1px 5px 0px rgba(0, 0, 0, .1);
    margin-bottom: 10px;
    &__top {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 20px;
      &-color {
        background: #C095C0;
        width: 40px;
        height: 7px;
        border-radius: 3px;
      }
    }
    &__mid {
      margin-bottom: 16px;
    }
    &__title {
      font-weight: 500;
      font-size: 16px;
      color: #000;
    }
    &__bot {
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: 12px;
      &-badge {
        display: flex;
        align-items: center;
        &-count {
          margin-right: 3px;
        }
      }
    }
  }
</style>
