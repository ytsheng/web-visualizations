# Open mHealth Web Visualizations

The Open mHealth Web Visualization library renders visualizations of Open mHealth structured data in a web browser.
It currently generates line and bar charts. Default settings are included for the following measures:

* body weight (`body_weight`)
* heart rate (`heart_rate`)
* blood pressure (`systolic_blood_pressure`,`diastolic_blood_pressure`)
* physical activity (`step_count`, `minutes_moderate_activity`)

The charting functions of the library are built on top of [Plottable.js](http://plottablejs.org/), which is built on top of [D3](http://d3js.org/).

You can play with a demo [here](http://jsfiddle.net/jasperspeicher/dremvboo/3/).

##Installation

To run the library,

* Install [Node.js](https://docs.npmjs.com/getting-started/installing-node), which comes with [npm](https://www.npmjs.com/)
* Install [Bower](http://bower.io/) to manage this library's dependencies
    * `npm install -g bower`
    * If you get a permission error, use `sudo`
* Clone this repository
    * `git clone https://github.com/openmhealth/web-visualizations.git`
* Navigate to the cloned repository and install the project's dependencies with Bower
    * `bower install`
* Open `charts.html` in the `demo` directory

If you want to modify or contribute to this library,

* Navigate to the cloned repository and install the development dependencies using npm
    * `npm install`
* Make your changes
* To publish your changes to the `dist` directory, run [gulp](http://gulpjs.com/)
    * `gulp`
* To let gulp watch for changes in the background and update `dist` as needed
    * `gulp watch`

##Building a chart

You can create a chart by calling:

```javascript
chart = new OMHWebVisualizations.Chart( data, element, measureList, options );
```

The arguments passed to the constructor are:

Argument | Description
---: | ---
*data* | An array of Open mHealth structured data points.
*element* | A dom element, such as a `<div>` containing an `<svg>` node. This can also be a jQuery object.
*measureList* | A string containing a comma-separated list of Open mHealth measures to display.
*options* | An object with configuration options for the chart. Can be left off or passed an empty object.

The easiest way to get some data points is to use our [sample data generator](https://github.com/openmhealth/sample-data-generator). You can either use a pre-generated [data set](https://github.com/openmhealth/sample-data-generator/releases/download/v1.0.0/one-year-of-data.json.gz), or download the generator itself to create data that fits your needs.

##Configurating a chart

Below is an example of the options object that can be passed into the `OMHWebVisualization.Chart(...)` function shown above. The settings below are the defaults used when a sparse, or empty, options object is passed in. You can specify any subset of these options:

```javascript
{
  'userInterface': {
    'toolbar': { 'enabled': true },
    'timespanButtons': { 'enabled': true },
    'zoomButtons': { 'enabled': true },
    'navigation': { 'enabled': true },
    'tooltips': {
      'enabled': true,
      'timeFormat': 'M/D/YY, h:mma'
     },
    'panZoom': {
      'enabled': true,
      'showHint': true
    },
  },
  'measures': {
    'body_weight' : {
      'valueKeyPath': 'body.body_weight.value',
      'range': { 'min':0, 'max':100 },
      'units': 'kg',
      'thresholds': { 'max':57 },
    },
    'heart_rate': {
      'valueKeyPath': 'body.heart_rate.value',
      'range': { 'min':30, 'max':150 },
      'units': 'bpm',
    },
    'step_count': {
      'valueKeyPath': 'body.step_count',
      'range': { 'min':0, 'max':1500 },
      'units': 'Steps',
      'seriesName': 'Steps',
      'chart': {
        'type':'clustered_bar',
        'barColor' : '#eeeeee',
        'daysShownOnTimeline': { 'min':7, 'max':90 },
      },
    },
    'minutes_moderate_activity': {
      'valueKeyPath': 'body.minutes_moderate_activity.value',
      'range': { 'min':0, 'max':300 },
      'units': 'Minutes',
      'seriesName': 'Minutes of moderate activity',
      'chart': {
        'type':'clustered_bar',
        'daysShownOnTimeline': { 'min':7, 'max':90 },
      },
    },
    'systolic_blood_pressure': {
      'valueKeyPath': 'body.systolic_blood_pressure.value',
      'range': { 'min':30, 'max':200 },
      'units': 'mmHg',
      'thresholds':  { 'max':120 },
    },
    'diastolic_blood_pressure': {
      'valueKeyPath': 'body.diastolic_blood_pressure.value',
      'range': { 'min':30, 'max':200 },
      'units': 'mmHg',
      'thresholds':  { 'max':80 },
    }
  }
}
```

In addition to the options shown in the `measures` sections above, the following default options are used. They can be specified in the `measures` section of the options object passed to `OMHWebVisualization.Chart(...)` as well:

```javascript
{
  'range': { 'min':0, 'max':100 },
  'units': 'Units',
  'seriesName': 'Series',
  'chart': {
    'type':'line',
    'pointSize': 9,
    'lineColor' : '#dedede',
    'pointFillColor' : '#4a90e2',
    'pointStrokeColor' : '#0066d6',
    'aboveThesholdPointFillColor' : '#e8ac4e',
    'aboveThesholdPointStrokeColor' : '#745628',
    'barColor' : '#4a90e2',
    'daysShownOnTimeline': { 'min':1, 'max':1000 },
  },
}
```

So, using the default settings to graph `heart_rate` data, ie, passing `{}` as the options argument to `OMHWebVisualization.Chart(...)` will give a chart that looks something like the following screenshot:

![Configured Chart](http://www.openmhealth.org/media/viz_example_default_options.png "Default Chart")

If you would like to graph `heart_rate` data with a blue line and no tooltips, you may use the following configuration object:

```javascript
{
  'userInterface': {
    'tooltips': {
      'enabled': false,
     }
  },
  'measures': {
    'heart_rate': {
      'chart': {
        'lineColor' : '#4a90e2'
      }
    }
  }
}
```
This will produce a chart that looks something like the following screenshot:

![Configured Chart](http://www.openmhealth.org/media/viz_example_user_options.png "Configured Chart")

##Destroying a chart

In order to free up resources or re-use an element for a new chart, the chart and all of its interactive features can be destroyed with:

```javascript
chart.destroy();
```








