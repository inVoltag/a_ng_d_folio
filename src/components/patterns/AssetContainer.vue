<script lang="ts">
  import { defineComponent } from 'vue'
  import { store } from '@/utilities/store'
  import Button from '@/components/ui/Button.vue'
  import Container from '@/components/ui/Container.vue'
  import VLazyImage from 'v-lazy-image'
  import { Dribbble, Codepen } from 'lucide-vue-next'

  export default defineComponent({
    name: 'AssetContainer',
    components: {
      Button,
      Container,
      VLazyImage,
      Dribbble,
      Codepen,
    },
    props: {
      title: String,
      thumbnail: String,
      hdnail: String,
      alt: String,
      type: String,
      sourceName: String,
      sourceLink: String,
      unmagnify: {
        type: Boolean,
        default: false,
      },
      theme: {
        type: String,
        default: 'DEFAULT',
      },
    },
    data: function () {
      return {
        store,
        isMagnified: false as boolean,
        assetWidth: 0 as unknown,
      }
    },
    methods: {
      magnifier(e: Event) {
        let width: number

        if (e.target != null) {
          width = (
            (
              ((e.target as HTMLElement).children[0] as HTMLElement)
                .children[0] as HTMLElement
            ).children[0] as HTMLElement
          ).offsetWidth
          this.isMagnified = !this.isMagnified
          this.assetWidth = width + 16 + 'px'
        }
      },
    },
    watch: {
      unmagnify(to) {
        to == true ? (this.isMagnified = false) : this.isMagnified
      },
    },
  })
</script>

<template>
  <Container
    @click="magnifier"
    @keyup.enter="magnifier"
    @focus="store.isFocus = true"
    @blur="store.isFocus = false"
    class="container card"
    :class="isMagnified ? 'container--magnified' : ''"
    isInteractive
  >
    <div class="asset-container__content" :data-theme="theme">
      <div class="asset-container__asset">
        <Transition
          name="fade"
          mode="in-out"
          :style="`--delay: ${isMagnified ? '0ms' : 'var(--delay-jogging)'}`"
        >
          <v-lazy-image v-if="!isMagnified" :src="thumbnail" :alt="alt" />
          <video
            v-else-if="isMagnified && type === 'video'"
            preload="true"
            autoplay
            muted
            loop
            playsinline
            poster=""
          >
            <source :src="hdnail" type="video/mp4" />
          </video>
          <img
            v-else-if="isMagnified && type === 'image'"
            :src="hdnail"
            :alt="alt"
          />
        </Transition>
      </div>
      <div class="asset-container__description">
        <h5>{{ title }}</h5>
        <Transition
          name="switch"
          mode="out-in"
          style="--delay: var(--delay-hare)"
        >
          <Button
            v-if="isMagnified"
            type="secondary"
            :path="sourceLink"
            :alt="alt"
            layout="ICON-ONLY"
            theme="DEFAULT"
          >
            <template #icon>
              <Component :is="sourceName" :size="24" />
            </template>
          </Button>
        </Transition>
      </div>
    </div>
  </Container>
</template>

<style scoped lang="sass">
  @use '@/assets/stylesheets/mixins' as device

  // Structure
  .container
    width: v-bind("isMagnified ? assetWidth : '30vh'")
    padding: var(--spacing-s-100)
    // cursor: zoom-in

    &--magnified
      // cursor: zoom-out

  .asset-container
    &__content
      --container-padding: var(--spacing-s-100)

      display: flex
      flex-flow: column nowrap
      gap: var(--container-padding) 0
      height: 100%
      pointer-events: none

    &__asset
      --container-padding: var(--spacing-s-100)

      flex: 1
      display: flex
      justify-content: center
      overflow: hidden
      -webkit-mask-image: -webkit-radial-gradient(white, black)
      border-radius: var(--asset-radius)
      transition: var(--simple-transition)
      animation: loading var(--duration-turtoise) infinite linear
      box-shadow: var(--inner-border)

      img, video
        height: 100%
        filter: v-bind("isMagnified ? 'brightness(1)' : 'brightness(.5)'")
        border-radius: var(--asset-radius)
        transition: var(--slow-transition)

        &.v-lazy-image
          opacity: 0

        &.v-lazy-image.v-lazy-image-loaded
          opacity: 1

    &__description
      display: flex
      flex: 0 0 var(--sizing-xl-300)
      flex-flow: row nowrap
      align-items: center
      padding: 0 var(--spacing-m-000)

      & > h5
        flex: 1
        white-space: nowrap
        overflow: hidden
        text-overflow: ellipsis
        padding: var(--spacing-s-000) 0

      & > a.button
        pointer-events: auto

  // Aspect
  .asset-container__content
    --color-1: var(--color-creamy-sun)
    --color-2: var(--color-soft-wind)
    --color-3: var(--color-candy-floss)
    --asset-radius: calc(var(--regular-border-radius) - var(--container-padding))

    &[data-theme="DARK"]
      --text-color: var(--color-soil)
</style>
