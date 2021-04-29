<template>
  <div class="container">
    <list-scroller
      :itemsData="items"
      class="list"
    />
  </div>
</template>

<script>
import ListScroller from '@/listscroller'

export default {
  components: { ListScroller },
  data() {
    return {
      items: [],
    }
  },
  methods: { },
  created() { },
}
</script>