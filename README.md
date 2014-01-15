# BEMSimpleLineGraph - Feature Branch
<p align="center"><img src="http://img843.imageshack.us/img843/3821/ru8f.png"/></p>	

<p align="center">
<b>BEMSimpleLineGraph</b> makes it easy to create and customize line graphs for iOS.
</p>

## Feature Branch Warning
You are currently viewing the feature branch of this GitHub repository. The feature branch contains bleeding edge commits / features, otherwise known as **alpha or beta** features. Content in this branch may be unstable, bug-ridden, non-working, and undocumented. The sole purpose of this branch is to test and improve on new features that may or may not be included in future (stable) versions.

It is not recommended that you use this branch in a production app of any kind. **For stable production ready code**, please refer to the [master branch](https://github.com/Sam-Spencer/BEMSimpleLineGraph/tree/master).

## Project Details
Learn more about the BEMSimpleLineGraph project requirements, licensing, and contributions.

### Requirements
- Requires iOS 6 or later. The sample project is optimized for iOS 7.
- Requires Automatic Reference Counting (ARC).
- Optimized for ARM64 Architecture

### License
See the [License](https://github.com/Boris-Em/BEMSimpleLineGraph/blob/master/LICENSE.md). You are free to make changes and use this in either personal or commercial projects. Attribution is not required, but it appreciated. A little Thanks! (or something to that affect) would be much appreciated. If you use BEMSimpleLineGraph in your app, let us know. 

### Contributions
Any contribution is more than welcome! You can contribute through pull requests and issues on GitHub. 

### Sample App
The iOS Sample App included with this project demonstrates how to correctly setup and use BEMSimpleLineGraph. You can refer to the sample app for an understanding of how to use and setup BEMSimpleLineGraph.

## Documentation
All methods, properties, types, and delegate methods available on the BEMSimpleLineGraph class are documented below. If you're using Xcode 5 with BEMSimpleLineGraph, documentation is available directly within Xcode (just Option-Click any method for Quick Help).

### Installation
The easiest way to install BEMSimpleLineGraph is to use <a href="http://cocoapods.org/" target="_blank">CocoaPods</a>. To do so, simply add the following line to your `Podfile`:
	<pre><code>pod BEMSimpleLineGraph</code></pre>
	
The other way to install BEMSimpleLineGraph, is to drag and drop the *Classes* folder into your Xcode project. When you do so, check the "*Copy items into destination group's folder*" box.

### Setup
Setting up BEMSimpleLineGraph in your project is simple. If you're familiar with UITableView, then BEMSimpleLineGraph should be a breeze. Follow the steps below to get everything up and running.

 1. Import `"BEMSimpleLineGraphView.h"` to the header of your view controller:

         #import "BEMSimpleLineGraphView.h"

 2. Implement the `BEMSimpleLineGraphDelegate` to the same view controller:

         @interface YourViewController : UIViewController <BEMSimpleLineGraphDelegate>

 3.  BEMSimpleLineGraphView can be initialized in one of two ways. You can either add it directly to your interface (storyboard file) OR through code. Both ways provide the same initialization, just different ways to do the same thing. Use the method that makes sense for your app or project.

     **Interface Initialization**  
     1 - Add a UIView to your UIViewController  
     2 - Change the class type of the UIView to `BEMSimpleLineGraphView`  
     3 - Link the view to your code using an `IBOutlet`. You can set the property to `weak` and `nonatomic`.  
     4 - Select the Connect the `BEMSimpleLineGraphView` in your interface. Connect the delegate property to your ViewController.  

     **Code Initialization**  
     Just add the following code to your implementation (usually the `viewDidLoad` method).

         BEMSimpleLineGraphView *myGraph = [[BEMSimpleLineGraphView alloc] initWithFrame:CGRectMake(0, 0, 320, 200)];
         myGraph.delegate = self;
         [self.view addSubview:myGraph];

 4. Implement the two required methods: `numberOfPointsInLineGraph:` and `lineGraph:valueForPointAtIndex:`. See documentation below for more information

### Required Delegate / Data Source Methods

**Number of Points in Graph**  
Returns the number of points in the line graph. The line graph gets the value returned by this method from its data source and caches it.

    - (int)numberOfPointsInLineGraph:(BEMSimpleLineGraphView *)graph {
    		return X; // Number of points in the graph.
    }

**Value for Point at Index**  
Informs the position of each point on the Y-Axis at a given index. This method is called for every point specifed in the `numberOfPointsInLineGraph:` method. The parameter `index` is the position from left to right of the point on the X-Axis:

	- (float)lineGraph:(BEMSimpleLineGraphView *)graph valueForPointAtIndex:(NSInteger)index {
    		return …; // The value of the point on the Y-Axis for the index.
	}

### Reloading the Data Source
Similar to a UITableView's `reloadData` method, BEMSimpleLineGraph has a `reloadGraph` method. Call this method to reload all the data that is used to construct the graph, including points, axis, index arrays, colors, alphas, and so on. Calling this method will cause the line graph to call `layoutSubviews` on itself. The line graph will also call all of its data source and delegate methods again (to get the updated data).

    - (void)anyMethodInYourOwnController {
        // Change graph properties
        // Update data source / arrays
        
        // Reload the graph
        [self.myGraph reloadGraph];
    }

### Retrieving the Data Source
The values submitted through the data source delegate methods are saved / recorded. You can retrieve the line graph's current data points and X-Axis with the two methods below.

    // Retrieve current X-Axis values
    NSArray *arrayOfXLabels = [self.myGraph graphValuesForXAxis]; // The returned NSArray contains an array of NSString objects
    
    // Retrieve the Data Points
    NSArray *arrayOfDataPoints = [self.myGraph graphValuesForDataPoints]; // The returned NSArray contains an array of NSNumber objects (originally formatted as a float).

### Data Source Calculations
In addition to recording and displaying data, BEMSimpleLineGraph can also perform advanced calculations with your data. All calculation methods are available publically and begin with `calculate`. BEMSimpleLineGraph can calculate Standard Deviation, Average, Median, Mode, Minimum, and Maximum values.

### Touch Reporting
BEMSimpleLineGraph makes it possible to react to the user touching the graph. 

<p align="center"><img src="http://img30.imageshack.us/img30/4479/gt3s.png"/></p>
<p align="center"> When the user touches and moves his finger along the graph, the labels on top of the graph indicate the value of the closest point. </p>

To do so, first toggle the property  `enableTouchReport` property:

	self.myGraph.enableTouchReport = YES;

Next, implement the two following methods: `lineGraph:didTouchGraphWithClosestIndex` and `lineGraph:didReleaseTouchFromGraphWithClosestIndex:`.

**Did Touch Graph at Index**  
This method gets called when the user touches the graph. The parameter `index` is the closest index (X-Axis) from the user's finger position.

	- (void)lineGraph:(BEMSimpleLineGraphView *)graph didTouchGraphWithClosestIndex:(int)index {
		// Here you could change the text of a UILabel with the value of the closest index for example.
	}

**Did Release Touch at Graph Index**  
This method gets called when the user stops touching the graph. The parameter `index` is the closest index (X-Axis) from the user's last finger position.

	- (void)lineGraph:(BEMSimpleLineGraphView *)graph didReleaseTouchFromGraphWithClosestIndex:(float)index {
		// Set the UIlabel alpha to 0 for example.
	}

### X-Axis Labels
BEMSimpleLineGraph makes it possible to add labels along the X-Axis. To do so, simply implement the two followings methods: `numberOfGapsBetweenLabels` and `labelOnXAxisForIndex:`.

**Gaps between labels**  
Informs how much empty space is needed between each displayed label. Returning 0 will display all of the labels. Returning the total number of labels will only display the first and last label. See the image below for clarification.

	- (int)numberOfGapsBetweenLabelsOnLineGraph:(BEMSimpleLineGraphView *)graph {
		return X; // The number of hidden labels between each displayed label.
	}
	
<p align="center"><img src="http://img838.imageshack.us/img838/9329/tz01.png"/></p>	

<p align="center"> On the left, <tt>numberOfGapsBetweenLabelsOnLineGraph:</tt> returns 0, on the middle it returns 1 and on the right it returns the number of points in the graph. </p>

**Label on X Axis**  
The text to be displayed for each UILabel on the X-Axis at a given index. It should return as many strings as the number of points on the graph.

	- (NSString *)lineGraph:(BEMSimpleLineGraphView *)graph labelOnXAxisForIndex:(NSInteger)index {
		return X;
	}

**X-Axis Label Color**  
The property `colorXaxisLabel` controls the color of the text of the UILabels on the X-Axis:

	@property (strong, nonatomic) UIColor *colorXaxisLabel;

### Line Customization
Two delegate methods on the `BEMSimpleLineGraphDelegate` let you customize the color and alpha of a specifc line in the graph.

**Line Color**
Specify the color of the line at a specific index (the graph is made of multiple lines that appear as one).

    - (UIColor *)lineGraph:(BEMSimpleLineGraphView *)graph lineColorForIndex:(NSInteger)index {
        return [UIColor color];
    }

**Line Alpha**
Specify the alpha value of the line at a specific index (the graph is made of multiple lines that appear as one).

    - (float)lineGraph:(BEMSimpleLineGraphView *)graph lineAlphaForIndex:(NSInteger)index {
        return 1.0;
    }

### Properties
BEMSimpleLineGraphs can be customized by using various properties. A variety of properties let you control the animation, colors, and alpha of the graph.

**Entrance Animation**  
The `animationGraphEntranceSpeed` property controls the speed of the entrance animation. It is an NSInteger who's value should be between 0 and 10.

A value of 0 will disable the animation. A value of 1 will make the animation very slow. A value of 10 or more will make it fast.

<p align="center"><img src="http://img819.imageshack.us/img819/4290/3vs.gif"/></p>

**Custom Colors and Alpha**  
BEMSimpleLineGraphs are split into three parts - the top, the bottom, and the line. You can set the alpha and color of each of these parts separately.

 * Top Section. The `colorTop` and `alphaTop` properties control the color (UIColor) and alpha (float) of the top part of the graph.  
 * Bottom Section. The `colorBottom` and `alphaBottom` properties control the color (UIColor) and alpha (float) of the bottom part of the graph.  
 * Line. The `colorLine` and `alphaLine` properties control the color (UIColor) and alpha (float) of the line of the graph. The `widthLine` property controls the width of the line of graph (float that defaults to 1.0).

### Graph Snapshots
On iOS 7.0 and above you can take a snapshot of the line graph view and get a UIImage representation of the snapshot. To do so, simply call the method below at anytime in the graph's lifecycle. Note that the snapshot is not of the completed graph, but of the graph in its current state (whether it is in mid-animation or not). You can use the `lineGraphDidFinishLoading:` delegate method (coming soon) to find out when the graph has finished rendering and animating.

    // Method Definition
    - (UIImage *)graphSnapshotImage;
    
    // Method Usage
    UIImage *imageOfGraph = [self.myGraph graphSnapshotImage];
