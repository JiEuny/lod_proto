<template>
    <div>
        <div class="data" style="align:center; margin-right:100px">
            <el-row>
                <el-col :span="4">
                    <div class="grid-content bg-purple">
                        <br />
                        <div class="imgbox-updated">
                            <img class="img"  :src="getImgUrl(graphDetail ? graphDetail.image:'')" v-bind:alt="graphDetail?graphDetail.name:'No Default Image for Now'">
                        </div>
                        <br />
                    </div>
                </el-col>
                <el-col :span="20">
                    <br/>
                    <div class="name">{{graphDetail?graphDetail.graphName:graphDetailGraphName}}</div>
                    <el-row>
                        <div class="name" style="color:blue; text-decoration: underline;">http://203.253.128.164:7579/ParkingSpot/Yatap/Public</div>
<!--                        <span v-show="!graphDetail" style="align: center; color: red"><br/>Note => Detail will get from LOD button, For that search by keyword in Search tab or click on Select on the top Menu</span>-->
                    </el-row>
                </el-col>
            </el-row>
        </div>
        <div class="detail" style="align:center; margin-right:100px">
            <el-tabs type="border-card">
                <el-tab-pane label="Text" v-if="graphDetail && graphDetail.image != `ontology`">

                    <div style="margin:30px 30px 30px 30px">
                        <b-table-simple hover small caption-top responsive bordered >
                            <b-tbody >
                                <b-tr>
                                    <b-th rowspan="2" colspan="1" class="text-center" variant="secondary" >Location Information</b-th>
                                    <b-th class="text-center tbColSet2" colspan="1" >Address</b-th>
                                    <b-td colspan="3" variant="variant" class="tbColSet3">{{graphInfoDataForTextTab.address}}</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center tbColSet2"  >Latitude And Longitude</b-th>
                                    <b-td class="tbColSet3">Lat: {{graphInfoDataForTextTab.lat}},  Long: {{graphInfoDataForTextTab.long}}</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="2" class="text-center" variant="secondary" >Operating Time</b-th>
                                    <b-th class="text-center tbColSet2">Open - End Date</b-th>
                                    <b-td class="tbColSet3">{{graphInfoDataForTextTab.beginningDate}} - {{graphInfoDataForTextTab.endDate}}</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center tbColSet2" variant="secondary" >Modified date</b-th>
                                    <b-td class="tbColSet3">{{new Date(graphInfoDataForTextTab.modifiedDate) | dateFormat('YY/MM/DD hh:mm:ss A') }}</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th rowspan="6" class="text-center" variant="secondary" >Details</b-th>
                                    <b-th class="text-center tbColSet2" >Contact</b-th>
                                    <b-td  class="tbColSet3">{{graphInfoDataForTextTab.contact}}</b-td>
                                </b-tr>
                                <b-th class="text-center tbColSet2" >Email</b-th>
                                <b-td  class="tbColSet3">{{graphInfoDataForTextTab.email}}</b-td>
                                <b-tr>
                                    <b-th class="text-center tbColSet2" variant="secondary" >Total Parking Space</b-th>
                                    <b-td class="tbColSet3">{{graphInfoDataForTextTab.totalParkingSpace}}</b-td>
                                </b-tr>
                                <b-tr>
                                    <b-th class="text-center tbColSet2" variant="secondary" >Available Number Of Spots</b-th>
                                    <b-td class="tbColSet3">{{graphInfoDataForTextTab.availableNumberOfSpots}}</b-td>
                                </b-tr>
                            </b-tbody>
                        </b-table-simple>
                    </div>

                </el-tab-pane>
                <el-tab-pane label="Graph" style="text-align:center; height:786px">
                    <DrawGraph  v-if="graphNodesLinksDataLoaded" :drawGraphData="graphdataProps"/>
                </el-tab-pane>
                <el-tab-pane label="Download">
                        <b-card-group deck class="b-card-group" >
                            <b-card class="b-card"
                                    header="JSON-LD" align="center">
                                <b-card-text>
                                    <b-button title="Download(disabled)" class="b-button" :disabled="true">
                                        <b-icon class="b-icon" icon="download" aria-hidden="true"></b-icon>
                                    </b-button>
                                </b-card-text>
                            </b-card>
                            <b-card class="b-card"
                                    header="RDF/XML" align="center">
                                <b-card-text>
                                    <b-button title="Download" class="b-button">
                                        <b-icon class="b-icon" icon="download" aria-hidden="false" v-on:click="getGraphInfoByGraphName(true)"></b-icon>
                                    </b-button>
                                </b-card-text>
                            </b-card>
                            <b-card class="b-card"
                                    header="Turtle" align="center">
                                <b-card-text>
                                    <b-button title="Download(disabled)" class="b-button" :disabled="true">
                                        <b-icon class="b-icon" icon="download" aria-hidden="false"></b-icon>
                                    </b-button>
                                </b-card-text>
                            </b-card>
                            <b-card class="b-card"
                                    header="N3" align="center">
                                <b-card-text>
                                    <b-button title="Download(disabled)" class="b-button" :disabled="true">
                                        <b-icon class="b-icon" icon="download" aria-hidden="false"></b-icon>
                                    </b-button>
                                </b-card-text>
                            </b-card>

                            <b-card class="b-card"
                                    header="nTriple" align="center">
                                <b-card-text>
                                    <b-button title="Download(disabled)" class="b-button" :disabled="true">
                                        <b-icon class="b-icon" icon="download" aria-hidden="false"></b-icon>
                                    </b-button>
                                </b-card-text>
                            </b-card>
