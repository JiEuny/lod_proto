<template>
    <div>
        <div>
            <img style="margin:80px 80px 100px 80px" src="./../assets/graph.png"/>
        </div>
        <!-- <svg width="50" height="50">
            <circle cx="25" cy="25" r="25"  />
            <line x1="5" y1="5" x2="40" y2="40" stroke="gray" stroke-width="5"  />

    </svg> -->
    </div>

</template>

<script>
// import * as d3 from 'd3';



export default {
  name: 'Graph',
  data() {
      return {
        data: [
            { "color": "black"},
            { "color": "blue"},
            { "color": "green"}
        ]
      }
  },
  methods: {
    
  },
  mounted() {

  }
    
}
</script>

<style scoped>


</style>