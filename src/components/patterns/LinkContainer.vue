<script lang="ts">
  import { defineComponent } from 'vue'
  import Button from '@/components/ui/Button.vue'
  import Container from '@/components/ui/Container.vue'

  export default defineComponent({
    name: 'LinkContainer',
    components: {
      Button,
      Container,
    },
    props: {
      description: String,
      cta: String,
      href: String,
      alt: String,
      theme: {
        type: String,
        default: 'DEFAULT',
      },
    },
  })
</script>

<template>
  <Container>
    <div class="link-container__content" :data-theme="theme">
      <div class="link-container__icon">
        <slot name="icon"></slot>
      </div>
      <div class="link-container__description">
        <p>{{ description }}</p>
      </div>
      <div class="link-container__cta">
        <Button
          type="secondary"
          :path="href"
          :label="cta"
          layout="SIMPLE"
          :alt="alt"
          theme="DEFAULT"
        />
      </div>
    </div>
  </Container>
</template>

<style scoped lang="sass">
  @use '@/assets/stylesheets/mixins' as device

  // Structure
  .link-container
    &__content
      display: flex
      gap: var(--layout-row-gap) var(--layout-column-gap)

    &__icon
      padding: var(--spacing-s-000) 0

      :deep(svg)
        width: var(--icon-size-large)
        height: var(--icon-size-large)

    &__description
      flex: 1
      display: flex
      align-items: center

  @include device.tablet-portrait
    .link-container
      &__content
        flex-flow: column nowrap
        align-items: center

  @include device.smartphone
    .link-container
      &__content
        flex-flow: column nowrap
        align-items: center

  // Aspect
  .link-container__content
    &[data-theme="DARK"]
      --text-color: var(--color-soil)
</style>
