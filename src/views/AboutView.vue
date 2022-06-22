<template>
  <div class="about">
    <h1>Аналитика</h1>
      <div class="hello" ref="chartdiv" id="chartdiv">
      </div>
  </div>
</template>


<script>
import * as am5 from '@amcharts/amcharts5';
import * as am5xy from '@amcharts/amcharts5/xy';
import am5themes_Animated from '@amcharts/amcharts5/themes/Animated';

export default {
  name: 'AboutView',
  beforeCreate(){
    let storageCheck=localStorage.getItem('Leadhit-Site-Id');
    if(!storageCheck){
      window.open('/','_top');
    }
  },
    mounted() {
    var root = am5.Root.new("chartdiv");

// Set themes
// https://www.amcharts.com/docs/v5/concepts/themes/
root.setThemes([
  am5themes_Animated.new(root)
]);

var data = [
  { date: new Date(2020, 7, 1).getTime(), value: 213 },
  { date: new Date(2020, 7, 2).getTime(), value: 249 },
  { date: new Date(2020, 7, 3).getTime(), value: 179 },
  { date: new Date(2020, 7, 4).getTime(), value: 170 },
  { date: new Date(2020, 7, 5).getTime(), value: 184 },
  { date: new Date(2020, 7, 6).getTime(), value: 202 },
  { date: new Date(2020, 7, 7).getTime(), value: 198 },
  { date: new Date(2020, 7, 8).getTime(), value: 168 },
  { date: new Date(2020, 7, 9).getTime(), value: 176 },
  { date: new Date(2020, 7, 10).getTime(), value: 171 },
  { date: new Date(2020, 7, 11).getTime(), value: 190 },
  { date: new Date(2020, 7, 12).getTime(), value: 154 },
  { date: new Date(2020, 7, 13).getTime(), value: 246 },
  { date: new Date(2020, 7, 14).getTime(), value: 250 },
  { date: new Date(2020, 7, 15).getTime(), value: 227 },
  { date: new Date(2020, 7, 16).getTime(), value: 140 },
  { date: new Date(2020, 7, 17).getTime(), value: 170 },
  { date: new Date(2020, 7, 18).getTime(), value: 125 },
  { date: new Date(2020, 7, 19).getTime(), value: 106 },
  { date: new Date(2020, 7, 20).getTime(), value: 207 },
  { date: new Date(2020, 7, 21).getTime(), value: 222 },
  { date: new Date(2020, 7, 22).getTime(), value: 198 },
  { date: new Date(2020, 7, 23).getTime(), value: 204 },
  { date: new Date(2020, 7, 24).getTime(), value: 213 },
  { date: new Date(2020, 7, 25).getTime(), value: 145 },
  { date: new Date(2020, 7, 26).getTime(), value: 166 },
  { date: new Date(2020, 7, 27).getTime(), value: 163 },
  { date: new Date(2020, 7, 28).getTime(), value: 135 },
  { date: new Date(2020, 7, 29).getTime(), value: 45 },


];

// Create chart
// https://www.amcharts.com/docs/v5/charts/xy-chart/
var chart = root.container.children.push(
  am5xy.XYChart.new(root, {
    focusable: true,
    panX: true,
    panY: true,
    wheelX: "none",
    wheelY: "none"
  })
);

// Create curtain + message to show when wheel is used over chart without CTRL
var overlay = root.container.children.push(am5.Container.new(root, {
  width: am5.p100,
  height: am5.p100,
  layer: 100,
  visible: false
}));
                                           
var curtain = overlay.children.push(am5.Rectangle.new(root, {
  width: am5.p100,
  height: am5.p100,
  fill: am5.color(0x000000),
  fillOpacity: 0.3
}));


var message = overlay.children.push(am5.Label.new(root, {
  text: "Use CTRL + Scroll to zoom",
  fontSize: 30,
  x: am5.p50,
  y: am5.p50,
  centerX: am5.p50,
  centerY: am5.p50
}));
console.log(curtain);
console.log(message);


chart.plotContainer.events.on("wheel", function(ev) {
  // Show overlay when wheel is used over chart
  if (ev.originalEvent.ctrlKey) {
    ev.originalEvent.preventDefault();
    chart.set("wheelX", "panX");
    chart.set("wheelY", "zoomX");
  }
  else {
    chart.set("wheelX", "none");
    chart.set("wheelY", "none");
    overlay.show();
    overlay.setTimeout(function() {
      overlay.hide()
    }, 800);
  }
});

// Create axes
// https://www.amcharts.com/docs/v5/charts/xy-chart/axes/
var xAxis = chart.xAxes.push(
  am5xy.DateAxis.new(root, {
    maxDeviation: 0.1,
    groupData: false,
    baseInterval: {
      timeUnit: "day",
      count: 1
    },
    renderer: am5xy.AxisRendererX.new(root, {
      minGridDistance: 50
    }),
    tooltip: am5.Tooltip.new(root, {})
  })
);

var yAxis = chart.yAxes.push(
  am5xy.ValueAxis.new(root, {
    maxDeviation: 0.1,
    renderer: am5xy.AxisRendererY.new(root, {})
  })
);

// Add series
// https://www.amcharts.com/docs/v5/charts/xy-chart/series/
var series = chart.series.push(
  am5xy.LineSeries.new(root, {
    minBulletDistance: 10,
    xAxis: xAxis,
    yAxis: yAxis,
    valueYField: "value",
    valueXField: "date"
  })
);

series.data.setAll(data);

var tooltip = am5.Tooltip.new(root, {
  pointerOrientation: "horizontal"
});
tooltip.label.set("text", "{valueY}");
series.set("tooltip", tooltip);

series.bullets.push(function () {
  return am5.Bullet.new(root, {
    sprite: am5.Circle.new(root, {
      radius: 5,
      fill: series.get("fill"),
      stroke: root.interfaceColors.get("background"),
      strokeWidth: 2
    })
  });
});

// Add cursor
// https://www.amcharts.com/docs/v5/charts/xy-chart/cursor/
var cursor = chart.set("cursor", am5xy.XYCursor.new(root, {
  xAxis: xAxis
}));
cursor.lineY.set("visible", false);

// add scrollbar
chart.set("scrollbarX", am5.Scrollbar.new(root, {
  orientation: "horizontal"
}));

// Make stuff animate on load
// https://www.amcharts.com/docs/v5/concepts/animations/
series.appear(1000, 100);
chart.appear(1000, 100);
  },
   beforeUnmount() {
    if (this.root) {
      this.root.dispose();
    }
  },

  methods:{

  }
}
</script>
<style >
body {
  font-family: -apple-system,
    BlinkMacSystemFont, "Segoe UI",
    Roboto, Helvetica, Arial, sans-serif, 
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

#chartdiv {
  width: 100%;
  height: 95vh;
  margin-bottom: 100vh;
}
.about{
  background-color: azure;
}
</style>