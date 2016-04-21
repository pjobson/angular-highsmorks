# Highsmorks Angular Directive

I was not very happy with how the highcharts-ng, which uses its own pretty much undocumented version of **chartConfig** for some bizarre reason.

The directive is called Highsmorks, only to create a fun unique name.

## References

I highly recommend you go through the examples and API to learn how to use **chartConfig**.

HighStock API - http://api.highcharts.com/highstock
HighStock Demos - http://www.highcharts.com/stock/demo

HighChart API - http://api.highcharts.com/highcharts
HighChart Demos - http://www.highcharts.com/demo

## HTML

Setting up a chart is easy.

	<highsmorks id="chart1" chart-config="{{chartConfig}}" type="StockChart"></highsmorks>

Note "chart-config" this translates to **chartConfig** on the angular side.

## JavaScript

To setup the chart, you will need to create a **chartConfig**. The **chartConfig** I'm using is exactly the same as defined in the reference.

Here is a very simple one for example:

	$scope.chartConfig = {
		title: {
			text: 'Chart Title'
		},
		subtitle: {
			text: 'Chart Subtitle'
		},
		xAxis: {
			gapGridLineWidth: 0
		},
		scrollbar: {
			enabled: false
		},
		navigator: {
			enabled: false
		},
		rangeSelector: {
			enabled: false
		},
		series : [{
			name : 'Series Name',
			data: [[1420088400000,0.89],[1388552400000,47.56],[1357016400000,39.29]],
			tooltip: {
				valueDecimals: 2
			}
		}]
	};

## $broadcast Methods

There is two ways to pass in data to the **chartConfig** or you can use a $broadcast method if you are loading the data via AJAX.

### addSeries

This adds a series to a chart, the object is as defined in the HighStocks API.

	$scope.$broadcast('highsmorks',{
		id: 'chart1',
		method: 'addSeries',
		obj: {
			data: [[1420088400000,0.89],[1388552400000,47.56],[1357016400000,39.29],[1325394000000,25.16],[1293858000000,7.73]],
			name: 'ytds',
			type: 'area',
			gapSize: 5,
			tooltip: {
				valueDecimals: 2,
				valueSuffix: '%'
			}
		}
	});

### setTitle

Sets the title and/or subtitle dynamically.

	$scope.$broadcast('highsmorks',{
		id: 'chart1',
		method: 'setTitle',
		obj: {
			title: 'New Title',
			subtitle: 'New Subtitle'
		}
	});

### redraw

Redraws the chart.

	$scope.$broadcast('highsmorks',{
		id: 'chart1',
		method: 'redraw'
	});

### addPoint

Adds another point to a specific series.

	$scope.$broadcast('highsmorks',{
		id: 'chart1',
		method: 'addPoint',
		seriesName: 'ytds',
		obj: {
			x: 1520088400000,
			y: -25.0
		}
	});

## Themes

If you want all of your charts in your app to follow the same theme, you may use a theme file along with the setOptions() method.

Example:

	// Global Theme for Highcharts
	Highcharts.theme = {
		colors: ['#50B432', '#ED561B', '#DDDF00', '#24CBE5', '#64E572', '#FF9655', '#FFF263', '#6AF9C4'],
		credits: { enabled: false }
	};
	// Apply the theme with setOptions().
	Highcharts.setOptions(Highcharts.theme);

Comprehensive instructions are available from highcharts directly.

http://www.highcharts.com/docs/chart-design-and-style/themes

## Examples

**Doughnut chart for Market Capitalization**

	$scope.marketCapConfig = {
		chart: {
			plotBackgroundColor: null,
			plotBorderWidth: 0,
			plotShadow: false
		},
		title: {
			text: 'Market<br/>Capitalization',
			align: 'center',
			verticalAlign: 'middle',
			y: -15
		},
		tooltip: {
			pointFormat: '{series.name}: <b>{point.percentage:.1f}%</b>'
		},
		plotOptions: {
			pie: {
				dataLabels: {
					enabled: true,
					distance: -50,
					style: {
						fontWeight: 'bold',
						color: 'white',
						textShadow: '0px 1px 2px black'
					}
				},
				startAngle: 0,
				endAngle: 360,
				center: ['50%', '50%']
			}
		},
		series: [{
			type: 'pie',
			name: 'Account Percent',
			innerSize: '50%',
			data: [
				['Small Cap',   10.38],
				['Other',       56.33],
				['Large Cap',   24.03],
				['Other',        9.26]
			]
		}]
	};

**Simple Line Chart**

Data gets added in later by using the broadcast method addSeries above.

	$scope.chartCfg = {
		title: {
			text: ''
		},
		subtitle: {
			text: ''
		},
		xAxis: {
			gapGridLineWidth: 0
		},
		scrollbar: {
			enabled: false
		},
		navigator: {
			enabled: false
		},
		rangeSelector: {
			enabled: false
		}
	};

	// Later after ytds data is generated from an AJAX call.
	$scope.$broadcast('highsmorks',{
		id: account.number,
		method: 'addSeries',
		obj: {
			data: ytds,
			name: 'ytds',
			type: 'area',
			gapSize: 5,
			tooltip: {
				valueDecimals: 2,
				valueSuffix: '%'
			}
		}
	});


