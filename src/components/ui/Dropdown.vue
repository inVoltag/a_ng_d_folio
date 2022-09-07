<script lang="ts">
  import { defineComponent } from 'vue'
  import Button from '@/components/ui/Button.vue'
  import Container from '@/components/ui/Container.vue'
  import Label from '@/components/ui/Label.vue'
  import { doMap } from '@/utilities/operations'
  import { ChevronDown, Check } from 'lucide-vue-next'

  export default defineComponent({
    name: 'Dropdown',
    components: {
      Button,
      Container,
      Label,
      ChevronDown,
      Check
    },
    props: {
      label: String,

      options: {
        type: Array,
        default: []
      },
      theme: {
        type: String,
        default: 'DEFAULT'
      }
    },
    data: function() {
      return {
        isExpanded: false,
        activeOption: 0 as number,
        allOptions: this.options as Array<{name: string, action: any, isActive: boolean}>
      }
    },
    watch: {
      isExpanded(to, from) {
        if (to == false) window.removeEventListener("click",  this.closeOptions)
      }
    },
    methods: {
      setOption(callback: any, name: string, order: number) {
        (this.allOptions[(this.activeOption) as number] as any).isActive = false;
        setTimeout(() =>(this.activeOption as number) = order, 900);
        (this.allOptions[order] as any).isActive = true;
        this.isExpanded = false;
        callback?.()
      },
      openOptions(e: any) {
        this.isExpanded = true
        setTimeout(() => window.addEventListener("click",  this.closeOptions), 200)
      },
      closeOptions(e: any) {
        if (e.target.closest('.dropdown__list') == null) this.isExpanded = false
      },
      map(current: number) {
        return doMap(current, 0, this.allOptions.length - 1, this.allOptions.length - 1, 0)
      }
    },
    created: function() {
      this.activeOption = this.allOptions.findIndex(option => {
        if (option.isActive == true)
          return true
      })
    }
  })
</script>

<template>
  <div class="dropdown" @keydown.esc="isExpanded = false" :data-theme="theme">
    <Label
      v-if="label != undefined"
      :label="label"
    />
    <Button
      type="secondary"
      :label="allOptions[activeOption].name"
      layout="ICON-RIGHT"
      :theme="theme"
      @click="openOptions"
      @keyup.space="openOptions"
      extensible
    >
      <template #icon>
        <ChevronDown :size="24" />
      </template>
    </Button>
    <Transition name="switch" style="--delay: 0ms">
      <Container
        class="dropdown__list"
        v-if="isExpanded"
      >
        <ul class="dropdown__options">
          <li
            v-for="(option, index) in allOptions"
            :key="option.name"
            class="dropdown__option"
            :class="option.isActive ? 'dropdown__option--active' : ''"
            @click="setOption(option.action, option.name, index)"
            @keyup.space="setOption(option.action, option.name, index)"
            tabindex="0"
          >
            <Transition name="switch">
              <i v-if="option.isActive" class="dropdown__option__icon"><Check :size="24" /></i>
            </Transition>
            <Label
              class="dropdown__option__label"
              :label="option.name"
            />
          </li>
        </ul>
      </Container>
    </Transition>
  </div>
</template>

<style scoped lang="sass">
  // Structure
  .dropdown
    display: flex
    flex-flow: column nowrap
    gap: var(--layout-label-gap) 0

    &__list
      padding: var(--spacing-m-200)
      min-width: 100%
      min-height: 100%
      position: absolute
      bottom: calc((var(--spacing-m-200) + var(--sizing-xs-000) + (v-bind("map(activeOption)") * var(--button-height-size))) * -1)
      z-index: 5

    &__options
      padding: 0
      display: flex
      flex-flow: column nowrap
      gap: 0 var(--layout-paragraph-gap)

    &__option
      flex: 0 1 var(--button-height-size)
      display: flex
      align-items: center
      padding: 0 var(--spacing-l-000) 0 var(--button-height-size)
      cursor: pointer
      border-radius: calc(var(--button-height-size) / 2)
      transition: var(--simple-transition)
      box-shadow: 0 0 0 var(--spacing-xs-000) var(--item-border)
      background-color: var(--item-background)

      &--active
        padding: 0 var(--spacing-l-000) 0 0
        pointer-events: none

      &__icon
        display: flex
        flex: 0 0 var(--button-height-size)
        justify-content: center

  // Aspect
  .dropdown
    &__option
      --item-border: transparent
      --item-background: transparent

  // Event
  .dropdown
    &__option:hover
      --amplitude: var(--spacing-s-000)
      --item-background: var(--color-soft-wind)

      animation: excited var(--duration-running) var(--ease-peps)

    &__option:focus
      --item-background: var(--color-soft-wind)
      --item-border: var(--color-clear-water)

      transform: var(--focus-scale)
      outline: none
      z-index: 2

    &__option:active
      transform: var(--active-scale)
</style>