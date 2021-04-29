<template>
  <div ref="list" :style="listStyle">
    <div ref="spacer" :style="spacerStyle">
      展示组件
    </div>
  </div>
</template>

<script>

export default {
  name: "",
  props: {
    itemsData: { 
      type: Array, 
      required: true 
    },
  },
  data() {
    return {
      start: 0,
    }
  },
  computed: {

    spacerStyle() {
      return {
        transform: 'translateY(11px)',
      }
    },

    listStyle() {
      return {
        height: '222px',
        position: 'relative',
        overflow: 'hidden',
        overflowAnchor: 'none', // onItemsResize does this
      }
    },
  },

  methods: {
    

    /**
     * Sets a new start index
     * Calculates end index and updates observed items in ResizeObserver
     */
    setStart() {
      
    },

  },

  /* watch: {
    itemsData() {
      this.setStart(this.start)
    },
  }, */

  created() {
    
  },

  beforeDestroy() {
    
  },

  activated() {
    
  },

  deactivated() {
    
  },
}
</script>
