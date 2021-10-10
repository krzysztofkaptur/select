<template>
  <div class="multiselect">
    <div class="multiselect__placeholder" @click="openDropdown">
      <span
        class="multiselect__label"
        :class="[placeholder.length && 'multiselect__label--active']"
      >
        {{ filter.mainLabel.label }}
      </span>
      <span class="multiselect__selected" v-if="placeholder.length">
        {{ placeholder }}
      </span>
      <button class="multiselect__arrow">
        <div class="multiselect__arrow-icon"></div>
      </button>
    </div>
    <button
      class="multiselect__close"
      @click="clearChosen"
      v-if="chosenOptions.length && !isDropdownOpened"
    >
      <div class="multiselect__close-icon">X</div>
    </button>
    <div class="multiselect__options" v-if="isDropdownOpened">
      <VueCustomScrollbar class="scroll-area" :settings="settings">
        <div
          v-for="(option, index) in filter.options"
          @click="chooseOption(option)"
          class="multiselect__option"
          :class="[
            chosenOptions.some(item => item.value === option.value) &&
              'multiselect__option--active'
          ]"
        >
          <span :data-value="option.value" :key="index">
            {{ option.label }}
          </span>
        </div>
      </VueCustomScrollbar>
    </div>
  </div>
</template>

<script>
import VueCustomScrollbar from 'vue-custom-scrollbar'
import 'vue-custom-scrollbar/dist/vueScrollbar.css'

export default {
  components: {
    VueCustomScrollbar
  },
  props: {
    filter: {
      type: Object,
      default: () => {}
    },
    chosenOptions: {
      type: Array,
      default: () => []
    },
    isSingle: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      isDropdownOpened: false,
      settings: {
        suppressScrollY: false,
        suppressScrollX: false,
        wheelPropagation: false
      }
    }
  },
  computed: {
    placeholder() {
      return this.isSingle
        ? this.chosenOptions.length && this.chosenOptions[0].label
        : this.chosenOptions.length && `Wybrano: ${this.chosenOptions.length}`
    }
  },
  methods: {
    openDropdown() {
      this.isDropdownOpened = !this.isDropdownOpened
    },
    chooseOption(option) {
      this.$emit('select', {
        mainLabel: this.filter.mainLabel,
        option,
        isSingle: this.filter.isSingle
      })
    },
    clearChosen() {
      this.$emit('clear', this.filter.mainLabel)
    },
    isActive(value) {
      return true
    }
  }
}
</script>

<style lang="scss">
.multiselect {
  max-width: 350px;
  width: 100%;
  position: relative;

  &__label {
    position: relative;
    transform: translateY(1px);
    transition: transform 0.2s ease-in-out;

    &--active {
      transform: translateY(-20px);
    }
  }

  &__options {
    position: absolute;
    left: 0;
    right: 0;
    top: 100%;
    border: 1px solid #ddd;
    border-radius: 0 0 5px 5px;
    background-color: white;
    z-index: 100;
  }

  &__option {
    &--active {
      background-color: red;
      color: white;
    }
  }

  &__placeholder {
    display: flex;
    align-items: center;
    min-height: calc(1.5em + 0.75rem + 2px);
    position: relative;
    border: 1px solid #ddd;
    padding-left: 10px;
    padding-right: 40px;
  }

  &__arrow {
    border: none;
    position: absolute;
    background-color: transparent;
    right: 0;

    &-icon {
      width: 0;
      height: 0;
      border-left: 5px solid transparent;
      border-right: 5px solid transparent;
      border-top: 5px solid #f00;
    }
  }

  &__close {
    border: none;
    position: absolute;
    background-color: transparent;
    right: 20px;
    z-index: 50;
    padding: 0;
    width: 20px;
    height: 20px;
    top: calc(50% - 10px);
  }

  &__selected {
    position: absolute;
    left: 10px;
    right: 40px;
    max-width: 100%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: left;
  }

  .scroll-area {
    max-height: 100px;
    overflow: hidden;
  }
}
</style>
