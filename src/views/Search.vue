<template>
    <div class="cont" >
        <h2 style=" margin-left: 30px; margin-top: 20px ">Data Hub LOD Searching Service</h2>

        <div style="margin-top: 180px;" class="cent">
            <h3>Search by Keywork</h3>
            <div style="margin-top: 30px; align:center; margin-bottom: 200px">
                <el-input style="width:40%" placeholder="Please input" v-model="input" class="input-with-select">
                    <el-button slot="append" icon="el-icon-search"></el-button>
                </el-input>
            </div>
        </div>
    </div>
</template>

<script>

export default {
  name: 'Search',
  data() {
      return{
          input: ''
      }
  }

}
</script>

<style scoped>

.cont {
    width: 100%;
}

h2 {
    font-size: 1.4em;
    color: #373f42;
    margin: 0 0 0.25em 0;
}

h3 {
    
    color: #373f42;
    font-size: 1.1em;
    /* color: #96a9b5; */
}

.cent {
    width: 100%;
    align-self: center;
    text-align: center
}

</style>