<!--                            <b-card
                                    border-variant="secondary"
                                    header="nTriple"
                                    header-border-variant="secondary"
                                    align="center">
                                <b-card-text>
                                    <b-button title="Download" class="b-button">
                                    <b-icon class="b-icon" icon="download" aria-hidden="false"></b-icon>
                                </b-button>
                                </b-card-text>
                            </b-card>-->
                        </b-card-group>
                </el-tab-pane>
           <!--     <el-tab-pane label="LOD publish">
                    <div style="text-align:center">
                        <img style="margin:80px 80px 100px 80px" src="./../assets/fileimg.png"/>
                    </div>
                </el-tab-pane>-->
            </el-tabs>
        </div>

    </div>
</template>

<script>

import axios from 'axios'
import DrawGraph from './DrawGraph';

export default {
  name: 'Detail',
  data() {
      return {
          graphDetail :  this.$route.params.graph_obj,
          graphDetailGraphName :  this.$route.query.graphName,
          graphNodesLinksDataLoaded: false,
          graphInfoData: '',
          graphInfoDataForTextTab: {address: '', lat:'', long: '', beginningDate:'', endDate:'', contact:'',
           email:'', modifiedDate:'', totalParkingSpace:'', availableNumberOfSpots:'',  },

          graphInfoDataForNodesLinks:{nodes: [], linkes:[]},
          graphdataProps : '',
          asFile : false,

          graphdataPropsTest: { // object for testing
              "nodes":  [
                  {"id": "Namedindividual",  "name": "Namedindividual", shape : 'circle'},
                  {"id": "ProfilePicture_1_yatap_01",  "name": "ProfilePicture_1_yatap_01"},
                  {"id": "TemporalEntity_1_yatap_01",   "name": "TemporalEntity_1_yatap_01", shape : 'square'},
                  {"id":  "Currency_1_yatap_01",   "name": "Currency_1_yatap_01", type: 2},
                  {"id": "AvailableParkingSpots_1_yatap_01",   "name": "AvailableParkingSpots_1_yatap_01", shape : 'square'},
                  {"id": "PCEService_1_yatap_01",   "name": "PCEService_1_yatap_01"},
                  {"id": "ParkingCongestionEstimation_1_yatap_01",   "name": "ParkingCongestionEstimation_1_yatap_01"},
                  {"id": "ParkingLotProfile_1_yatap_01",   "name": "ParkingLotProfile_1_yatap_01"},
                  {"id": "ParkingLot_1_yatap_01",   "name": "ParkingLot_1_yatap_01", shape : 'square'},

              ],
              "linkes":  [
                  { "source":  "ProfilePicture_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "TemporalEntity_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "Currency_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "AvailableParkingSpots_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "PCEService_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "ParkingCongestionEstimation_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "ParkingLotProfile_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "ParkingLot_1_yatap_01",  "target":  "Namedindividual",  "value":  4,  type: "KNOWS"},
                  { "source":  "ParkingLot_1_yatap_01",  "target":  "TemporalEntity_1_yatap_01",  "value":  4,  type: "KNOWS"},
              ]
          },

          };
  },
  components: {
      DrawGraph
  },
  methods: {
    getImgUrl(img) {
        if(img){
            var images = require.context('@/assets/images', false, /\.jpg$/)
            return images('./' + img + ".jpg")
        }
    },
    downloadGraphInfoFile: function(fileData) {
        const xmltext = fileData;
        let pom = document.createElement('a');

        const filename = "graphInfoFile.xml";
         pom = document.createElement('a');
        const bb = new Blob([xmltext], {type: 'text/plain'});

        pom.setAttribute('href', window.URL.createObjectURL(bb));
        pom.setAttribute('download', filename);
        pom.dataset.downloadurl = ['text/plain', pom.download, pom.href].join(':');
        pom.draggable = true;
        pom.classList.add('dragout');
        pom.click();
      },

     getGraphInfoByGraphName(file) {

         const baseURI = "http://localhost:3000";
        // const gName =  (this.graphDetail) ?  this.graphDetail.name : 'no name found'
         const gName =  (this.graphDetailGraphName) ?  this.graphDetailGraphName : 'no name found';
         let isFile = this.asFile;

         if(file)
             isFile = file

         //"http://localhost:3000/graphs/parking:yatap_01?prefixFormat=normal&limit=10&asFile=false"
         // axios.get(baseURI+`/graphs/${gName}?prefixFormat=normal&limit=10&asFile=`+ isFile).then(gInfoRes => {
         axios.get(baseURI+`/graphs/${gName}?prefixFormat=normal&limit=10&asFile=`+ isFile + '&isOntology=' + this.graphDetail.image).then(gInfoRes => {
             console.log(gInfoRes.data);
             this.graphInfoData = gInfoRes.data;

             // if file is true then api will return xml text for download
             if (file) {
                 var xmltext =  this.graphInfoData;
                 this.downloadGraphInfoFile(xmltext)

             }
             else {

             const graphTriplesArr = this.graphInfoData['graph-triples'];
             let regForNumbers = /^(\D*)(\d+)/;
             let regForDateTime = /\d{4}-[01]\d-[0-3]\dT[0-2]\d:[0-5]\d:[0-5]\d(?:\.\d+)?Z?/;
             let regForDataProperty = /[A-Za-z]+:/;
             // let regForLatLong = /^(\D*)(\d+).(\D*)(\d+)/;

             for (let i = 0; i < graphTriplesArr.length; i++) {

                 const strSubject = graphTriplesArr[i].subject;
                 const strPredicate = graphTriplesArr[i].predicate;
                 let graphNameVal = false;
                 const gNameForFilter = gName.substring(gName.indexOf(":") + 1, gName.length);

                 if (strSubject.search(gNameForFilter) !== -1)
                     graphNameVal = true;

                 // get create properties set (Text Tab data) for given graphName
                 if (graphNameVal && this.graphDetail && this.graphDetail.image !== 'ontology') {
                     if (strPredicate.search('hasPostalAddress') !== -1)
                         this.graphInfoDataForTextTab.address = graphTriplesArr[i].object;
                     else if (strPredicate.search('hasLatitute') !== -1)
                         this.graphInfoDataForTextTab.lat = graphTriplesArr[i].object.match(/^(\D*)(\d+).(\D*)(\d+)/)[0]; // get lat, long from string by regex
                     else if (strPredicate.search('hasLongitude') !== -1)
                         this.graphInfoDataForTextTab.long = graphTriplesArr[i].object.match(/^(\D*)(\d+).(\D*)(\d+)/)[0];
                     else if (strPredicate.search('hasBeginning') !== -1)
                         this.graphInfoDataForTextTab.beginningDate = graphTriplesArr[i].object;
                     else if (strPredicate.search('hasEnd') !== -1)
                         this.graphInfoDataForTextTab.endDate = graphTriplesArr[i].object;
                     // else if (graphNameVal && strPredicate.search('dayOfWeek') !== -1)
                     else if (strPredicate.search('hasTelephoneNumber') !== -1)
                         this.graphInfoDataForTextTab.contact = graphTriplesArr[i].object;
                     else if (strPredicate.search('hasEmail') !== -1)
                         this.graphInfoDataForTextTab.email = graphTriplesArr[i].object;
                     // else if (graphNameVal && strPredicate.search('hasDateModified') !== -1)
                     else if (strPredicate.search('inXSDDateTime') !== -1)
                         this.graphInfoDataForTextTab.modifiedDate = graphTriplesArr[i].object.match(regForDateTime)[0]; // get date from string by regex
                     else if (strPredicate.search('hasTotalSpots') !== -1)
                         this.graphInfoDataForTextTab.totalParkingSpace = graphTriplesArr[i].object.match(regForNumbers)[0];// get number from string by regex
                     else if (strPredicate.search('hasAvailableNumberOfSpots') !== -1)
                         this.graphInfoDataForTextTab.availableNumberOfSpots = graphTriplesArr[i].object.match(regForNumbers)[0];

                 } else
                     console.log('no data found for graph Name');
             }

             // create array (for Graph Tab) of node and links to draw graph

             for (let j = 0; j < graphTriplesArr.length; j++) {


                 // Original string of sub, obj and pred
                 const strSubjectOrg = graphTriplesArr[j].subject;
                 const strPredicateOrg = graphTriplesArr[j].predicate;
                 const strObjectOrg = graphTriplesArr[j].object;

                 // remove word from sub string before ':',  like parking:text -> text
                 const strSubject = strSubjectOrg.substring(strSubjectOrg.indexOf(":") + 1, strSubjectOrg.length);
                 const strPredicate = strPredicateOrg.substring(strPredicateOrg.indexOf(":") + 1, strPredicateOrg.length);

                 // remove word from obj string after '^^',  like 110^^textbalaba -> textbalaba
                 const strObject = (strObjectOrg.indexOf('^') !== -1 || strObjectOrg.search(':') !== 0) ?
                     ((strObjectOrg.indexOf('^') !== -1) ? strObjectOrg.substring(0, strObjectOrg.indexOf("^^"))
                         : strObjectOrg.substring(strObjectOrg.indexOf(":") + 1, strObjectOrg.length))
                     : strObjectOrg

                 let graphNameVal = false;
                 // remove word from string before ':',  like parking:text -> text
                 const gNameForFilter = gName.substring(gName.indexOf(":") + 1, gName.length);

                 if (strSubject.search(gNameForFilter) !== -1)
                     graphNameVal = true;

                 const shape = strObjectOrg.match(regForDataProperty)
                 && (!strObject.match(regForDateTime))
                 && !Number.isInteger(parseInt(strObject)) ? "circle" : "square";
                 console.log('shape----'+ shape + ',   strObjectOrg----'+strObjectOrg + ',    strObject-----'+strObject)
                 if (graphNameVal || this.graphDetail && this.graphDetail.image == 'ontology') {

                     // discluded hasImage pred and type pred
                     if (strPredicate.search('hasImage') === -1 && strPredicate.search('type') === -1) {
                         // create nodes array for graph
                         if (this.graphInfoDataForNodesLinks.nodes.indexOf(strSubject) === -1) {
                             this.graphInfoDataForNodesLinks.nodes.push({id: strSubject, name: strSubject, "group": 1, shape: shape});
                         }

                         if (this.graphInfoDataForNodesLinks.nodes.indexOf(strObject) === -1) {
                             this.graphInfoDataForNodesLinks.nodes.push({id: strObject, name: strObject, "group": 1, shape: shape});
                         }
                         // create linkes array for graph
                         this.graphInfoDataForNodesLinks.linkes.push({
                             source: strSubject,
                             target: strObject,
                             type: strPredicate
                         })
                     }
                 }

             }
             console.log('-----graphInfoDataForNodesLinks-----> ', this.graphInfoDataForNodesLinks);
             // pass  graph array to props
             this.graphdataProps = this.graphInfoDataForNodesLinks;
             // this.graphdataProps = this.graphdataPropsTest;
             // load DrawGraph component  when prapare graph array
             this.graphNodesLinksDataLoaded = true;
            }
         });
      }
  },
   mounted() {
             // mount method to get graph triple by graph api (param -> graph name)
        this.getGraphInfoByGraphName();
    },
}
</script>


<style scoped>

marker {
    stroke: #999;
    fill:rgba(124,240,10,0);
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
.tbColSet2 {
    background-color: #c6c9cdc9;
    bottom: 1px;
}
.tbColSet3 {
    background-color: #dfe3e8c9;
    bottom: 1px;
}
.b-button {
    background: rgba(212, 194, 194, 0.04);
    border-color: cornflowerblue;
    box-shadow: 2px 2px 2px 1px rgba(128, 128, 128, 0.1);
}

.b-icon {
    border-color: rgba(115, 108, 129, 0.54);
    color: cornflowerblue;
    width: 80px;
}

.b-card {
    border-variant:"secondary";
    header-border-variant :"secondary";
    align:"center";
    max-width: 10rem;
    max-height: 25rem;
    align:"center"
}

.b-card-group {
    align-content: center;
    display: flex;
    ms-flex-flow: row wrap;
    webkit-box-orient: horizontal;
    webkit-box-direction: normal;
    flex-flow: row wrap;
    margin-right: 0px;
    margin-left:330px;
}
</style>
