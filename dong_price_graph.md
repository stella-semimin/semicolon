## 동별 그래프 그리기 



```
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

//차트값 끝에 접미사붙여줘, 
/* chart.numberFormatter.bigNumberPrefixes = [
  { "number": 1e+3, "suffix": "K" },
  { "number": 1e+6, "suffix": "M" },
  { "number": 1e+9, "suffix": "B" }
]; */

var label = chart.plotContainer.createChild(am4core.Label);
label.x = am4core.percent(97);
label.y = am4core.percent(95);
label.horizontalCenter = "right";
label.verticalCenter = "middle";
label.dx = -15;
label.fontSize = 50;

//play버튼 위치 및 작동
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
categoryAxis.dataFields.category = "dong";
categoryAxis.renderer.minGridDistance = 1;
categoryAxis.renderer.inversed = true;
categoryAxis.renderer.grid.template.disabled = true;

var valueAxis = chart.xAxes.push(new am4charts.ValueAxis());
valueAxis.min = 0;
valueAxis.rangeChangeEasing = am4core.ease.linear;
valueAxis.rangeChangeDuration = stepDuration;
valueAxis.extraMax = 0.1;

var series = chart.series.push(new am4charts.ColumnSeries());
series.dataFields.categoryY = "dong";
series.dataFields.valueX = "price";
series.tooltipText = "{valueX.value}"
series.columns.template.strokeOpacity = 0;
series.columns.template.column.cornerRadiusBottomRight = 5;
series.columns.template.column.cornerRadiusTopRight = 5;
series.interpolationDuration = stepDuration;
series.interpolationEasing = am4core.ease.linear;

//표에 해당 수치 표현하는 부분
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
    chart.data[i].price = newData[i].price;
    if (chart.data[i].price > 0) {
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
      "dong": "대치동",
      "price": 19993
    },
    {
      "dong": "역삼동",
      "price": 19062
    },
    {
      "dong": "도곡동",
      "price": 0
    },
    {
      "dong": "삼성동",
      "price": 0
    },

    {
      "dong": "압구정동",
      "price": 19832.5
    },
    {
      "dong": "신사동",
      "price": 0
    }
  ],
  "2011": [
	  {
	      "dong": "대치동",
	      "price": 0
	    },
	    {
	        "dong": "역삼동",
	        "price": 16851.33	      
	    }, 
	    {
	      "dong": "도곡동",
	      "price": 0
	    },
	    {
	      "dong": "삼성동",
	      "price": 18709
	    },

	    {
	      "dong": "압구정동",
	      "price": 15354
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2012": [
	  {
	      "dong": "대치동",
	      "price": 15873.75
	    },
	    {
	        "dong": "역삼동",
	        "price": 13601.50
	      },
	    {
	      "dong": "도곡동",
	      "price": 20788.67
	    },
	    {
	      "dong": "삼성동",
	      "price": 20566.00
	    },

	    {
	      "dong": "압구정동",
	      "price": 20216
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2013": [
	  {
	      "dong": "대치동",
	      "price": 15270.5
	    },
	    {
	        "dong": "역삼동",
	        "price": 14815.80
	      },
	    {
	      "dong": "도곡동",
	      "price": 14606.33
	    },
	    {
	      "dong": "삼성동",
	      "price": 0
	    },

	    {
	      "dong": "압구정동",
	      "price": 20352.37
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2014": [
	  {
	      "dong": "대치동",
	      "price": 16080.5
	    },
	    {
	        "dong": "역삼동",
	        "price": 15054.43
	      },
	    {
	      "dong": "도곡동",
	      "price": 17887
	    },
	    {
	      "dong": "삼성동",
	      "price": 0
	    },

	    {
	      "dong": "압구정동",
	      "price": 19081.5
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2015": [
	  {
	      "dong": "대치동",
	      "price": 17323.08
	    },
	    {
	        "dong": "역삼동",
	        "price": 16473.20
	      },
	    {
	      "dong": "도곡동",
	      "price": 17499.50
	    },
	    {
	      "dong": "삼성동",
	      "price": 19216.00
	    },

	    {
	      "dong": "압구정동",
	      "price": 21128.73
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2016": [
	  {
	      "dong": "대치동",
	      "price": 18445.4
	    },
	    {
	        "dong": "역삼동",
	        "price": 17749.50
	      },
	    {
	      "dong": "도곡동",
	      "price": 19258.25
	    },
	    {
	      "dong": "삼성동",
	      "price": 22065.25
	    },

	    {
	      "dong": "압구정동",
	      "price": 24310.50
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2017": [
	  {
	      "dong": "대치동",
	      "price": 23992
	    },
	    {
	        "dong": "역삼동",
	        "price": 19542.73
	      },
	    {
	      "dong": "도곡동",
	      "price": 20298.12
	    },
	    {
	      "dong": "삼성동",
	      "price": 23193.33
	    },

	    {
	      "dong": "압구정동",
	      "price": 21879.50
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2018": [
	  {
	      "dong": "대치동",
	      "price": 21762.5
	    },
	    {
	        "dong": "역삼동",
	        "price": 21994.00
	      },
	    {
	      "dong": "도곡동",
	      "price": 26652.00
	    },
	    {
	      "dong": "삼성동",
	      "price": 24678.500
	    },

	    {
	      "dong": "압구정동",
	      "price": 29343.000
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ],
  "2019": [
	  {
	      "dong": "대치동",
	      "price": 25363.6
	    },
	    {
	        "dong": "역삼동",
	        "price": 23364.500
	      },
	    {
	      "dong": "도곡동",
	      "price": 0
	    },
	    {
	      "dong": "삼성동",
	      "price": 27135.500
	    },

	    {
	      "dong": "압구정동",
	      "price": 24975.670
	    },
	    {
	      "dong": "신사동",
	      "price": 30891.000
	    }
  ],
  "2020": [
	  {
	      "dong": "대치동",
	      "price": 0
	    },
	    {
	        "dong": "역삼동",
	        "price": 26680.000
	      },
	    {
	      "dong": "도곡동",
	      "price": 0
	    },
	    {
	      "dong": "삼성동",
	      "price": 0
	    },

	    {
	      "dong": "압구정동",
	      "price": 29172.000
	    },
	    {
	      "dong": "신사동",
	      "price": 0
	    }
  ]
}

chart.data = JSON.parse(JSON.stringify(allData[year]));
categoryAxis.zoom({ start: 0, end: 1 / chart.data.length });

series.events.on("inited", function() {
  setTimeout(function() {
    playButton.isActive = true; // this starts interval
  }, 3000)
})

}); // end am4core.ready()
</script>

<!-- HTML -->
<div id="chartdiv"></div>
</body>
</html>

```

