<template>
    <div>
        <div class="data" style="align:center; margin-right:100px">
            <el-row>
                <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <br />
                        <div class="imgbox-updated">
                            <img class="img" v-bind:src="graphDetail.imageLink" v-bind:alt="'Park-' + graphDetail.id">
                        </div>
                        <br />
                    </div>
                </el-col>
                <el-col :span="20">
     <!--               <el-row>
                        <el-col :span="5" class="park">{{graphDetail.parking}}</el-col>
                    </el-row>-->
                    <br/>
                    <div class="name">{{graphDetail.parkingLot}}</div>
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
                                    <b-th rowspan="2" colspan="1" class="text-center" variant="secondary" >Location information</b-th>
                                    <b-th class="text-center" colspan="1" variant="secondary" >address</b-th>
                                    <b-td colspan="3">KR, Gyeonggi-do, Seongnam-si</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >Latitude and longitude</b-th>
                                    <b-td>Lat : 00.00, Lon : 00.00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="2" class="text-center" variant="secondary" >operating time</b-th>
                                    <b-th class="text-center" variant="secondary" >open</b-th>
                                    <b-td>월-금 AM 10:00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >deadline</b-th>
                                    <b-td>월-금 PM 10:00</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="4" class="text-center" variant="secondary" >Details</b-th>
                                    <b-th class="text-center" variant="secondary" >Contact</b-th>
                                    <b-td>031-000-0000</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >Modified date</b-th>
                                    <b-td>2018-12-11</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >Total parking space</b-th>
                                    <b-td>110</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center" variant="secondary" >Parking type</b-th>
                                    <b-td>Indoor parking lot</b-td>
                                </b-tr>
                            </b-tbody>
                        </b-table-simple>
                    </div>

                </el-tab-pane>
                <el-tab-pane label="Test" style="text-align:center; height:786px">

                    <Test/>
                </el-tab-pane>
                <el-tab-pane label="LOD publish">
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
// import Graph from './Graph';
import Test from './Test2';

// var svg = d3.select("#svg-body").append("svg")
// 				.attr("width", 800)
// 				.attr("height", 600)
// 				;

// 	var force = d3.layout.force().size([800, 600]);

//     var graph = triplesToGraph(this.triples);

// function filterNodesById(nodes,id){
// 			return nodes.filter(function(n) { return n.id === id; });
// 		}

// 		function triplesToGraph(triples){

// 			svg.html("");
// 			//Graph
// 			var graph={nodes:[], links:[]};

// 			//Initial Graph from triples
// 			triples.forEach(function(triple){
// 				var subjId = triple.subject;
// 				var predId = triple.predicate;
// 				var objId = triple.object;

// 				var subjNode = filterNodesById(graph.nodes, subjId)[0];
// 				var objNode  = filterNodesById(graph.nodes, objId)[0];

// 				if(subjNode==null){
// 					subjNode = {id:subjId, label:subjId, weight:1};
// 					graph.nodes.push(subjNode);
// 				}

// 				if(objNode==null){
// 					objNode = {id:objId, label:objId, weight:1};
// 					graph.nodes.push(objNode);
// 				}


// 				graph.links.push({source:subjNode, target:objNode, predicate:predId, weight:1});
// 			});

// 			return graph;
// 		}


// 		function update(){
// 			// ==================== Add Marker ====================
//             svg.append("svg:defs").selectAll("marker")
//                 .data(["end"])
//                 .enter().append("svg:marker")
//                 .attr("id", String)
//                 .attr("viewBox", "0 -5 10 10")
//                 .attr("refX", 30)
//                 .attr("refY", -0.5)
//                 .attr("markerWidth", 6)
//                 .attr("markerHeight", 6)
//                 .attr("orient", "auto")
//                 .append("svg:polyline")
//                 .attr("points", "0,-5 10,0 0,5");

// 			// ==================== Add Links ====================
// 			var links = svg.selectAll(".link")
// 								.data(graph.links)
// 								.enter()
// 								.append("line")
// 									.attr("marker-end", "url(#end)")
// 									.attr("class", "link")
// 									.attr("stroke-width",1)
// 							;//links

