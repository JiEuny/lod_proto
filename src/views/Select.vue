<template>
  <div>
    <h2 style=" margin-left: 30px; margin-top: 20px  ">검색어: {{$store.state.keyword}}</h2>

    <div class="data" style="align:center; margin-right:100px">
      <el-row>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <br />
            <div class="imgbox">
              <br />
              <br />image
              <br />
              <br />
              <br />
            </div>
            <br />
          </div>
        </el-col>
        <el-col :span="20">
          <el-row>
            <el-col :span="3" class="park">실내주차장</el-col>
          </el-row>
          <div class="name">야탑 제1 환승 주차장</div>
          <el-row>
            <el-col :span="3" class="btn">LOD</el-col>
            <el-col :span="3" class="btn">지도 보기</el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>
    <div class="data" style="align:center; margin-right:100px">
      <el-row>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <br />
            <div class="imgbox">
              <br />
              <br />image
              <br />
              <br />
              <br />
            </div>
            <br />
          </div>
        </el-col>
        <el-col :span="20">
          <el-row>
            <el-col :span="3" class="park">실내주차장</el-col>
          </el-row>
          <div class="name">야탑 제2 환승 주차장</div>
          <el-row>
            <el-col :span="3" class="btn">LOD</el-col>
            <el-col :span="3" class="btn">지도 보기</el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>
    <div class="data" style="align:center; margin-right:100px">
      <el-row>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <br />
            <div class="imgbox">
              <br />
              <br />image
              <br />
              <br />
              <br />
            </div>
            <br />
          </div>
        </el-col>
        <el-col :span="20">
          <el-row>
            <el-col :span="3" class="park">실내주차장</el-col>
          </el-row>
          <div class="name">CGV 야탑 주차장</div>
          <el-row>
            <el-col :span="3" class="btn">LOD</el-col>
            <el-col :span="3" class="btn">지도 보기</el-col>
          </el-row>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>

export default {
  name: "Select",
  components: {},
  data() {
    return {
      keyword: this.$store.state.keyword
    };
  },
  methods: {
    // getKeyword() {
    //   alert(this.keyword);
    // }
  },
  created() {

  }
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
  margin: 20px 0px 0px 20px;
  text-align: center;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.name {
  margin: 20px 0px 0px 20px;
  font-size: 1.1em;
}

.btn {
  border: solid 1px cornflowerblue;
  margin: 20px 0px 0px 20px;
  text-align: center;
  color: cornflowerblue;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.data {
  background-color: white;
  /* width: 75%; */
  margin-left: 100px;
  margin-top: 30px;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.imgbox {
  border: solid 1px;
  margin: 0px 35px 0px 35px;
  background-color: #b5bec3;
  box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.el-row {
  margin-bottom: 20px;
}

.bg-purple {
  margin-right: 0px;
  border-right: solid 1px #b5bec3;
  text-align: center;
}

.grid-content {
  min-height: 36px;
}
</style>