<template>
  <li>
    <VCollapse>
      <template #header>
        <div class="flex flex-row items-center w-11/12">
          <div class="flex flex-row items-center space-x-2 flex-1">
            <img v-if="item.image" :src="item.image" :alt="item.name" class="rounded-sm w-6 h-6">
            <div v-else :title="item.name" class="rounded-sm w-6 h-6 border bg-zinc-400"></div>
            <VStatus :status="item.status" @toggle-status="$emit('toggle-status', $event)"/>
            <span>{{ item.name }}</span>
          </div>
          <span>{{ formatedDate }}</span>
        </div>
      </template>
      <template #content>
        <div class="space-y-4 py-3" ref="content">
          <div class="flex flex-row items-center justify-between">
            <div role="button" class="h-36 flex items-center justify-center border border-dashed rounded-md flex-[.9] cursor-pointer group hover:bg-slate-50 hover:border-zinc-600">
              <span class="text-zinc-400 group-hover:text-zinc-600">Upload Photo</span>
            </div>
            <div class="flex flex-col space-y-2">
              <button class="btn btn-primary" @click="replaceImage(imageCode)">
                <span>Replace</span>
              </button>
              <button class="btn btn-clear" @click="replaceImage('')">
                <span>Clear</span>
              </button>
            </div>
          </div>
          <div class="flex flex-col space-y-1">
            <label class="font-bold">Image Title</label>
            <input type="text" class="input" placeholder="Image title">
          </div>
          <div class="flex flex-col space-y-1">
            <label class="font-bold">Alt Text</label>
            <input type="text" class="input" placeholder="Alt Text">
          </div>
          <div class="flex flex-col space-y-1">
            <label class="font-bold">Link</label>
            <input type="text" class="input" placeholder="Link">
          </div>
          <div v-if="!item.image" class="flex flex-col space-y-1">
            <label class="font-bold">Code</label>
            <input type="text" class="input" placeholder="Code" v-model="imageCode">
          </div>
        </div>
      </template>
    </VCollapse>
  </li>
</template>

<script>
import VCollapse from './VCollapse.vue';
import VStatus from './VStatus.vue';

export default {
  components: {
    VCollapse,
    VStatus,
  },
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      showImgCodeInput: false,
      imageCode: '',
    }
  },
  computed: {
    formatedDate() {
      if (this.item.date) {
        return new Date(this.item.date).toLocaleDateString('en-US')
      }
      return 'No date'
    },
  },
  methods: {
    replaceImage($event) {
      this.$emit('replace-image', $event)
      this.imageCode = ''
    },
  },
}
</script>