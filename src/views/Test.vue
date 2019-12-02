<template>
    <div>

        <div >
            <svg width="100%" height="100%">

            </svg>
        </div>
    </div>
</template>

<script>
import * as d3 from 'd3';

export default {
    name: 'Test',
    data() {
        return {
            svg: null,
            // links: [],
            // nodes: [],
            graph: {
                "nodes": [
                    {
                        "id": "Alice"
                    },
                    {
                        "id": "Bob"
                    },
                    {
                        "id": "Carol"
                    }
                ],
                "linkes": [
                    {
                        "source": 0,
                        "target": 1
                    },
                    {
                        "source": 1,
                        "target": 2
                    }
                ]
            },
            circle: null,
            line: null,
            simulation: {},
            // color: d3.scaleOrdinal(d3.schemeCategory20),
            settings: {
            //     strokeColor: "#29B5FF",
            //     width: 100,
                svgWigth: 960,
                svgHeight: 600
            }
        }
    },
    mounted() {
        // d3.json("/data.json", function(error, graph) {
            // if(error) throw error;
            // that.graph = graph;
            // this.simulation = d3.forceSimulation(this.graph.nodes)
            //     .force("link", d3.forceLink(this.graph.links).distance(100).strength(0.1))
            //     .force("charge", d3.forceManyBody())
            //     .force("center", d3.forceCenter(this.settings.svgWigth / 2, this.settings.svgHeight / 2));
        // })
        this.svg = d3.select("svg")
        
        // this.simulation.alphaTarget(0.3).restart();
        // this.circle = this.svg.append("g").attr("class", "nodes").selectAll("circle").data(this.graph.nodes).enter().append("circle").attr("r", 20)
        // this.line = this.svg.append("g").attr("class", "links").selectAll("line").data(this.graph.links).enter().append("line")
        //                     .attr("x1", 100).attr("y1", 100).attr("x2", 300).attr("y2", 300).attr("stroke-width", 50000).attr("stroke", "black")
            // .call(d3.drag()

            //             .on("start", this.dragstarted
            //             .on("drag", this.dragged)
            //             .on("end", this.dragended)))
        // this.dragging(this.circle)
        // this.svg.append("g").attr("class", "nodes").selectAll("circle").data(this.graph.nodes).enter().append("circle").attr("r", 20)
        // this.simulation.nodes()
        this.makeline()
    },
    methods: {
        makeline() {
            // this.svg.append("circle").attr("cx", 30).attr("cy", 30).attr("r", 20);
            // this.svg.append("line").attr("x1", 50).attr("y1", 50).attr("x2", 100).attr("y2", 100).attr("stroke-width", 2).attr("stroke", "black");
            this.simulation = d3.forceSimulation(this.graph.nodes)
            .force("link", d3.forceLink(this.graph.links).distance(100).strength(0.1))
            .force("charge", d3.forceManyBody())
            .force("center", d3.forceCenter(this.settings.svgWigth / 2, this.settings.svgHeight / 2));
            this.circle = this.svg.append("g").attr("class", "nodes").selectAll("circle").data(this.graph.nodes).enter().append("circle").attr("r", 20)
                .call(d3.drag()
                                .on("start", function dragstarted(d) {
                                    if (!d3.event.active) this.simulation.alphaTarget(0.3).restart();
                                    d.fx = d.x;
                                    d.fy = d.y;
                                })
                                .on("drag", function dragged(d) {
                                    d.fx = d3.event.x;
                                    d.fy = d3.event.y;
                                })
                                .on("end", function dragended(d) {
                                    if (!d3.event.active) this.simulation.alphaTarget(0);
                                    d.fx = null;
                                    d.fy = null;
                                }))
                // .call(d3.drag().on("start", this.nodeDragStarted).on("drag", this.dragged).on("end", this.dragended))
            this.line = this.svg.append("g").attr("class", "links").selectAll("line").data(this.graph.linkes).enter().append("line").attr("stroke-width", 50000)
                            // .attr("x1", 100).attr("y1", 100).attr("x2", 300).attr("y2", 300).attr("stroke-width", 50000).attr("stroke", "black")
                            // .attr("stroke-width", function (d) { return Math.sqrt(d.value)})
        },
        dragging(circle) {
            circle.call(d3.drag()
                        .on("start", this.dragstarted
                        .on("drag", this.dragged)
                        .on("end", this.dragended)))
        },
        nodeDragStarted(d) {
        if (!d3.event.active) { this.simulation.alphaTarget(0.3).restart(); }
        d.fx = d.x;
        d.fy = d.y;
      },
      dragged(d) {
                            d.fx = d3.event.x;
                            d.fy = d3.event.y;
                        },
        dragended(d) {
                            if (!d3.event.active) this.simulation.alphaTarget(0);
                            d.fx = null;
                            d.fy = null;
                        }
    },
    updated() {
        this.simulation.nodes(this.graph.nodes).on('tick', function ticked() {
                    this.circle
                        .attr("x1", function (d) { return d.source.x; })
                        .attr("y1", function (d) { return d.source.y; })
                        .attr("x2", function (d) { return d.target.x; })
                        .attr("y2", function (d) { return d.target.y; });

                    this.links
                        .attr("cx", function (d) { return d.x; })
                        .attr("cy", function (d) { return d.y; });
                })
    },
    computed: {
        node() {
            if (this.graph) {
                return this.svg.append("g").attr("class", "nodes").selectAll("circle").data(this.graph.nodes).enter().append("circle").attr("r", 20).call(d3.drag()
                        .on("start", this.dragstarted
                        .on("drag", this.dragged)
                        .on("end", this.dragended)))
            }
            else {
                return 0;
            }
        }
        // nodes: function() {
        //     // if (that.graph) {
        //         return this.svg.append("g")
        //             .attr("class", "nodes")
        //             .selectAll("circle")
        //             .data(that.graph.nodes)
        //             .enter().append("circle")
        //             .attr("r", 20)
        //             .attr("fill", function (d, i) {
        //                 return that.color(i);
        //             })
        //             .call(d3.drag()
        //                 .on("start", function dragstarted(d) {
        //                     if (!d3.event.active) that.simulation.alphaTarget(0.3).restart();
        //                     d.fx = d.x;
        //                     d.fy = d.y;
        //                 })
        //                 .on("drag", function dragged(d) {
        //                     d.fx = d3.event.x;
        //                     d.fy = d3.event.y;
        //                 })
        //                 .on("end", function dragended(d) {
        //                     if (!d3.event.active) that.simulation.alphaTarget(0);
        //                     d.fx = null;
        //                     d.fy = null;
        //                 }))
        //     // }
        // },
        // links: function() {
        //     // if(that.graph) {
        //         return this.svg.append("g")
        //             .attr("class", "links")
        //             .selectAll("line")
        //             .data(that.graph.links)
        //             .enter().append("line")
        //             .attr("stroke-width", function (d) { return Math.sqrt(d.value); });
        //     // }
        // }
    
            // updated: function () {

            //     that.simulation.nodes(that.graph.nodes).on('tick', function ticked() {
            //         that.links
            //             .attr("x1", function (d) { return d.source.x; })
            //             .attr("y1", function (d) { return d.source.y; })
            //             .attr("x2", function (d) { return d.target.x; })
            //             .attr("y2", function (d) { return d.target.y; });

            //         that.nodes
            //             .attr("cx", function (d) { return d.x; })
            //             .attr("cy", function (d) { return d.y; });
            //     });
            }
    
}
</script>

<style scoped>
.controls {
            position: fixed;
            top: 16px;
            left: 16px;
            background: #f8f8f8;
            padding: 0.5rem;
            display: flex;
            flex-direction: column;
        }

        .svg-container {
            display: table;
            border: 1px solid #f8f8f8;
            box-shadow: 1px 2px 4px rgba(0, 0, 0, .5);
        }

        .controls>*+* {
            margin-top: 1rem;
        }

        label {
            display: block;
        }

        .links line {
            stroke: #999;
            stroke-opacity: 0.6;
        }

        .nodes circle {
            stroke: #fff;
            stroke-width: 1.5px;
        }
</style>