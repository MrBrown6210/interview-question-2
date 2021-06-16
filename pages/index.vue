<template>
  <div class="p-5">
    <div class="w-screen flex items-center align-middle mb-4">
      <input type="text" class="w-full border-2 border-black rounded-lg" v-model="filtered" @input="filter">
    </div>
    <table class="table-auto border-2 border-black">
      <thead>
        <tr class="border-2 border-black">
          <th>Title</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in filteredList" :key="item">
          <td :class="{ 'bg-green-300': index % 2 === 1 }" >{{ item }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, Ref, ref, toRef, toRefs } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const list: Ref<string[]> = ref([])
    const filteredList: Ref<string[]> = ref([])
    const filtered = ''
    return { list, filteredList, filtered }
  },
  mounted() {
    this.load()
  },
  methods: {
    async load() {
      const list = await this.$axios.$get('https://api.publicapis.org/categories?search')
      this.list = list
      this.filter()
    },
    filter() {
      this.filteredList = this.list.filter(item => {
        if (this.filtered === '') return true
        if (item.includes(this.filtered)) return true
        return false
      })
    }
  }
})
</script>
