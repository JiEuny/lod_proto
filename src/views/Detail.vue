<template>
    <div>
        <div class="data" style="align:center; margin-right:100px">
            <el-row >
                <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <br/>
                        <div class="imgbox">
                            <br/><br/>image<br/><br/><br/>
                        </div>
                        <br/>
                    </div>
                </el-col>
                <el-col :span="20">
                    <el-row>
                        <el-col :span="3" class="park">실내주차장</el-col>
                    </el-row>
                    <div class="name">야탑 제1 환승 주차장
                    </div>
                    <el-row>
                    <div class="name" style="color:blue; text-decoration: underline;">http://203.253.128.164:7579/ParkingSpot/Yatap/Public</div>
                    </el-row>
                </el-col>
            </el-row>
        </div>
        <div class="detail" style="align:center; margin-right:100px">
            <el-tabs type="border-card">
                <el-tab-pane label="Text">

                    <div style="margin:30px 30px 30px 30px">
                        <b-table-simple hover small caption-top responsive bordered >
                            <b-tbody >
                                <b-tr>
                                    <b-th rowspan="2" colspan="1" class="text-center" variant="secondary" >위치정보</b-th>
                                    <b-th class="text-center" colspan="1" variant="secondary" >주소</b-th>
                                    <b-td colspan="3">KR, 경기도, 성남시</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >위경도</b-th>
                                    <b-td>Lat : 00.00, Lon : 00.00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="2" class="text-center" variant="secondary" >운영시간</b-th>
                                    <b-th class="text-center" variant="secondary" >오픈</b-th>
                                    <b-td>월-금 AM 10:00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >마감</b-th>
                                    <b-td>월-금 PM 10:00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="4" class="text-center" variant="secondary" >세부정보</b-th>
                                    <b-th class="text-center" variant="secondary" >연락처</b-th>
                                    <b-td>031-000-0000</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >수정된 날짜</b-th>
                                    <b-td>2018-12-11</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >총 주차면</b-th>
                                    <b-td>110</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >주차장 유형</b-th>
                                    <b-td>실내 주차장</b-td>
                                </b-tr>
                            </b-tbody>
                        </b-table-simple>
                    </div>

                </el-tab-pane>
                <el-tab-pane label="Graph" style="text-align:center">
                    <svg width="500" height="270">
                        <!-- <g style="transform: translate(0, 10px)">
                        <path :d="line" />
                        </g> -->
                    </svg>
                </el-tab-pane>
                <el-tab-pane label="LOD 발행">
                    <div style="text-align:center">
                        <img style="margin:80px 80px 100px 80px" src="./../assets/fileimg.png"/>
                    </div>
                </el-tab-pane>
            </el-tabs>
        </div>

    </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'Detail',
  data() {
      return {
          data: [99, 71, 78, 25, 36, 92],
          line: ''
      };
  },
  components: {
    
  },
  mounted() {
    const svg = d3.select(this.$el)
      .append('svg')
      .attr('width', 500)
      .attr('height', 270)
      .append('g')
      .attr('transform', 'translate(0, 10)');

    const x = d3.scaleLinear().range([0, 430]);
    const y = d3.scaleLinear().range([210, 0]);
    d3.axisLeft().scale(x);
    d3.axisTop().scale(y);
    x.domain(d3.extent(this.data, (d, i) => i));
    y.domain([0, d3.max(this.data, d => d)]);

    const createPath = d3.line()
      .x((d, i) => x(i))
      .y(d => y(d));
    svg.append('path').attr('d', createPath(this.data));

    // this.calculatePath();
  },
  methods: {
    // getScales() {
    //   const x = d3.scaleTime().range([0, 430]);
    //   const y = d3.scaleLinear().range([210, 0]);
    //   d3.axisLeft().scale(x);
    //   d3.axisBottom().scale(y);
    //   x.domain(d3.extent(this.data, (d, i) => i));
    //   y.domain([0, d3.max(this.data, d => d)]);
    //   return { x, y };
    // },
    // calculatePath() {
    //   const scale = this.getScales();
    //   const path = d3.line()
    //     .x((d, i) => scale.x(i))
    //     .y(d => scale.y(d));
    //   this.line = path(this.data);
    // }
  }

}
</script>


<style scoped>

.svg {
    margin: 25px;
}

.path {
    fill:nono;
    stroke: #76bf8a;
    stroke-width: 3px;
}

.data {
    background-color: #d6d6d6;
    /* width: 75%; */
    margin-left: 100px;
    margin-top: 30px;
    box-shadow: 2px 2px 2px 2px rgba(128, 128, 128, 0.1);
}

.detail {
    background-color: white;
    margin-left: 100px;
    margin-top: 30px;
}

.bg-purple {
    margin-right: 0px;
    border-right: solid 1px #b5bec3;
    text-align: center
}

.imgbox {
    border: solid 1px;
    margin: 0px 35px 0px 35px;
    background-color: #efefef
}

.name {
    margin: 20px 0px 0px 20px;
    font-size: 1.1em;
    
}

.park {
    background-color: orange;
    margin: 20px 0px 0px 20px;
    text-align: center;
}

</style>