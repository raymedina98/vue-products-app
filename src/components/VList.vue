<template>
  <section>
    <header class="flex flex-col md:flex-row items-start space-y-3 md:space-y-0 md:space-x-2">
      <slot name="header" />
    </header>
  </section>
  <ul class="mt-4 divide-y-[1px]" v-if="items.length">
    <template
      :key="`list-item-${index + 1}`"
      v-for="(item, index) in items">
      <slot name="item" :index="index" :item="item" />
    </template>
  </ul>
  <div v-else class="mt-4 py-3 flex items-center justify-center">
    <span>No items found</span>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      default: () => ([]),
    },
  },
}
</script>