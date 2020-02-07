---
layout: post
title: Playing with D3

---

So, we tinkered with D3.js, a Javascript library for creating interactive plots.  Feel free to play around here.
<html>
<body>
<div id="chart"></div>

<script src="https://d3js.org/d3.v3.min.js"></script>
<script>

var width = 600,
    height = 600;

var nodes = d3.range(250).map(function() { return {radius: Math.random() * 4 + 8}; }),
    root = nodes[0],
    color = d3.scale.category10().range(["#FF0000", "#FFA500", "#FFFF00", "#00FF00" , "#0000FF", "#800080"]);

root.radius = 0;
root.fixed = true;

var force = d3.layout.force()
    .gravity(0.05)
    .charge(function(d, i) { return i ? 0 : -800; })
    .nodes(nodes)
    .size([width, height]);

force.start();

var svg = d3.select("#chart").append("svg")
    .attr("width", width)
    .attr("height", height);

svg.selectAll("circle")
    .data(nodes.slice(1))
  .enter().append("circle")
    .attr("r", function(d) { return d.radius; })
    .style("fill", function(d, i) { return color(i % 6); });

force.on("tick", function(e) {
  var q = d3.geom.quadtree(nodes),
      i = 0,
      n = nodes.length;

  while (++i < n) q.visit(collide(nodes[i]));

  svg.selectAll("circle")
      .attr("cx", function(d) { return d.x; })
      .attr("cy", function(d) { return d.y; });
});

svg.on("mousemove", function() {
  var p1 = d3.mouse(this);
  root.px = p1[0];
  root.py = p1[1];
  force.resume();
});

function collide(node) {
  var r = node.radius + 16,
      nx1 = node.x - r,
      nx2 = node.x + r,
      ny1 = node.y - r,
      ny2 = node.y + r;
  return function(quad, x1, y1, x2, y2) {
    if (quad.point && (quad.point !== node)) {
      var x = node.x - quad.point.x,
          y = node.y - quad.point.y,
          l = Math.sqrt(x * x + y * y),
          r = node.radius + quad.point.radius;
      if (l < r) {
        l = (l - r) / l * .5;
        node.x -= x *= l;
        node.y -= y *= l;
        quad.point.x += x;
        quad.point.y += y;
      }
    }
    return x1 > nx2 || x2 < nx1 || y1 > ny2 || y2 < ny1;
  };
}
</script>
</body>
</html>

Credit to [Mike Bostock](https://github.com/mbostock) for the original design, as well as for pretty much all of D3.
