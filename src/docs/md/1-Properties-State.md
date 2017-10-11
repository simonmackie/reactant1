


# Working with Data in React: Properties & State

By Eric Greene

_This article is part of a web development series from Microsoft. Thank you for supporting the partners who make SitePoint possible._

<div class="header">
	# My Big Header
	</div>

**Managing data is essential to any application. Orchestrating the flow of data through the user interface (UI) of an application can be challenging. Often, today's web applications have complex UIs such that modifying the data in one area of the UI can directly and indirectly affect other areas of the UI. Two-way data binding via Knockout.js and Angular.js are popular solutions to this problem.**

For some applications (especially with a simple data flow), two-way binding can be a sufficient and quick solution. For more complex applications, two-way data binding can prove to be insufficient and a hindrance to effective UI design. React does not solve the larger problem of application data flow (although [Flux does][1]), but it does solve the problem of data flow within a single component.

Within the context of a single component, React solves both the problem of data flow, as well as updating the UI to reflect the results of the data flow. The second problem of UI updates is solved using a pattern named Reconciliation which involves innovative ideas such as a Virtual DOM. The next article will examine Reconciliation in detail. This article is focused on the first problem of data flow, and the kinds of data React uses within its components.

## Kinds of Component Data

Data within React Components is stored as either properties or state.

Properties are the input values to the component. They are used when rendering the component and initializing state (discussed shortly). After instantiating the component, properties should be considered immutable. Property values can only be set when instantiating the component, then when the component is re-rendered in the DOM, React will compare between the old and new property values to determine what DOM updates are required.

Here is a demonstration of setting the property values and updating the DOM in consideration of updated property values.

See the Pen [React.js Property Update Demo][2] by SitePoint ([@SitePoint][3]) on [CodePen][4].

State data can be changed by the component and is usually wired into the component's event handlers. Typically, updating the state triggers React components re-render themselves. Before a component is initialized, its state must be initialized. The initialized values can include constant values, as well as, property values (as mentioned above).

In comparison with frameworks such as Angular.js, properties can be thought of as one-way bound data, and state as two-way bound data. This is not a perfect analogy since Angular.js uses one kind of data object which is used two different ways, and React is using two data objects, each with their specific usages.

## Properties

My previous [React article][5] covered the syntax to specify and access properties. The article explored the use of JavaScript and JSX with static as well as dynamic properties in various code demonstrations. Expanding on the earlier exploration, let's look at some interesting details regarding working with properties.

When adding a CSS class name to a component, the property name `className` must be used, rather than `class` must be used. React requires this because ES2015 identifies the word `class` as a reserved keyword and is used for defining objects. To avoid confusion with this keyword, the property name `className` is used. If a property named `class` is used, React will display a helpful console message informing the developer that the property name needs to be changed to `className`.

Observe the incorrect `class` property name, and the helpful warning message displayed in the Microsoft Edge console window.

![Incorrect class property name][6]

Changing the `class` property to `className`, results in the warning message not being displayed.

![class property changed to ClassName][7]

When the property name is changed from `class` to `className` the warning message does not appear. See below for the complete CodePen demonstration.

See the Pen [React.js Class Property Demo][8] by SitePoint ([@SitePoint][9]) on [CodePen][10].

In addition to property names such as `className`, React properties have other interesting aspects. For example, mutating component properties is an anti-pattern. Properties can be set when instantiating a component, but should not be mutated afterwards. This includes changing properties after instantiating the component, as well as after rendering it. Mutating values within a component are considered state, and tracked with the `state` property rather than the `props` property.

In the following code sample, `SomeComponent` is instantiated with `createElement`, and then the property values are manipulated afterwards.

**JavaScript:**
    
    
    var someComponent = React.createElement(SomeComponent);
    
    someComponent.props.prop1 = "some value";
    someComponent.props.prop2 = "some value";
    

**JSX:**
    
    
    var someComponent = &lt;SomeComponent /&gt;;
    
    someComponent.props.prop1 = "some value";
    someComponent.props.prop2 = "some value";
    

Manipulating the `props` of the instantiated component could result in errors that would be hard to trace. Also, changing the properties does not trigger an update to the component, resulting in the component and the properties could be out of sync.

Instead, properties should be set as part of the component instantiation process, as shown below.

**JavaScript:**
    
    
    var someComponent = React.createElement(SomeComponent, {
      prop1: "some value",
      prop2: "some value"
    });
    

**JSX:**
    
    
    var someComponent = <somecomponent prop1="some value" prop2="some value">
    

The component can then be re-rendered at which point React will perform its Reconciliation process to determine how the new property values affect the DOM. Then, the DOM is updated with the changes.

See the first CodePen demonstration at the top of this article for a demonstration of the DOM updates.

## State

