<template>
    <div :style="{ width: width + 'px', height: height + 'px' }">
        <!-- <div>
            <img style="margin:80px 80px 100px 80px" src="./../assets/graph.png"/>
        </div> -->
        <!-- <svg width="50" height="50">
            <circle :id="circle" cx="25" cy="25" r="25"  />
            <line x1="5" y1="5" x2="40" y2="40" stroke="gray" stroke-width="5"  />

    </svg> -->

        <svg width="100%" height="100%"></svg>


    </div>

</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'Graph',
  data() {
      return {
        circle: null,
        graph: null,
        width: 1024,
        height: 768,
        selections: {},
        simulation: null,
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
        data: {
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
    "links": [
        {
            "source": 0,
            "target": 1
        },
        {
            "source": 1,
            "target": 2
        }
    ]
        }
      }
  },
//   methods: {

    //   render() {
    //     const svg_width = 1000;
    //     const svg_height = 600;

    //     const svg = d3.select("svg")
    //         .attr("width", svg_width)
    //         .attr("height", svg_height);

    //     // this.graph = svg.append("g")
    //     //     .attr("transform", 'translate(${margin}, ${margin})');
        
    //     this.circle = svg.append("circle")
    //         .attr("cx", 30).attr("cy", 30).attr("r", 20);
    //   }
    
//   },
  computed: {
      nodes() { return this.data.nodes; },
      links() { return this.data.links; },
  },
  mounted() {

      this.simulation = d3.forceSimulation(this.nodes)
        .force("link", d3.forceLink())
        .force("charge", d3.forceManyBody())
        .force("collide", d3.forceCollide())
        .force("center", d3.forceCenter())
        .force("forceX", d3.forceX())
        .force("forceY", d3.forceY())
        .on("tick", this.tick)
      this.updateForces()

      this.selections.svg = d3.select("svg")
      this.selections.graph = this.selections.svg.append("g")
      this.graph = this.selections.graph

      this.circle = this.selections.svg.append("circle")
            .attr("cx", 30).attr("cy", 30).attr("r", 20);

    this.selections.svg.attr("class", "nodes")
        .selectAll("circle")
        .data(this.nodes)
        .enter().append("circle")
        .attr("r", 20)
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
    
    d3.json("/data.json").then(data => {
        this.data = data
        
      });

    //   this.render();
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
      updateData() {
        this.simulation.nodes(this.nodes)
        this.simulation.force("link").links(this.links)

        const simulation = this.simulation
        const graph = this.selections.graph

        graph.selectAll("path")
          .data(simulation.force("link").links())
        .enter().append("path")
          .attr("class", d => "link " + d.type)
        .exit().remove()

        graph.selectAll("circle")
          .data(simulation.nodes())
        .enter().append("circle")
          .attr("r", 30)
          .attr("class", d => d.class)
        .exit().remove()

        graph.selectAll("text")
          .data(simulation.nodes())
        .enter().append("text")
          .attr("x", 0)
          .attr("y", ".31em")
          .attr("text-anchor", "middle")
          .text(d => d.name)

        simulation.alpha(1).restart();
      },
      updateForces() {
        const { simulation, forceProperties, width, height } = this
        simulation.force("center")
        .x(width * forceProperties.center.x)
        .y(height * forceProperties.center.y);
        simulation.force("charge")
          .strength(forceProperties.charge.strength * forceProperties.charge.enabled)
          .distanceMin(forceProperties.charge.distanceMin)
          .distanceMax(forceProperties.charge.distanceMax);
        simulation.force("collide")
          .strength(forceProperties.collide.strength * forceProperties.collide.enabled)
          .radius(forceProperties.collide.radius)
          .iterations(forceProperties.collide.iterations);
        simulation.force("forceX")
          .strength(forceProperties.forceX.strength * forceProperties.forceX.enabled)
          .x(width * forceProperties.forceX.x);
        simulation.force("forceY")
          .strength(forceProperties.forceY.strength * forceProperties.forceY.enabled)
          .y(height * forceProperties.forceY.y);
        simulation.force("link")
          .distance(forceProperties.link.distance)
          .iterations(forceProperties.link.iterations);

        // updates ignored until this is run
        // restarts the simulation (important if simulation has already slowed down)
        simulation.alpha(1).restart();
      }

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