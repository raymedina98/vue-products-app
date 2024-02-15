<template>
  <div
    role="button"
    :class="`rounded-full w-[10px] h-[10px] ${statusClass}`"
    @click="toggleStatus"
  />
</template>

<script>
export default {
  props: {
    status: {
      type: String,
      required: true
    },
  },
  created() {
    this.originalStatus = this.status
  },
  data() {
    return {
      originalStatus: '',
    }
  },
  computed: {
    statusClass() {
      return {
        green: 'bg-lime-700',
        yellow: 'bg-amber-300',
        red: 'bg-red-600',
      }[this.status || 'red'];
    },
  },
  methods: {
    toggleStatus() {
      if (this.originalStatus === 'red') {
        this.$emit('toggle-status', 'green')
        this.originalStatus = 'green'
      } else {
        const newStatus = this.status === this.originalStatus ? 'red' : this.originalStatus
        this.$emit('toggle-status', newStatus)
      }
    }
  },
}
</script>