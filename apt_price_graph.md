

0527 그래프 1, 개별아파트

1. bullet ) 0527/거래량.csv

2. x축= yymm, y축=실거래가(단위: 백만원) // cf. x축을 yymmdd까지 넣어도 bullet 처리가능. 어떤게 더 좋은지는 실제 db넣은후에 확인해야할것 같음.

3. ay : 범례1의 실거래가, by = 범례2의 실거래가.... cy, dy, ey..... (평수별로 구현하기)

4. 실제 거래가 없는 달이 있는데 아래 예시처럼 그런경우에 값을 아예 안넣으면 그래프상 표현잘됨.(0으로 넣으면 0으로 표현되기 때문에 안됨!!)

   

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
  width: 50%;
  height: 300px;
}
</style>

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

// Create chart instance
var chart = am4core.create("chartdiv", am4charts.XYChart);

// Add data
chart.data = [{
  "date": "2010-06",
  "by": 420,
  "aValue": 1,
  "bValue": 1
} ,
{"date": "2010-12",
  "by": 415,
  "aValue": 5,
  "bValue": 0
} ,
{
  "date": "2011-02",
  "by": 382,
  "aValue": 0,
  "bValue": 1
} ,{
  "date": "2011-05",
  "ay": 435,
  "aValue": 4,
  "bValue": 3
} , {
  "date": "2012-01",
  "ay": 387,
  "by": 400,
  "aValue": 8,
  "bValue": 3
} , {
  "date": "2012-05",
  "ay": 373,
  "aValue": 16,
  "bValue": 4
},{
  "date": "2013-01",
  "ay": 340,
  "by": 347.5,
  "aValue": 15,
  "bValue": 10
},{
  "date": "2013-05",
  "ay": 360,
  "by": 347.5,
  "aValue": 0,
  "bValue": 7
},{
  "date": "2013-08",
  "ay": 355,
  "by": 370,
  "aValue": 5,
  "bValue": 5
	} ,
{
  "date": "2013-09",
  "ay": 370,
  "aValue": 15,
  "bValue": 10
} ,{
  "date": "2014-07",
  "by": 366,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2014-09",
  "ay": 422,
  "by": 422,
  "aValue": 15,
  "bValue": 10
} ,{
  "date": "2015-05",
  "ay": 398,
  "by": 431.5,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2015-08",
  "ay": 433,
  "by": 448,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2015-09",
  "ay": 450,
  "by": 437.333,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2015-10",
  "ay": 433,
  "by": 434.5,
  "aValue": 15,
  "bValue": 10
} ,{
  "date": "2016-08",
  "ay": 493,
  "by": 492,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2016-10",
  "ay": 500,
  "by": 507.5,
  "aValue": 1,
  "bValue": 10
} ,
{
  "date": "2017-04",
  "ay": 525,
  "by": 505,
  "aValue": 5,
  "bValue": 0
} ,{
  "date": "2017-05",
  "ay": 520,
  "by": 517,
  "aValue": 7,
  "bValue": 1
} ,{
  "date": "2017-11",
  "ay": 550,
  "by": 560,
  "aValue": 11,
  "bValue": 4
} ,
{
  "date": "2018-09",
  "ay": 767,
  "aValue": 5,
  "bValue": 1
} ,
{
  "date": "2018-10",
  "ay": 710,
  "by": 772,
  "aValue": 15,
  "bValue": 10
} ,
{
  "date": "2019-03",
  "ay": 740,
  "aValue": 15,
  "bValue": 10
} ,
{
 "date": "2019-06",
 "ay": 750,
 "aValue": 15,
 "bValue": 10
},
{
 "date": "2020-02",
 "by": 755,
 "aValue": 15,
 "bValue": 10
	}];

// Create axes
var dateAxis = chart.xAxes.push(new am4charts.DateAxis());
dateAxis.renderer.grid.template.location = 0;
dateAxis.renderer.minGridDistance = 50;
dateAxis.renderer.grid.template.disabled = true;
dateAxis.renderer.fullWidthTooltip = true;

// Create value axis
var yAxis = chart.yAxes.push(new am4charts.ValueAxis());

// Create series
var series1 = chart.series.push(new am4charts.LineSeries());
series1.dataFields.dateX = "date";
series1.dataFields.valueY = "ay";
series1.dataFields.value = "aValue";
series1.strokeWidth = 2;

var bullet1 = series1.bullets.push(new am4charts.CircleBullet());
series1.heatRules.push({
  target: bullet1.circle,
  min: 0,
  max: 5,
  property: "radius"
});

bullet1.tooltipText = "{date} 평균실거래가: [bold]{valueY}[/]";

var series2 = chart.series.push(new am4charts.LineSeries());
series2.dataFields.dateX = "date";
series2.dataFields.valueY = "by";
series2.dataFields.value = "bValue";
series2.strokeWidth = 2;

var bullet2 = series2.bullets.push(new am4charts.CircleBullet());
series2.heatRules.push({
  target: bullet2.circle,
  min: 0,
  max: 5,
  property: "radius"
});

bullet2.tooltipText = "{date} 평균실거래가: [bold]{valueY}[/]";

//scrollbars
chart.scrollbarX = new am4core.Scrollbar();
chart.scrollbarY = new am4core.Scrollbar();

}); // end am4core.ready()
</script>

<!-- HTML -->
<div id="chartdiv"></div>
</body>
</html>

```

