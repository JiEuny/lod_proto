<template>
<!--<div>-->
    <svg width="100%" height="100%">

    </svg>

</template>

<script>
import * as d3 from 'd3';

export default {
    name: 'DrawGraph',
    data() {
        return {
            svg: null,
            zoom: null,
            width: 2048,
            height: 768,
            graphdata: this.drawGraphData,
            simulation: null,
            selections: {},
            forceProperties: {
                center: {
                    x: 0.3,
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
                    radius: 45
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
                    distance: 200,
                    iterations: 1
                }
            },
            refX: 9, // for marker
            refY: -0.3,
            markerWidth: 10,
            markerHeight: 10,
            arrowPoints: '0,-5 10,0 0,5',
        }
    },
    props: {drawGraphData : {}},
    mounted() {
        this.selections.svg = d3.select("svg")
        this.selections.graph = this.selections.svg.append("g")

        this.zoom = d3.zoom()
            .scaleExtent([0.6, 10])
            .on('zoom', this.zoomed)

        this.selections.svg.call(this.zoom)

        this.simulation = d3.forceSimulation()
            .force("link", d3.forceLink().id(function(d) { return d.id; }))
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
                // console.log('-----transform-d', d);
                if(d.x && d.y)
                 return "translate(" + d.x + "," + d.y + ")";

            }

/*            const link = d => {
                // console.log('-link---dy', d.target);

                return "M" + d.source.x + "," + d.source.y + " L" + d.target.x + "," + d.target.y;
            }*/


            const link = d => {
                // console.log('-link---dy', d.target);

                var inter = this.pointOnRect(d.source.x, d.source.y,
                    d.target.x - 32, d.target.y - 32,
                    d.target.x + 32, d.target.y + 32);

                return "M" + d.source.x + "," + d.source.y + "L" + inter.x + "," + inter.y;

            }



            const graph = this.selections.graph
            graph.selectAll("path").attr("d", link)
            graph.selectAll("circle").attr("transform", transform)
            graph.selectAll("text").attr("transform", transform)
            // graph.selectAll("rect").attr("transform", transform)
        },
        pointOnRect(x, y, minX, minY, maxX, maxY, check) {
            //assert minX <= maxX;
            //assert minY <= maxY;
            if (check && (minX <= x && x <= maxX) && (minY <= y && y <= maxY))
                throw "Point " + [x, y] + "cannot be inside " + "the circle: " + [minX, minY] + " - " + [maxX, maxY] + ".";
            var midX = (minX + maxX) / 2;
            var midY = (minY + maxY) / 2;
            // if (midX - x == 0) -> m == ±Inf -> minYx/maxYx == x (because value / ±Inf = ±0)
            var m = (midY - y) / (midX - x);

            if (x <= midX) { // check "left" side
                var minXy = m * (minX - x) + y;
                if (minY <= minXy && minXy <= maxY)
                    return {
                        x: minX,
                        y: minXy
                    };
            }

            if (x >= midX) { // check "right" side
                var maxXy = m * (maxX - x) + y;
                if (minY <= maxXy && maxXy <= maxY)
                    return {
                        x: maxX,
                        y: maxXy
                    };
            }

            if (y <= midY) { // check "top" side
                var minYx = (minY - y) / m + x;
                if (minX <= minYx && minYx <= maxX)
                    return {
                        x: minYx,
                        y: minY
                    };
            }

            if (y >= midY) { // check "bottom" side
                var maxYx = (maxY - y) / m + x;
                if (minX <= maxYx && maxYx <= maxX)
                    return {
                        x: maxYx,
                        y: maxY
                    };
            }

            throw "Cannot find intersection for " + [x, y] + " inside circle " + [minX, minY] + " - " + [maxX, maxY] + ".";
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

             this.dragstarted = (d) => {
                if (!d3.event.active) simulation.alphaTarget(.03).restart();
                d.fx = d.x;
                d.fy = d.y;
            }

            this.dragged = (d) => {
                d.fx = d3.event.x;
                d.fy = d3.event.y;
            }

            this.dragended = (d) => {
                if (!d3.event.active) simulation.alphaTarget(.03);
                d.fx = null;
                d.fy = null;
            }

            this.simulation.nodes(this.nodes)
            this.simulation.force("link").links(this.links)
            //, d3.forceLink().id(function(d) { return d.id; }))

            const simulation = this.simulation;
            const graph = this.selections.graph;

            graph.selectAll("path")
                .data(simulation.force("link").links())
                .enter().append("path")
                .attr("class", d=> "link " + d.type).attr("id",function(d,i) { return "linkId_" + i; })
                .attr("stroke", "#06592a")
                .attr("marker-end", "url(#end)")
                .attr("stroke-width",1)
                // .attr("stroke-width", function(d) { return (d.target.value * 50); })
                // .attr('marker-start', 'url(#arrow)')
                .exit().remove();

            graph.selectAll("circle")
                .data(simulation.nodes())
                .enter().append("circle")
                .attr("r", 35)
                .attr("class", d => d.class)
                .attr("fill", "#c7c2d4")
                .attr("stroke", "#660000")
                .call(d3.drag()
                    .on("start", this.dragstarted)
                    .on("drag", this.dragged)
                    .on("end", this.dragended))
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
                .text(d => d.name);

            // Define the arrowhead marker variables
            graph.append("svg:defs").selectAll("marker")
                .data(["end"])
                .enter().append("svg:marker")
                .attr("id", String)
                .attr("viewBox", "0 -5 10 10")
                .attr("refX", this.refX)
                .attr("refY", this.refY)
                .attr("markerWidth", this.markerWidth)
                .attr("markerHeight", this.markerHeight)
                .attr("orient", "auto")
                .attr("stroke", "#06592a")
                .append("svg:polyline")
                .attr("points", this.arrowPoints);

            graph.selectAll("linklabel")
                .data(simulation.force("link").links())
                .enter().append("text")
                .attr("class","linklabel")
                .attr("dx",65)
                .attr("dy",0)
                .style("fill","#7fb906")
                .append("textPath")
                .attr("href",function(d,i) { return "#linkId_" + i;})
                .text(function(d) { return d.type});

            simulation.alpha(1).restart();
        },
        zoomed() {
            const transform = d3.event.transform;
            const translate = transform.x % (transform.k) + ',' +
                transform.y % (transform.k)
            this.selections.svg.attr('transform', 'translate(' +
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
