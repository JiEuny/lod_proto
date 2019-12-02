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
            width: 2048,
            height: 768,
            graphdata: {
                "nodes":  [
                    { "name": "firmware" },
                    { "name": "loader"},
                    { "name": "kernel" },
                    { "name": "systemd" },
                    { "name": "mount"},
                    { "name": "init" },
                    { "name": "system"},
                    { "name": "system-getty" },
                    { "name": "systemd-initctl" },
                    { "name": "tmp" },
                    { "name": "sys-devices" },
                    { "name": "boot" }
                ],
                "linkes":  [
                    { "source":  1,  "target":  0},
                    { "source":  2,  "target":  1},
                    { "source":  3,  "target":  2 },
                    { "source":  4,  "target":  3},
                    { "source":  5,  "target":  3},
                    { "source":  6,  "target":  3 },
                    { "source":  7,  "target":  3 },
                    { "source":  8,  "target":  3 },
                    { "source":  9,  "target":  3 },
                    { "source": 11,  "target": 10 },
                    { "source": 11,  "target":  3 },
                    { "source": 11,  "target":  2 },
                    { "source": 11,  "target":  3 }
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
                .attr("stroke", "#005900")
                .exit().remove()

            graph.selectAll("circle")
                .data(simulation.nodes())
                .enter().append("circle")
                .attr("r", 30)
                .attr("class", d => d.class)
                .attr("fill", "#cce5ff")
                .attr("stroke", "#003366")
                .exit().remove()

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
            
            simulation.alpha(1).restart();
        },

    },

    computed: {
        nodes() { return this.graphdata.nodes; },
        links() { return this.graphdata.linkes; }
    }

}
</script>

<style scoped>

</style>