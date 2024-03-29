<script lang="ts">
  import { defineComponent } from 'vue'

  export default defineComponent({
    name: 'TwoColumns',
    props: {
      title: String,
      center: Boolean,
      layout: {
        type: String,
        default: '1_1',
      },
      theme: {
        type: String,
        default: 'DEFAULT',
      },
    },
  })
</script>

<template>
  <div class="col-2" :data-theme="theme">
    <template v-if="title != undefined">
      <h3>{{ title }}</h3>
    </template>
    <div class="col-2__columns">
      <div class="col-2__column">
        <slot name="left"></slot>
      </div>
      <div class="col-2__column">
        <slot name="right"></slot>
      </div>
    </div>
  </div>
</template>

<style scoped lang="sass">
  @use '@/assets/stylesheets/mixins' as device

  // Structure
  .col-2
    display: flex
    flex-flow: column nowrap
    padding: 0 var(--layout-center)
    gap: var(--layout-title-gap)

    &__columns
      display: flex
      gap: var(--layout-row-gap) var(--layout-column-gap)

    &__column
      display: flex
      flex-flow: column nowrap
      flex: 1
      gap: var(--layout-row-gap) 0
      justify-content: v-bind("center ? 'center' : 'flex-start'")

      &:first-child
        flex: v-bind("layout === '2_1' ? '2' : '1'")

      &:last-child
        flex: v-bind("layout === '1_2' ? '2' : '1'")

  @include device.smartphone
    .col-2
      &__columns
        flex-flow: column nowrap

      &--invert
        .col-2__column:nth-child(1)
          order: 2

        .col-2__column:nth-child(2)
          order: 1

  // Aspect`
  .col-2
    &[data-theme="DARK"]
      --text-color: var(--color-cream)
</style>
