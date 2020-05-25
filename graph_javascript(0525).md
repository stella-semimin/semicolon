```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- Styles -->
<style>
#chartdiv {
  width: 100%;
  height: 500px;
}
</style>
</head>
<body>

<!-- Resources -->
<script src="https://www.amcharts.com/lib/4/core.js"></script>
<script src="https://www.amcharts.com/lib/4/charts.js"></script>
<script src="https://www.amcharts.com/lib/4/themes/animated.js"></script>

<!-- Chart code -->
<script>
am4core.ready(function() {

// Themes begin
am4core.useTheme(am4themes_animated);
// Themes end

var chart = am4core.create("chartdiv", am4charts.XYChart);
chart.padding(40, 40, 40, 40);

chart.numberFormatter.bigNumberPrefixes = [
  { "number": 1e+3, "suffix": "K" },
  { "number": 1e+6, "suffix": "M" },
  { "number": 1e+9, "suffix": "B" }
];

var label = chart.plotContainer.createChild(am4core.Label);
label.x = am4core.percent(97);
label.y = am4core.percent(95);
label.horizontalCenter = "right";
label.verticalCenter = "middle";
label.dx = -15;
label.fontSize = 50;

var playButton = chart.plotContainer.createChild(am4core.PlayButton);
playButton.x = am4core.percent(97);
playButton.y = am4core.percent(95);
playButton.dy = -2;
playButton.verticalCenter = "middle";
playButton.events.on("toggled", function(event) {
  if (event.target.isActive) {
    play();
  }
  else {
    stop();
  }
})

var stepDuration = 3000;

var categoryAxis = chart.yAxes.push(new am4charts.CategoryAxis());
categoryAxis.renderer.grid.template.location = 0;
categoryAxis.dataFields.category = "network";
categoryAxis.renderer.minGridDistance = 1;
categoryAxis.renderer.inversed = true;
categoryAxis.renderer.grid.template.disabled = true;

var valueAxis = chart.xAxes.push(new am4charts.ValueAxis());
valueAxis.min = 0;
valueAxis.rangeChangeEasing = am4core.ease.linear;
valueAxis.rangeChangeDuration = stepDuration;
valueAxis.extraMax = 0.1;

var series = chart.series.push(new am4charts.ColumnSeries());
series.dataFields.categoryY = "network";
series.dataFields.valueX = "MAU";
series.tooltipText = "{valueX.value}"
series.columns.template.strokeOpacity = 0;
series.columns.template.column.cornerRadiusBottomRight = 5;
series.columns.template.column.cornerRadiusTopRight = 5;
series.interpolationDuration = stepDuration;
series.interpolationEasing = am4core.ease.linear;

var labelBullet = series.bullets.push(new am4charts.LabelBullet())
labelBullet.label.horizontalCenter = "right";
labelBullet.label.text = "{values.valueX.workingValue.formatNumber('#.0as')}";
labelBullet.label.textAlign = "end";
labelBullet.label.dx = -10;

chart.zoomOutButton.disabled = true;

// as by default columns of the same series are of the same color, we add adapter which takes colors from chart.colors color set
series.columns.template.adapter.add("fill", function(fill, target){
  return chart.colors.getIndex(target.dataItem.index);
});

//시작하는 년도
var year = 2010;
label.text = year.toString();

var interval;

function play() {
  interval = setInterval(function(){
    nextYear();
  }, stepDuration)
  nextYear();
}

function stop() {
  if (interval) {
    clearInterval(interval);
  }
}

function nextYear() {
  year++

  //2010~2020년을 보여주기 위함이니까, 2020년까지 끝나면 2010년으로 다시 가줘
  if (year > 2020) {
    year = 2010;
  }

  var newData = allData[year];
  var itemsWithNonZero = 0;
  for (var i = 0; i < chart.data.length; i++) {
    chart.data[i].MAU = newData[i].MAU;
    if (chart.data[i].MAU > 0) {
      itemsWithNonZero++;
    }
  }

  if (year == 2010) {
    series.interpolationDuration = stepDuration / 4;
    valueAxis.rangeChangeDuration = stepDuration / 4;
  }
  else {
    series.interpolationDuration = stepDuration;
    valueAxis.rangeChangeDuration = stepDuration;
  }

  chart.invalidateRawData();
  label.text = year.toString();

  categoryAxis.zoom({ start: 0, end: itemsWithNonZero / categoryAxis.dataItems.length });
}


categoryAxis.sortBySeries = series;

var allData = {
  "2010": [
    {
      "network": "아이파크145.05",
      "MAU": 19993
    },
    {
      "network": "아이파크156.86.",
      "MAU": 19062
    },
    {
      "network": "아이파크167.72",
      "MAU": 0
    },
    {
      "network": "아이파크175.05",
      "MAU": 0
    },

    {
      "network": "아이파크195.39",
      "MAU": 19832.5
    },
    {
      "network": "아이파크222.6",
      "MAU": 0
    }
  ],
  "2011": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 0
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 16851.33	      
	    }, 
	    {
	      "network": "아이파크167.72",
	      "MAU": 0
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 18709
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 15354
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2012": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 15873.75
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 13601.50
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 20788.67
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 20566.00
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 20216
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2013": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 15270.5
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 14815.80
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 14606.33
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 0
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 20352.37
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2014": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 16080.5
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 15054.43
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 17887
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 0
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 19081.5
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2015": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 17323.08
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 16473.20
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 17499.50
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 19216.00
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 21128.73
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2016": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 18445.4
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 17749.50
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 19258.25
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 22065.25
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 24310.50
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2017": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 23992
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 19542.73
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 20298.12
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 23193.33
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 21879.50
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2018": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 21762.5
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 21994.00
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 26652.00
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 24678.500
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 29343.000
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ],
  "2019": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 25363.6
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 23364.500
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 0
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 27135.500
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 24975.670
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 30891.000
	    }
  ],
  "2020": [
	  {
	      "network": "아이파크145.05",
	      "MAU": 0
	    },
	    {
	        "network": "아이파크156.86.",
	        "MAU": 26680.000
	      },
	    {
	      "network": "아이파크167.72",
	      "MAU": 0
	    },
	    {
	      "network": "아이파크175.05",
	      "MAU": 0
	    },

	    {
	      "network": "아이파크195.39",
	      "MAU": 29172.000
	    },
	    {
	      "network": "아이파크222.6",
	      "MAU": 0
	    }
  ]
}

chart.data = JSON.parse(JSON.stringify(allData[year]));
categoryAxis.zoom({ start: 0, end: 1 / chart.data.length });

series.events.on("inited", function() {
  setTimeout(function() {
    playButton.isActive = true; // this starts interval
  }, 2000)
})

}); // end am4core.ready()
</script>

<!-- HTML -->
<div id="chartdiv"></div>
</body>
</html>

```

