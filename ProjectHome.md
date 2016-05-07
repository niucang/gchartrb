## Introduction ##
gchartrb is a Ruby wrapper around the Google chart API located at http://code.google.com/apis/chart/

It provides a nice object oriented interface with friendly names, and methods to populate data. Then the data is automatically encoded and the URL can be generated.

Features in the new release of Google Charts API are pending and should be implemented soon. Check out TodoList for a complete list.

## Download and Installation ##
The simplest way to install `gchartrb` is via Rubygems

```
gem install gchartrb
```

Alternatively you can download the latest packages from the [Download Page](http://code.google.com/p/gchartrb/downloads/list). Run/View the `lib/test.rb` file to get started.


## Documentation ##
A comprehensive README along with the API docs are located http://gchartrb.rubyforge.org

You can also look at the `lib/test.rb` ([link](http://gchartrb.googlecode.com/svn/trunk/lib/test.rb)) and `lib/example.rb` ([link](http://gchartrb.googlecode.com/svn/trunk/lib/example.rb)) in the package for a comprehensive set of examples.

Another source is the [gchartrb entries on Deepak's blog](http://blog.deepak.jois.name/search/label/gchartrb)

## Changes ##
**May 21 2008**
  * latest [commit](http://github.com/deepakjois/gchartrb/commit/0edba9041765e2217a3c20dd2ad48513d3baa609#L7L1) on github contains a new `test.rb` file which gives an indication of how things are going to change in the next release, syntaxwise. Docs are still pending.

**May 13 2008**
  * Massive rewrite ongoing. Follow at http://github.com/deepakjois/gchartrb/

**May 7 2008**
  * Moved primary development to github at http://github.com/deepakjois/gchartrb/tree . Watch out for implementation of new API features soon

**Mar 20 2008**
  * _Version 0.8_ released
  * Fixed Rakefile to process URL correctly (Thanks Aaron Blohowiak)
  * Fixed Axis error due to typo in base.rb (Thanks Jack Nutting)

**Feb 3 2008**
  * _Version 0.7_ released
  * Added fill area feature (see [fill\_area](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html#M000021))
  * Added [to\_escaped\_url](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html#M000013) method for encoding the URLs properly
  * Fixed bug where wrong variable name was being used in method

**Jan 27 2008**
  * _Version 0.6_ released
  * Added a new `lfi` chart type. See [FinancialLineChart](http://gchartrb.rubyforge.org/classes/GoogleChart/FinancialLineChart.html). (EXPERIMENTAL)
  * Added support for line styles, using [line\_style](http://gchartrb.rubyforge.org/classes/GoogleChart/LineChart.html#M000052) method
  * Added support for customisable width and spacing for bar charts, using [width\_spacing\_options](http://gchartrb.rubyforge.org/classes/GoogleChart/BarChart.html#M000009) method

**Jan 23 2008**
  * _Version 0.5.5_ released
  * Fixed a bug related to encoding of multiple datasets using extended encoding.

**Jan 07 2008**
  * _Version 0.5.4_ released
  * Fixed a bug related to data encoding in Stacked Charts
  * Fixed a bug in which title font and title font size values were being ignored
  * Fixed a bug where shape markers and range markers were overriding each other

**Dec 19 2007**
  * _Version 0.5.3_ released
  * Welcoming Aseem Tandon to the gchartrb team.
  * Added idiomatic Ruby syntax using blocks (see updated examples below)
  * Added support for shape markers. [RDoc documentation](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html#M000016)
**Dec 14 2007**
  * _Version 0.5.2_ released
  * Added support for title color via `title_color` attribute ([read Rdoc Docs](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html))
  * Added support for vertical and horizontal range markers ([read Rdoc docs](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html#M000015)) using the `range_marker` method
  * Removed show\_labels from base.rb and added it only for pie charts

**Dec 12 2007 (evening)**
  * _Version 0.5.1_ released
  * `max_value` method to allow setting of maximum values to encode against. See [RDoc documentation](http://gchartrb.rubyforge.org/classes/GoogleChart/Base.html#M000011) or code sample for scatter chart below for example.
  * Added an `lib/example.rb` to plot some graphs like at http://24ways.org/2007/tracking-christmas-cheer-with-google-charts

**Dec 12 2007**

  * _Version 0.5_ released
  * Scatter Charts support added
  * Documentation Updated

## Sample Usage ##
Here is some sample code
```
    require 'rubygems'
    require 'google_chart'

    # Pie Chart
    GoogleChart::PieChart.new('320x200', "Pie Chart",false) do |pc|
      pc.data "Apples", 40
      pc.data "Banana", 20
      pc.data "Peach", 30
      pc.data "Orange", 60
      puts "\nPie Chart"
      puts pc.to_url

      # Pie Chart with no labels
      pc.show_labels = false
      puts "\nPie Chart (with no labels)"
      puts pc.to_url  
    end


    # Line Chart
    GoogleChart::LineChart.new('320x200', "Line Chart", false) do |lc|
      lc.data "Trend 1", [5,4,3,1,3,5,6], '0000ff'
      lc.show_legend = true
      lc.data "Trend 2", [1,2,3,4,5,6], '00ff00'
      lc.data "Trend 3", [6,5,4,3,2,1], 'ff0000'
      lc.axis :y, :range => [0,6], :color => 'ff00ff', :font_size => 16, :alignment => :center
      lc.axis :x, :range => [0,6], :color => '00ffff', :font_size => 16, :alignment => :center
      lc.grid :x_step => 100.0/6.0, :y_step => 100.0/6.0, :length_segment => 1, :length_blank => 0
      puts "\nLine Chart"
      puts lc.to_url
    end

    # Bar Chart
    GoogleChart::BarChart.new('800x200', "Bar Chart", :vertical, false) do |bc|
      bc.data "Trend 1", [5,4,3,1,3,5], '0000ff' 
      bc.data "Trend 2", [1,2,3,4,5,6], 'ff0000'
      bc.data "Trend 3", [6,5,4,4,5,6], '00ff00'
      puts "\nBar Chart"
      puts bc.to_url
    end

    # Line XY Chart
    line_chart_xy = GoogleChart::LineChart.new('320x200', "Line XY Chart", true) do |lcxy|
      lcxy.data "Trend 1", [[1,1], [2,2], [3,3], [4,4]], '0000ff'
      lcxy.data "Trend 2", [[4,5], [2,2], [1,1], [3,4]], '00ff00'
      puts "\nLine XY Chart (inside a block)"
      puts lcxy.to_url   
    end

    # Venn Diagram
    # Supply three vd.data statements of label, size, color for circles A, B, C
    # Then, an :intersections with four values:
    # the first value specifies the area of A intersecting B
    # the second value specifies the area of B intersecting C
    # the third value specifies the area of C intersecting A
    # the fourth value specifies the area of A intersecting B intersecting C
    GoogleChart::VennDiagram.new("320x200", 'Venn Diagram') do |vd|
      vd.data "Blue", 100, '0000ff'
      vd.data "Green", 80, '00ff00'
      vd.data "Red",   60, 'ff0000'
      vd.intersections 30,30,30,10
      puts "\nVenn Diagram"
      puts vd.to_url
    end

    # Scatter Chart
    GoogleChart::ScatterChart.new('320x200',"Scatter Chart") do |sc|
      sc.data "Scatter Set", [[1,1,], [2,2], [3,3], [4,4]]
      sc.max_value [5,5] # Setting the max value
      sc.axis :x, :range => [0,5]
      sc.axis :y, :range => [0,5], :labels => [0,1,2,3,4,5]
      sc.point_sizes [10,15,30,55] # Optional
      puts "\nScatter Chart"
      puts sc.to_url
    end


    GoogleChart::LineChart.new('320x200', "Line Chart", false) do |lc|
      lc.data "Trend 1", [5,4,3,1,3,5,6], '0000ff'
      lc.show_legend = true
      lc.data "Trend 2", [1,2,3,4,5,6], '00ff00'
      lc.data "Trend 3", [6,5,4,3,2,1], 'ff0000'
      lc.axis :y, :range => [0,6], :color => 'ff00ff', :font_size => 16, :alignment => :center
      lc.axis :x, :range => [0,6], :color => '00ffff', :font_size => 16, :alignment => :center
      lc.grid :x_step => 100.0/6.0, :y_step => 100.0/6.0, :length_segment => 1, :length_blank => 0
      puts "\nLine Chart"
    end

    # Solid fill
    line_chart_xy.fill(:background, :solid, {:color => 'fff2cc'})
    line_chart_xy.fill(:chart, :solid, {:color => 'ffcccc'})
    puts "\nLine Chart with Solid Fill"
    puts line_chart_xy.to_url

    # Gradient fill
    line_chart_xy.fill :background, :gradient, :angle => 0,  :color => [['76A4FB',1],['ffffff',0]]
    line_chart_xy.fill :chart, :gradient, :angle => 0, :color => [['76A4FB',1], ['ffffff',0]]
    puts "\nLine Chart with Gradient Fill"
    puts line_chart_xy.to_url

    # Stripes Fill
    line_chart_xy.fill :chart, :stripes, :angle => 90, :color => [['76A4FB',0.2], ['ffffff',0.2]]
    puts "\nLine Chart with Stripes Fill"
    puts line_chart_xy.to_url

    puts "\nLine Chart with range markers and shape markers"  
    GoogleChart::LineChart.new('320x200', "Line Chart", false) do |lc|
      lc.title_color = 'ff00ff'
      lc.data "Trend 1", [5,4,3,1,3,5,6], '0000ff'
      lc.data "Trend 2", [1,2,3,4,5,6], '00ff00'
      lc.data "Trend 3", [6,5,4,3,2,1], 'ff0000'
      lc.max_value 10 # Setting max value for simple line chart 
      lc.range_marker :horizontal, :color => 'E5ECF9', :start_point => 0.1, :end_point => 0.5
      lc.range_marker :vertical, :color => 'a0bae9', :start_point => 0.1, :end_point => 0.5
      # Draw an arrow shape marker against lowest value in dataset
      lc.shape_marker :arrow, :color => '000000', :data_set_index => 0, :data_point_index => 3, :pixel_size => 10   
      puts lc.to_url
    end

```

Which generates the following as output (copy paste the URLs in the browser to view output)
```
Pie Chart
http://chart.apis.google.com/chart?cht=p&chl=Apples|Banana|Peach|Orange&chs=320x200&chd=s:oUe9&chtt=Pie+Chart

Pie Chart (with no labels)
http://chart.apis.google.com/chart?cht=p&chs=320x200&chd=s:oUe9&chtt=Pie+Chart

Line Chart
http://chart.apis.google.com/chart?chxt=y,x&cht=lc&chs=320x200&chco=0000ff,00ff00,ff0000&chdl=Trend+1|Trend+2|Trend+3&chd=s:yoeKey9,KUeoy9,9yoeUK&chxr=0,0,6|1,0,6&chtt=Line+Chart&chg=16.6666666666667,16.6666666666667,1.0,0.0&chxs=0,ff00ff,16,0|1,00ffff,16,0

Bar Chart
http://chart.apis.google.com/chart?cht=bvg&chs=800x200&chco=0000ff,ff0000,00ff00&chdl=Trend+1|Trend+2|Trend+3&chd=s:yoeKey,KUeoy9,9yooy9&chtt=Bar+Chart

Line XY Chart (inside a block)
http://chart.apis.google.com/chart?cht=lxy&chs=320x200&chco=0000ff,00ff00&chdl=Trend+1|Trend+2&chd=s:Pet9,MYkw,9ePt,9YMw&chtt=Line+XY+Chart

Venn Diagram
http://chart.apis.google.com/chart?cht=v&chs=320x200&chco=0000ff,00ff00,ff0000&chdl=Blue|Green|Red&chd=s:9wkSSSG&chtt=Venn+Diagram

Scatter Chart
http://chart.apis.google.com/chart?chxt=x,y&cht=s&chxl=1:|0|1|2|3|4|5&chs=320x200&chd=s:MYkw,MYkw,LQh9&chxr=0,0,5|1,0,5&chtt=Scatter+Chart

Line Chart

Line Chart with Solid Fill
http://chart.apis.google.com/chart?chf=bg,s,fff2cc|c,s,ffcccc&cht=lxy&chs=320x200&chco=0000ff,00ff00&chdl=Trend+1|Trend+2&chd=s:Pet9,MYkw,9ePt,9YMw&chtt=Line+XY+Chart

Line Chart with Gradient Fill
http://chart.apis.google.com/chart?chf=bg,lg,0,76A4FB,1,ffffff,0|c,lg,0,76A4FB,1,ffffff,0&cht=lxy&chs=320x200&chco=0000ff,00ff00&chdl=Trend+1|Trend+2&chd=s:Pet9,MYkw,9ePt,9YMw&chtt=Line+XY+Chart

Line Chart with Stripes Fill
http://chart.apis.google.com/chart?chf=bg,lg,0,76A4FB,1,ffffff,0|c,ls,90,76A4FB,0.2,ffffff,0.2&cht=lxy&chs=320x200&chco=0000ff,00ff00&chdl=Trend+1|Trend+2&chd=s:Pet9,MYkw,9ePt,9YMw&chtt=Line+XY+Chart

Line Chart with range markers and shape markers
http://chart.apis.google.com/chart?cht=lc&chs=320x200&chco=0000ff,00ff00,ff0000&chdl=Trend+1|Trend+2|Trend+3&chd=s:eYSGSek,GMSYek,keYSMG&chtt=Line+Chart&chts=ff00ff&chm=a,000000,0,3,10
```