// 			// ==================== Add Link Names =====================
//             var linkTexts = svg.selectAll(".link-text")
//                 .data(graph.links)
//                 .enter()
//                 .append("text")
//                 .attr("class", "link-text")
//                 .text( function (d) { return d.predicate; });

// 				//linkTexts.append("title")
// 				//		.text(function(d) { return d.predicate; });

// 			// ==================== Add Link Names =====================
//             var nodeTexts = svg.selectAll(".node-text")
//                 .data(graph.nodes)
//                 .enter()
//                 .append("text")
//                 .attr("class", "node-text")
//                 .text( function (d) { return d.label; });

// 				//nodeTexts.append("title")
// 				//		.text(function(d) { return d.label; });

// 			// ==================== Add Node =====================
// 			var nodes = svg.selectAll(".node")
// 								.data(graph.nodes)
// 								.enter()
// 								.append("circle")
// 									.attr("class", "node")
// 									.attr("r",8)
// 									.call(force.drag)
// 							;//nodes

// 			// ==================== Force ====================
// 			force.on("tick", function() {
// 				nodes
// 					.attr("cx", function(d){ return d.x; })
// 					.attr("cy", function(d){ return d.y; })
// 					;

// 				links
//                     .attr("x1", 	function(d)	{ return d.source.x; })
//                     .attr("y1", 	function(d) { return d.source.y; })
//                     .attr("x2", 	function(d) { return d.target.x; })
//                     .attr("y2", 	function(d) { return d.target.y; });

// 				nodeTexts
// 					.attr("x", function(d) { return d.x + 12 ; })
// 					.attr("y", function(d) { return d.y + 3; })
// 					;

// 				linkTexts
// 					.attr("x", function(d) { return 4 + (d.source.x + d.target.x)/2  ; })
// 					.attr("y", function(d) { return 4 + (d.source.y + d.target.y)/2 ; })
// 					;
// 			});

// 			// ==================== Run ====================
//             force
//                 .nodes(graph.nodes)
//                 .links(graph.links)
//                 .charge(-500)
//                 .linkDistance(100)
//                 .start();
// 		}

export default {
  name: 'Detail',
  data() {
      return {
          data: [99, 71, 78, 25, 36, 92],
          line: '',
          triples: [
            {subject:"ThaiLand", predicate:"hasFood", object:"TomYumKung"}
            ],
          graphDetail :  this.$route.params.graph_obj
      };
  },
  components: {
    // Graph,
    Test
  },
  mounted() {

                // update();
    // const svg = d3.select(this.$el)
    //   .append('svg')
    //   .attr('width', 500)
    //   .attr('height', 270)
    //   .append('g')
    //   .attr('transform', 'translate(0, 10)');

    // const x = d3.scaleLinear().range([0, 430]);
    // const y = d3.scaleLinear().range([210, 0]);
    // d3.axisLeft().scale(x);
    // d3.axisTop().scale(y);
    // x.domain(d3.extent(this.data, (d, i) => i));
    // y.domain([0, d3.max(this.data, d => d)]);

    // const createPath = d3.line()
    //   .x((d, i) => x(i))
    //   .y(d => y(d));
    // svg.append('path').attr('d', createPath(this.data));

    this.calculatePath();
  },
  methods: {
    getScales() {
      const x = d3.scaleTime().range([0, 430]);
      const y = d3.scaleLinear().range([210, 0]);
      d3.axisLeft().scale(x);
      d3.axisBottom().scale(y);
      x.domain(d3.extent(this.data, (d, i) => i));
      y.domain([0, d3.max(this.data, d => d)]);
      return { x, y };
    },
    calculatePath() {
      const scale = this.getScales();
      const path = d3.line()
        .x((d, i) => scale.x(i))
        .y(d => scale.y(d));
      this.line = path(this.data);
    }
  }

}
</script>


<style scoped>
.node {
    stroke: #fff;
    fill:#ddd;
    stroke-width: 1.5px;
    }
.link {
    stroke: #999;
    stroke-opacity: .6;
    stroke-width: 1px;
}
marker {
    stroke: #999;
    fill:rgba(124,240,10,0);
}
.node-text {
	font: 11px sans-serif;
	fill:black;
}
.link-text {
	font: 9px sans-serif;
	fill:grey;
}

.svg {
    margin: 25px;
}

.path {
    fill:none;
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

</style>