State represents data that is changed by a component, usually through interaction with the user. To facilitate this change, event handlers are registered for the appropriate DOM elements. When the events occur, the updated values are retrieved from the DOM, and notify the component of the new state. Before the component can utilize state, the state must be initialized via the `getInitialState` function. Typically, the `getInitialState` function initializes the state using static values, passed in properties, or another data store.
    
    
    var Message = React.createClass({
    
      getInitialState: function() {
        return { message: this.props.message };
      },
    

Once the state is initialized, the state values can be used like property values when rendering the component. To capture the user interactions which update the state, event handlers are registered. To keep the React components self-contained, event handler function objects can be passed in as properties or defined directly on the component object definition itself.

See the Pen [React.js State Update Demo][11] by SitePoint ([@SitePoint][3]) on [CodePen][4].

One of the benefits of React is that standard HTML events are used. Included with standard HTML events is the standard HTML Event object. Learning special event libraries, event handlers, or custom event objects is not needed. Because modern browsers are largely compatible, intermediary cross-browser libraries such as jQuery are not needed.

To handle the state changes, the `setState` function is used to set the new value on the appropriate state properties. Calling this function causes the component to re-render itself.

As shown below in the Visual Studio Code editor, the `setState` function is called from the `_messageChange` event handler.

![Visual Studio code editor][12]

### Recommended Courses

![][13]

Wes Bos

A step-by-step training course to get you building real world React.js + Firebase apps and website components in a couple of afternoons. Use coupon code **'SITEPOINT'** at checkout to get **25% off**.

## Conclusion

React components provide two mechanisms for working with data: properties and state. Dividing data between immutable properties and mutable state more clearly identifies the role of each kind of data, and the component's relationship to it. In general, properties are preferred because they simplify the flow of data. State is useful for capturing data updates resulting from user interactions and other UI events.

The relationship between properties and state facilitate the flow of data through a component. Properties can be used to initialize state, and state values can be used to set properties when instantiating and rendering a component. New values from user interaction are captured via state, and then used to update the properties.

The larger flow of data within an application is accomplished via a pattern named [Flux][14].

This article is part of the web development series from Microsoft tech evangelists and [DevelopIntelligence][15] on practical JavaScript learning, open source projects, and interoperability best practices including [Microsoft Edge][16] browser and the new [EdgeHTML rendering engine][17]. DevelopIntelligence offers JavaScript Training and React Training Courses through [appendTo][18], their front-end focused blog and [course site][19].

We encourage you to test across browsers and devices including Microsoft Edge — the default browser for Windows 10 — with free tools on [dev.microsoftedge.com][20], including [status.microsoftedge.com][21], a portal for the latest implementation status and future roadmap for interoperable web platform features in Microsoft Edge and other browsers, including Internet Explorer. Also, [visit the Edge blog][22] to stay updated and informed from Microsoft developers and experts.

[ ![Eric Greene][23]][24]

Eric Greene is a professional software developer specializing in HTML, CSS, and JavaScript technologies. Right now he is focused on Node.js, React, GraphQL, Relay, Angular (1 and 2), Backbone, and jQuery. He has been developing software and delivering training classes for nearly 19 years. He holds the MCSD Certification for ASP.Net Web Applications, and is a Microsoft Certified Trainer. Eric has worked with companies of all sizes in the insurance, nuclear engineering, publishing and communications industries. Among his many professional endeavors, Eric is a Technical Instructor at [DevelopIntelligence][25].

[1]: https://www.sitepoint.com/building-a-react-universal-blog-app-implementing-flux/
[2]: http://codepen.io/SitePoint/pen/XKqQqN/
[3]: http://codepen.io/SitePoint
[4]: http://codepen.io
[5]: https://www.sitepoint.com/getting-started-react/
[6]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2016/07/1469284535working-with-data-in-react-properties-state01.png
[7]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2016/07/1469284540working-with-data-in-react-properties-state02.png
[8]: https://codepen.io/SitePoint/pen/akGxGW/
[9]: https://codepen.io/SitePoint
[10]: https://codepen.io
[11]: http://codepen.io/SitePoint/pen/JKvVvy/
[12]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2016/07/1469284546working-with-data-in-react-properties-state03.png
[13]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/07/1501203893wesbos.jpg
[14]: http://facebook.github.io/flux/
[15]: http://www.developintelligence.com/
[16]: http://blogs.windows.com/msedgedev/2015/05/06/a-break-from-the-past-part-2-saying-goodbye-to-activex-vbscript-attachevent/?wt.mc_id=DX_873178
[17]: http://blogs.windows.com/msedgedev/2015/02/26/a-break-from-the-past-the-birth-of-microsofts-new-web-rendering-engine/?wt.mc_id=DX_873178
[18]: http://appendto.com/
[19]: http://appendto.com/courses/
[20]: https://dev.windows.com/en-us/?wt.mc_id=DX_873178
[21]: https://developer.microsoft.com/en-us/microsoft-edge/platform/status/?wt.mc_id=DX_873178
[22]: https://blogs.windows.com/msedgedev/?wt.mc_id=DX_873178
[23]: https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2016/02/1454616608eric_green-96x96.png
[24]: https://www.sitepoint.com/author/ericgreene/
[25]: http://www.developintelligence.com/instructors/eric

  </somecomponent>