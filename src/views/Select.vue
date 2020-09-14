<template>
  <div>
    <h2 style=" margin-left: 30px; margin-top: 20px  ">Keyword: {{$store.state.keyword}}</h2>
    <!-- <button v-on:click=getKeyword>qweqr</button> -->
    <!-- {{$keyword}} -->
    <div class="data" v-for="gd in getGraphList" :key="gd.id">
      <el-row>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <br />
            <div class="imgbox-updated">
                <img class="img" v-bind:src="gd.imageLink" v-bind:alt="'Park-' + gd.id">
            </div>
            <br />
          </div>
        </el-col>
        <el-col :span="20">
<!--          <el-row>-->
<!--            <el-col :span="4" class="park">{{gd.parking}}</el-col>-->
<!--          </el-row>-->
          <br/>
          <div class="name">{{gd.parkingLot}}</div>
          <el-row> <!--gd.parking -> will be graph name -->
<!--              <router-link :to="`/detail/${gd.parking}`"><el-col :span="2" class="btn">LOD</el-col> </router-link>-->
              <router-link :to="{ name: 'Detail', params: { graph_obj: gd }}"><el-col :span="2" class="btn">LOD</el-col> </router-link>
            <el-col :span="2" class="btn">View Map</el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "Select",
  components: {},
  data() {
    return {
      keyword: this.$store.state.keyword,
        //getGraphList: [] // it will uncommented once getKeyword data prepared
        //Note-> hardcoded array for now, this result will populate from getKeyword method (props name in objects are taken randomly for binding testing)
        getGraphList: [
            {id:1, parkingLot : 'http://www.city-hub.kr/ontologies/2019/1/parking#parkinglot_yt_lot_1', parking:'Yatap first transfer parking lot', lan:'forMap', lat:'forMap', imageLink: require('@/assets/images/smart_city_ontology.jpg')},
            {id:2, parkingLot: 'http://www.city-hub.kr/ontologies/2019/1/parking#parkingspot_23_yt_lot_1', parking:'Yatap second transfer parking lot', lan:'forMap', lat:'forMap', imageLink: require('@/assets/images/smart_city_ontology.jpg')},
            {id:3, parkingLot: 'http://www.city-hub.kr/ontologies/2019/1/parking#parkinglot_yt_lot_3', parking:'CGV Yatap parking lot', lan:'forMap', lat:'forMap', imageLink: require('@/assets/images/smart_city_ontology.jpg')}
        ]
    };
  },
  mounted() {
    this.getKeyword();

  },
  methods: {
    // getKeyword() {
    //   const baseURI = "http://localhost:3000";
    //   this.$http.get(`${baseURI}/graphs`).then((result) => {
    //     // alert(result.data);
    //     this.gets = result.data;
    //     // alert(this.keyword)
    //   });
    //   alert(this.gets)
    // },
    // getKeyword() {
    //   // const axios = require('axios');
    //   axios.get('http://192.168.0.20:3000/graphs')
    //   .then(res => {
    //     this.gets = res.data.map(r => r.data)
    //   })
    //   alert(this.gets);
    // }

    getKeyword() {
      const baseURI = "http://localhost:3000";
      axios.get(baseURI+'/graphList').then(res => {
        this.getGraphList = res.data;
        // alert(res.data.graphList);
        console.log(this.getGraphList.graphList);
        axios.get(baseURI+'/graph').then(res2 => {
          console.log(res2.data);
        })
      })
    }
  },
  created() {},
};
</script>

<style scoped>
h2 {
  font-size: 1.4em;
  color: #373f42;
  margin: 0 0 0.25em 0;
}

.park {
  background-color: orange;
  margin: 30px 0px 0px 30px;
  text-align: center;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.name {
  margin: 20px 0px 0px 20px;
  font-size: 1.1em;
}

.btn {
  border: solid 1px cornflowerblue;
  margin: 20px 0px 10px 20px;
  text-align: center;
  color: cornflowerblue;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.data {
  background-color: white;
  /* width: 75%; */
  margin-left: 100px;
  margin-top: 40px;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.imgbox-updated {
    height: 115px;
    width: 210px;
    border: solid 1px;
    margin: 0px 35px 0px 35px;
    background-color: #b5bec3;
    box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.img {
  border: 1px solid #ddd;
  border-radius: 4px;
  height: 120px;
  width: 215px;
}

.imgbox {
  border: solid 1px;
  margin: 0px 35px 0px 35px;
  background-color: #b5bec3;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.bg-purple {
  margin-right: 0px;
  border-right: solid 1px #b5bec3;
  text-align: center;
}

.grid-content {
  min-height: 36px;
  padding: 3px
}

</style>
