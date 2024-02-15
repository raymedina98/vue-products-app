<template>
  <div>
    <header class="flex flex-row items-center py-3 relative">
      <slot name="header" />
      <ExpandMoreIcon
        class="absolute right-0"
        :class="['h-6 self-end w-6 transition-all delay-50 cursor-pointer', {'rotate-180': showContent}]"
        @click="toggleContent"
      />
    </header>
    <section
      ref="content"
      :class="['transition-[height] delay-50', {'h-0 overflow-hidden': !showContent}]">
      <slot name="content" />
    </section>
  </div>
</template>

<script>
import ExpandMoreIcon from './ExpandMoreIcon.vue';

export default {
  components: {
    ExpandMoreIcon,
  },
  data() {
    return {
      showContent: false,
    }
  },
  methods: {
    toggleContent() {
      this.showContent = !this.showContent
    },
  },
}
</script>