# Visualizing Sport Results

[![example of the triangle stacks visualization](https://plotparade.com/chartimg/triangle/triangle2.gif)](#)


This "Triangle Stacks" data visualization gives a recap of a match and shows the order in which teams scored. This works for several sport disciplines when teams or individuals win by scoring goals or points.

[![animation explanation drawing](https://krisztinaszucs.com/blog/20220829_gif/img/expanation-01.png)](#)

A demo with gif export is available at [https://plotparade.com/44_giftriangle/](https://plotparade.com/44_giftriangle/)


## Getting Started

To use this visualization, simply open the `triangle_stacks.html` file in a web browser. No web server is required.

The `data` array contains the progress of the match in terms of goals scored. You can edit the 3-letter country codes to show different teams.

```javascript
const TEAM1 = "BRA";
const TEAM2 = "CRO";

const data = [
  [0,1],
  [0,2],
  [1,2],
  [2,2],
  [2,3],
  [3,3],
  [3,4],
  [3,5],
  [4,5],
  [5,5],
  [6,5],
]
```

## How the Animation Works

[![explanation of the transition](https://plotparade.com/chartimg/triangle/explanation.png)](#)

The animation is built by adding simple triangles on top of one another. The starting shape appears as a simple line, but it is actually a polygon with three points, where point B and C are in the exact same position. With a transition, we move point C upwards to make the triangle appear. When the other team scores a goal, we add a new line created by three points on the edge of the previous triangle, covering the line between points A and C. Then, we move point F up in the same way as we did with point C.

```javascript
const triangles = {
  team1: {
    startPoly: [C1X, c1y, C2X, c2y, C1X, c1y],
    finalPoly: [C1X, c1y, C2X, c2y, C1X, COLUMNH - scaleY(d[0])],
    score: d[0],
  },
  team2: {
    startPoly: [C1X, c1y, C2X, c2y, C2X, c2y],
    finalPoly: [C1X, c1y, C2X, c2y, C2X, COLUMNH - scaleY(d[1])],
    score: d[1],
  }
};

changePolygon = function (whichTeamScored) {
  vizGroup.append("polygon")
    .attr("points", function () { return triangles[whichTeamScored].startPoly.join(","); })
    .attr("fill", colorSet[whichTeamScored])
    .attr("stroke", colorSet.bg)
    .attr("stroke-width", 3)
    .transition()
    .delay(ANIMTIMEBETWEENGOALS)
    .duration(TRIANIMDURATION)
    .ease(d3.easePolyOut.exponent(5))
    .attr("points", function () { return triangles[whichTeamScored].finalPoly.join(","); })
}
```

## Examples with Real Data

Read more about this type of visualization and see examples of it in use 
- [https://krisztinaszucs.com/blog/20220829_gif/](https://krisztinaszucs.com/blog/20220829_gif/)
- [https://krisztinaszucs.com/my-product/FIFA/](https://krisztinaszucs.com/my-product/FIFA/)


## What's Not Included

Please note that the following features are not included in this code:

- Penalty score visualization
- Gaps for halves, periods, quarters, overtime, etc.
- Visualizing points when two opponents score a point simultaneously (e.g., fencing).
