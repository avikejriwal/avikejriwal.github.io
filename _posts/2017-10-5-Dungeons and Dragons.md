---
layout: post
title: The Statistics of Dungeons and Dragons, 5th edition

---

<head>
    <script type="text/javascript"
            src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>
</head>

So, I recently started playing Dungeons and Dragons.  The game involves determining virtually every outcome through dice rolls. Unsurprisingly, this yields a lot of room for probabilistic analysis.

### Rolling Base Stats

When building a new character, there are 6 base stats that are calculated:
- Strength  
- Dexterity  
- Constitution  
- Intelligence  
- Wisdom  
- Charisma

Each stat determines different abilities, and every class values different stats.

There are three ways to calculate base stats:  
- [27 point buy](http://1-dot-encounter-planner.appspot.com/point-buy-calculator.html)  
- Allocate from the standard array: 15, 14, 13, 12, 10, 8
- for each stat, roll four 6-sided dice (4d6) and take the sum of the highest 3

I was curious about the balance between the three options.  The first two options make for building a character are fairly straightforward.  Evenly distributing the points with the 27-point buy yields an average stat value of 12.5, while the standard array yields an average of 12.  Also, note that the standard array can be generated using the 27-point buy.

The third option is a bit trickier to analyze.  Fortunately, using [Anydice](http://anydice.com/program/13e), I could simulate the distribution of outcomes.  It yields a mean of 12.24, with a mode at 13.  In essence, you can expect a similar stat distribution with this method compared to the 27-point buy.

However, the 27-point buy system sets a hard limit on how many points can be allocated towards a single stat (stats at first level can't be set to higher than 15 or lower than 8).

### Advantage and Disadvantage

When playing Dungeons and Dragons, most challenges (attacks, ability checks and saving throws) will be resolved with a d20 roll (a 20-sided dice).  The expected value for a single (fair) 20-sided dice is simple to compute: 10.5.

However, sometimes a player will have advantage on a d20 roll, in which case they roll two d20's and take the higher roll.  On the flipside, disadvantage forces a player to take the lower value from two d20 rolls.  Of course these will shift the probability distributions and the expected rolls, but by how much?

Again, using [Anydice](http://anydice.com/program/b38), a roll with advantage has a distribution where the PMF increases linearly with the value of the roll, yielding a 9.75% chance of rolling a natural 20.  With [disadvantage](http://anydice.com/program/1227), the PMF is virtually the same, but sloping in the opposite direction, yielding a 9.75% chance for rolling a natural 1.

The expected value in each case shifts by about 3.33.

Exact probabilities:  
- Normal:
$$
\begin{equation}
P(x \ge n) = \frac{21-n}{20}
\end{equation}
$$  
- Advantage:
$$
\begin{equation}
P(x \ge n) = \frac{-n^2 + 2n + 399}{400}
\end{equation}
$$  
- Disadvantage:
$$
\begin{equation}
P(x \ge n) = \frac{n^2 - 42n + 441}{400}
\end{equation}
$$  

I've made a little visualization ([original credit](http://bl.ocks.org/tjdecke/5558084)) that shows how success probabilities vary with ability modifiers, Difficulty Class (DC), and advantage/disadvantage.  The DC describes the d20 roll (before adding modifiers) required to succeed in a particular task
(resisting poison or magical effects, avoiding a hostile creature, finding food, etc.).

One thing should be noted: In many games, a natural 20 is always considered a success and a natural 1 a failure, regardless of the DC.  This is why the probability is always strictly greater than 0, but strictly less than 1.

<meta charset="utf-8">
<html>
  <head>
    <style>
      rect.bordered {
        stroke: #E6E6E6;
        stroke-width:2px;
      }
      text.mono {
        font-size: 9pt;
        font-family: Consolas, courier;
        fill: #aaa;
      }
      text.axis-workweek {
        fill: #000;
      }
      text.axis-worktime {
        fill: #000;
      }
    </style>
    <script src="https://d3js.org/d3.v4.js"></script>
    <script src="https://d3js.org/colorbrewer.v1.min.js"></script>
  </head>
  <body>
    <div id="chart"></div>
    <div id="dataset-picker">
    </div>
    <script type="text/javascript">
      const margin = { top: 50, right: 0, bottom: 50, left: 60 },
          width = 960 - margin.left - margin.right,
          height = 620 - margin.top - margin.bottom,
          gridSize = Math.floor(width / 24),
          legendElementWidth = gridSize*2,
          buckets = 9,
          colors = colorbrewer.YlGnBu[buckets]
          mod = [11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, -1],
          DC = [5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25];
          datasets = ["/d3/Disadvantage.tsv", "/d3/Normal.tsv", "/d3/Advantage.tsv"];

      const svg = d3.select("#chart").append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

      const modLabels = svg.selectAll(".modLabel")
          .data(mod)
          .enter().append("text")
            .text(function (d) { return d; })
            .attr("x", 0)
            .attr("y", (d, i) => i * gridSize)
            .style("text-anchor", "end")
            .attr("transform", "translate(-6," + gridSize / 1.5 + ")")
            .attr("class", (d, i) => ((i >= 0 && i <= 4) ? "modLabel mono axis axis-workweek" : "modLabel mono axis"));

      const modAxis = svg
          .append("text")
            .text("Ability Modifier")
            .attr("x", -30)
            .attr("y", gridSize/2)
            .style("text-anchor", "end")
            .attr("transform", "translate(-50," + gridSize*4 + ")rotate(-90)");

      const dcLabels = svg.selectAll(".dcLabel")
          .data(DC)
          .enter().append("text")
            .text((d) => d)
            .attr("x", (d, i) => i * gridSize)
            .attr("y", 0)
            .style("text-anchor", "middle")
            .attr("transform", "translate(" + gridSize / 2 + ", -6)")
            .attr("class", (d, i) => ((i >= 7 && i <= 16) ? "dcLabel mono axis axis-worktime" : "dcLabel mono axis"));

      const dcAxis = svg
          .append("text")
            .text("Difficulty Class (DC)")
            .attr("x", 0)
            .attr("y", gridSize/2)
            .style("text-anchor", "end")
            .attr("transform", "translate(475," + -50 + ")");

      const type = (d) => {
        return {
          mod: +d.mod,
          dc: +d.dc,
          prob: +d.prob
        };
      };

      const heatmapChart = function(tsvFile) {
        d3.tsv(tsvFile, type, (error, data) => {
          const colorScale = d3.scaleQuantile()
            .domain([0, 1.1])
            .range(colors);

          const cards = svg.selectAll(".hour")
              .data(data, (d) => d.mod+':'+d.dc);

          cards.append("title");

          cards.enter().append("rect")
              .attr("x", (d) => (d.dc - 4 -1) * gridSize)
              .attr("y", (d) => (d.mod +1 -1) * gridSize)
              .attr("rx", 4)
              .attr("ry", 4)
              .attr("class", "hour bordered")
              .attr("width", gridSize)
              .attr("height", gridSize)
              .style("fill", colors[0])
            .merge(cards)
              .transition()
              .duration(1000)
              .style("fill", (d) => colorScale(d.prob));

          cards.select("title").text((d) => d.prob);

          cards.exit().remove();

          const legend = svg.selectAll(".legend")
              .data([0].concat(colorScale.quantiles()), (d) => d);

          const legend_g = legend.enter().append("g")
              .attr("class", "legend");

          legend_g.append("rect")
            .attr("x", (d, i) => legendElementWidth * i)
            .attr("y", height)
            .attr("width", legendElementWidth)
            .attr("height", gridSize / 2)
            .style("fill", (d, i) => colors[i]);

          legend_g.append("text")
            .attr("class", "mono")
            .text((d) => "≥ " + Math.round(d*1000)/1000)
            .attr("x", (d, i) => legendElementWidth * i)
            .attr("y", height + gridSize);

          legend.exit().remove();
        });
      };

      heatmapChart(datasets[0]);

      const datasetpicker = d3.select("#dataset-picker")
        .selectAll(".dataset-button")
        .data(datasets);

      datasetpicker.enter()
        .append("input")
        .attr("type", "button")
        .attr("value", function(d) { return d.slice(4,-4);})
        .attr("class", "dataset-button")
        .on("click", (d) => heatmapChart(d));

    </script>
  </body>
</html>

 <br>

### Great Weapon Master/Sharpshooter

Great Weapon Master (GWM) and Sharpshooter are optional abilities which enable characters to sacrifice accuracy for additional damage.  Generally, when declaring an attack, the attack hits if the d20 roll (plus the player's attack modifier) equals or exceeds the target's armor class (AC).  With these feats, a player can take a -5 penalty to their roll in exchange for +10 damage.

The expected damage for each case is as follows (assuming no advantage/disadvantage):  
- With GWM/Sharpshooter: $$
\begin{equation}
\frac{(damage+10)(16-AC+modifier)}{20}
\end{equation}
$$  
- Without GWM/Sharpshooter: $$
\begin{equation}
\frac{(damage)(21-AC+modifier)}{20}
\end{equation}
$$  

The question becomes: When should a player choose to use it?

By setting the two expected values equal and applying some tedious algebra, we get the following:

$$
\begin{equation}
AC = 16+modifier - \frac{damage}{2}
\end{equation}
$$  

This describes the maximum AC for which it is beneficial to utilize the GWM/Sharpshooter damage bonus.  Beyond this threshold, it is better to attack normally.  Intuitively a higher attack modifier means a higher threshold, as the chance of an attack landing is higher.  

What is not as intuitive is the penalty associated with having higher expected damage (without the ability).  I interpret is as being an opportunity cost; The higher one's base damage, the more damage output they risk losing by taking the -5 penalty.

With Advantage/Disadvantage, the probability distribution for a successful hit changes, so this formula doesn't necessarily apply.

### Parting words

["I'm attacking the darkness!"](https://www.youtube.com/watch?v=-leYc4oC83E)

Update: [Interesting read](https://fivethirtyeight.com/features/is-your-dd-character-rare/)

<!---License for the D3 visualization
Copyright (c) 2016, Tom May

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
-->
