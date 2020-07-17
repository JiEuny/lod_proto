<template>

    <svg width="100%" height="100%">

    </svg>

</template>

<script>
import * as d3 from 'd3';

export default {
    name: 'Test2',
    data() {
        return {
            svg: null,
            zoom: null,
            width: 2048,
            height: 768,
            graphdata: {
                "nodes":  [
                    { "name": "야탑주차장",              "group":  1, "class": "system" },
                    { "name": "잔여주차공간",            "group":  1, "class": "system" },
                    { "name": "kernel",                 "group":  1, "class": "system" },
                    { "name": "systemd",                "group":  1, "class": "mount" },
                    { "name": "-.mount",                "group":  1, "class": "mount" },
                    { "name": "init.scope",             "group":  1, "class": "init" },
                    { "name": "system.slice",           "group":  1, "class": "init" },
                    { "name": "system-getty.slice",     "group":  1, "class": "init" },
                    { "name": "systemd-initctl.socker", "group":  1, "class": "init" },
                    { "name": "tmp.mount",              "group":  1, "class": "init" },
                    { "name": "sys-devices",            "group":  2, "class": "init" },
                    { "name": "boot.mount",             "group":  2, "class": "init" }
                ],
                "linkes":  [
                    { "source":  1,  "target":  0,  "value":  1, "type": "depends" },
                    { "source":  2,  "target":  1,  "value":  8, "type": "depends" },
                    { "source":  3,  "target":  2,  "value":  6, "type": "depends" },
                    { "source":  4,  "target":  3,  "value":  1, "type": "needs" },
                    { "source":  5,  "target":  3,  "value":  1, "type": "needs" },
                    { "source":  6,  "target":  3,  "value":  1, "type": "needs" },
                    { "source":  7,  "target":  3,  "value":  1, "type": "needs" },
                    { "source":  8,  "target":  3,  "value":  2, "type": "needs" },
                    { "source":  9,  "target":  3,  "value":  1, "type": "needs" },
                    { "source": 11,  "target": 10,  "value":  1, "type": "depends" },
                    { "source": 11,  "target":  3,  "value":  3, "type": "depends" },
                    { "source": 11,  "target":  2,  "value":  3, "type": "depends" },
                    { "source": 11,  "target":  3,  "value":  5, "type": "needs" }
                ]
            },

            simulation: null,
            selections: {},
            forceProperties: {
                center: {
                    x: 0.5,
                    y: 0.5
                },
                charge: {
                    enabled: true,
                    strength: -300,
                    distanceMin: 1,
                    distanceMax: 2000
                },
                collide: {
                    enabled: true,
                    strength: .7,
                    iterations: 1,
                    radius: 35
                },
                forceX: {
                    enabled: false,
                    strength: .1,
                    x: .5
                },
                forceY: {
                    enabled: false,
                    strength: .35,
                    y: .5
                },
                link: {
                    enabled: true,
                    distance: 100,
                    iterations: 1
                }
            },
        }
    },
    mounted() {
        this.selections.svg = d3.select("svg")
        this.selections.graph = this.selections.svg.append("g")

        this.zoom = d3.zoom()
            .scaleExtent([1 / 4, 4])
            .on('zoom', this.zoomed)

        this.selections.svg.call(this.zoom)

        this.simulation = d3.forceSimulation()
            .force("link", d3.forceLink())
            .force("charge", d3.forceManyBody())
            .force("collide", d3.forceCollide())
            .force("center", d3.forceCenter())
            .force("forceX", d3.forceX())
            .force("forceY", d3.forceY())
            .on("tick", this.tick)
        
        this.updateForces()
        this.updateData()

    },
    methods: {
        tick() {
            const transform = d => {
                return "translate(" + d.x + "," + d.y + ")";
            }

            const link = d => {
                return "M" + d.source.x + "," + d.source.y + " L" + d.target.x + "," + d.target.y;
            }

            const graph = this.selections.graph
            graph.selectAll("path").attr("d", link)
            graph.selectAll("circle").attr("transform", transform)
            // graph.selectAll("rect").attr("transform", transform)
            graph.selectAll("text").attr("transform", transform)
        },
        updateForces() {
            this.simulation.force("center")
                .x(this.width * this.forceProperties.center.x)
                .y(this.height * this.forceProperties.center.y)
            this.simulation.force("charge")
                .strength(this.forceProperties.charge.strength * this.forceProperties.charge.enabled)
                .distanceMin(this.forceProperties.charge.distanceMin)
                .distanceMax(this.forceProperties.charge.distanceMax);
            this.simulation.force("collide")
                .strength(this.forceProperties.collide.strength * this.forceProperties.collide.enabled)
                .radius(this.forceProperties.collide.radius)
                .iterations(this.forceProperties.collide.iterations);
            this.simulation.force("forceX")
                .strength(this.forceProperties.forceX.strength * this.forceProperties.forceX.enabled)
                .x(this.width * this.forceProperties.forceX.x);
            this.simulation.force("forceY")
                .strength(this.forceProperties.forceY.strength * this.forceProperties.forceY.enabled)
                .y(this.height * this.forceProperties.forceY.y);
            this.simulation.force("link")
                .distance(this.forceProperties.link.distance)
                .iterations(this.forceProperties.link.iterations);
            
            this.simulation.alpha(1).restart();
        },
        updateData() {
            this.simulation.nodes(this.nodes)
            this.simulation.force("link").links(this.links)

            const simulation = this.simulation
            const graph = this.selections.graph

            graph.selectAll("path")
                .data(simulation.force("link").links())
                .enter().append("path")
                .attr("class", d=> "link " + d.type)
                .attr("stroke", "#005900")
                .exit().remove()
            
            // if(this.node.class == "system") {

                // graph.selectAll("circle")
                // .data(simulation.nodes())
                // .enter().append("circle")
                // .attr("r", 25)
                // .attr("class", d => d.class)
                // .attr("fill", "#cce5ff")
                // .attr("stroke", "#003366")
                // .exit().remove()

            // } 
                
            graph.selectAll("circle")
                .data(simulation.nodes())
                .enter().append("circle")
                .attr("r", 25)
                .attr("class", d => d.class)
                .attr("fill", "#ffe5e5")
                .attr("stroke", "#660000")
                .exit().remove()

            



            // graph.selectAll("rect")
            //     .data(simulation.nodes())
            //     .enter().append("rect")
            //     .attr("x", -70)
            //     .attr("y", 40)
            //     .attr("width", 100)
            //     .attr("height", 30)
            //     .attr("fill", "#cce5ff")
            //     .attr("stroke", "#003366")
            //     .exit().remove()

            graph.selectAll("text")
                .data(simulation.nodes())
                .enter().append("text")
                .attr("x", 0)
                .attr("y", ".31em")
                .attr("font", "10px sans-serif")
                .attr("pointer-events", "none")
                .attr("text-shadow", "0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff")
                .attr("text-anchor", "middle")
                .text(d => d.name)

            // graph.selectAll("text")
            //     .data(simulation.node2())
            //     .enter().append("text")
            //     .attr("x", 0)
            //     .attr("y", ".31em")
            //     .attr("font", "10px sans-serif")
            //     .attr("pointer-events", "none")
            //     .attr("text-shadow", "0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff")
            //     .attr("text-anchor", "middle")
            //     .text(d => d.name)
            
            simulation.alpha(1).restart();
        },
        zoomed() {
            const transform = d3.event.transform;
            const translate = transform.x % (this.gridSize * transform.k) + ',' +
                transform.y % (this.gridSize * transform.k) 
            this.selections.grid.attr('transform', 'translate(' +
                translate + ') scale(' + transform.k + ')');
            this.selections.graph.attr('transform', transform)

            const graphBox = this.selections.graph.node().getBBox()
            const margin = 200
            const worldTopLeft = [graphBox.x - margin, graphBox.y - margin]
            const worldBottomRight = [
                graphBox.x + graphBox.width + margin,
                graphBox.y + graphBox.height + margin
            ]
            this.zoom.translateExtent([worldTopLeft, worldBottomRight])
        }

    },

    computed: {
        // node2() {return this.graphdata.node2},
        nodes() { return this.graphdata.nodes; },
        links() { return this.graphdata.linkes; }
    }

}
</script>

<style scoped>

  path.link {
    fill: none;
    stroke: #666;
    stroke-width: 1.5px;
  }
  path.link.depends {
    stroke: #005900;
    stroke-dasharray: 5, 2;
  }
  path.link.needs {
    stroke: #7f3f00;
  }

  circle {
    fill: #ffff99;
    stroke: #191900;
    stroke-width: 1.5px;
  }
  circle.system {
    fill: #cce5ff;
    stroke: #003366;
  }
  circle.mount {
    fill: #ffe5e5;
    stroke: #660000;
  }
  circle.init {
    fill: #b2e8b2;
    stroke: #001900;
  }

  text {
    font: 10px sans-serif;
    pointer-events: none;
    text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
  }

</style>