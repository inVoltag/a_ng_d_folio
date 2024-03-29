<script lang="ts">
  import { defineComponent } from 'vue'
  import Footer from '@/components/patterns/Footer.vue'
  import Button from '@/components/ui/Button.vue'
  import ScrollingText from '@/components/ui/ScrollingText.vue'
  import { Mail } from 'lucide-vue-next'

  export default defineComponent({
    name: 'Contact',
    components: {
      Footer,
      Button,
      ScrollingText,
      Mail,
    },
    props: {
      theme: {
        type: String,
        default: 'DEFAULT',
      },
    },
  })
</script>

<template>
  <main class="page">
    <section class="jumbotron">
      <div class="jumbotron__content" :data-theme="theme">
        <Transition
          name="slide-up"
          style="--delay: var(--delay-turtoise)"
          appear
        >
          <ScrollingText :label="$t('contact.friendlyName')" :theme="theme" />
        </Transition>
        <Transition
          name="slide-up"
          style="
            --delay: calc(var(--delay-turtoise) + (var(--duration-step) * 0.5));
          "
          appear
        >
          <p class="jumbotron__baseline">{{ $t('contact.description') }}</p>
        </Transition>
      </div>
      <div class="jumbotron__actions">
        <Transition
          name="slide-up"
          style="
            --delay: calc(var(--delay-turtoise) + (var(--duration-step) * 1));
          "
          appear
        >
          <Button
            type="primary"
            :label="$t('contact.email.hello')"
            path="mailto:a.grimaud.hello@an.gd"
            layout="ICON-LEFT"
            :alt="$t('contact.email.hello')"
            :theme="theme"
          >
            <template #icon>
              <Mail :size="24" />
            </template>
          </Button>
        </Transition>
        <Transition
          name="slide-up"
          style="
            --delay: calc(var(--delay-turtoise) + (var(--duration-step) * 1.5));
          "
          appear
        >
          <Button
            type="primary"
            :label="$t('contact.email.pro')"
            path="mailto:a.grimaud.pro@an.gd"
            layout="ICON-LEFT"
            :alt="$t('contact.email.pro')"
            :theme="theme"
          >
            <template #icon>
              <Mail :size="24" />
            </template>
          </Button>
        </Transition>
      </div>
    </section>
    <Transition name="pull-up" style="--delay: var(--delay-turtoise)" appear>
      <Footer alignment="L" :theme="theme" />
    </Transition>
  </main>
</template>

<style scoped lang="sass">
  @use '@/assets/stylesheets/mixins' as device

  // Structure
  .page
    height: 100%
    overflow: hidden

  .jumbotron
    grid-area: main
    display: flex
    flex-flow: column nowrap
    justify-content: space-evenly
    padding: 0

    &[data-theme="DARK"]
      --text-color: var(--color-cream)

    &__content
      display: flex
      flex-flow: column nowrap
      align-items: center
      gap: var(--layout-row-gap) 0
      justify-content: center

    &__baseline
      padding: 0 var(--layout-center)

    &__actions
      display: flex
      justify-content: space-around
      gap: var(--spacing-xl-000)
      padding: 0 var(--layout-center)

  @include device.smartphone-landscape
    .jumbotron
      gap: var(--layout-paragraph-gap)

      &__baseline
        display: none

  @include device.smartphone
    .jumbotron
      &__actions
        flex-flow: column nowrap
        gap: var(--spacing-l-000)

        a
          width: 100%
</style>
