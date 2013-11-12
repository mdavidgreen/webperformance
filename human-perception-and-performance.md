Human Perception and Performance

Ilya Grigorik's presentation at Velocity NYC 2013 focused on performance as an issue of human perception.

### 1000 MS
Ultimately, it's not about numbers and statistics as much as it is about how the vistors you really have feel about the application you are providing. That said, there is an underlying imperative to reduce the time it takes before a visitor can start interacting with your application as low as possible. There is a fabled 1000 millisecond goal developers strive to achieve, but it is important to recognize all the ways that it is a challenge to achieve in a mobile enviroment, given all the constraints we face today.

There are three basic areas that need to be considered: input latency, network latency, transport latency, and render latency.

### Input Latency (hardware and software)

Before a developer even starts, the input device needs to be considered. Many mobile manufacturers implement a default 300ms delay after each click. Their intention is to make sure the user is actually just clicking, and not trying to initiate a double click or some zoom behavior. If you develop your application so it doesn't rely on those behaviors, you can put in workarounds that bypass the 300ms delay, improving perceived responsiveness.

Modern mobile browsers may respect a meta element in the head identifying your content as something a user should not be able to scale:
```
<meta name="viewport" content="user-scalable=no">
```
There are also Javascript libraries such as Fastclick.js that will bypass the 300ms delay.

### Network Latency (Radio and Carrier)
As soon as a mobile application starts requesting content, it has to copw with network latency. This is an area that is constantly changing, as carriers update their infrastructure. But there are ranges of performance that a developer can reasonably anticipate. What's important is to measure what your actual vistors are facing, either by logging behavior or integrating a real-user monitoring (RUM) solution.

If a lot of your visitors are coming through on 3G, your application should be flexible enough to accommodate an initial link-up delay of as much as  2,500ms. That will typically be shorter, but can still be 50-500ms for a reauest to be submitted for lookup on 4G.

When trying to predict and optimize your performance, keep in minde that individual files are batched in chunks, each of which needs to be sent and received. For the sake of evaluation and planning, it might be useful to consider your network latency optimization goals in terms of minimizing the number of independent chunks that need to be requested and delivered.

### Routing and Network Transport Latency
We spend 70 percent of the time just waiting on the network, but there are several things we can do by default to improve the way we deliver content over the network:

Use a content distribution network (CDN) to get your static asset deliver as reduntant and as close to your visitors as possible.

Compress and optimize your content, using server and header settings to maximize automated compression, and minifying, compressing, and bundling static assets before you serve them.

Enable caching for all your static assets. This will not only provide immediate delivery for visitors requesting the same content more than once within the expiration date range, it will also reduce load on your servers.

Reuse connections so you don't have to initiate a new connection every time you need data associated with a particular host.

Optimize transport layer security (TLS) for session resumption and record size.

Eliminate atency from redirects and DNS lookups.

The details on all of these concepts can be intricate, but you can read more about them in Ilya's free book, [High Performance Browser Networking](http://bit.ly/browser-networking) from O'Reilly

### Render Latency
The DOM, the HTML, and the CSS above the fold are the critical rendering path to keep in mind, because these are the aspects of your site that a visitor needs to have available in order to interact with your application. This is the area where the developer has the most control, but it also requires the most detailed information about how the application is constructed and experienced.

The key points to keep in mind for the content that appears above the fold for your target visitors are:
- Stream the HTML to the client
- Get the CSS as small and as fast as you can.
- Eliminate blocking Javascript from the head and the top of the documento

Optimizing for mobile adds a few extra considerations to keep in mind, especially for those first critical pieces of the application your visitor will see:
- Avoid plugins that need to be loaded separately
- Eliminate scripts and features that are specific to desktop
- Configure viewport layout to reduce the need to redraw
- Fit contents to viewport width
- Make sure your text is legible
- Make sure tap targets are the right size

How many roundtrips does it take to render the content above the fold on the page? Can you inline what you need above the fold, and defer other things. Try inlining the CSS and Javascripts required for above-the-fold content, and lazy loading the rest:
- Get all the initial reqirements into a single request by making them a single file.
- Defer all beacons, plugins, analytics until after the initial screen is usable
- Inline all the CSS and JS needed above the fold, and defer loading the remainder.

Optimize fetching, DNS resolution, and rendering in modern browsers by including appropriate link elements in the head of the document. These can provide hints and directives to the browser, allowing separate resources that a visitor is likely to need to download in parallel or in the background.

The &lt;link rel="dns-prefetch" href="http://example.com"&gt; head element opens a connection to a host in the background. This is a low-risk, inexpensive thing to add to a page when you know you will be needing content from a particular host, such as a CDN provider, and you want the connection to be ready for you.

The &lt;link rel="subresource" href="http://example.com"&gt; head element instructs the browser to start downloading specific files that this page will need, bypassing the normal prioritization defined by the placement of elements in the HTML.

The &lt;link rel="prefetch" href="http://example.com"&gt; head element is similar to the subresource element, but it is less specific to a particular page, and also more flexible about how the browser should carry out the recommendation. 

The &lt;link rel="prerender" href="http://example.com"&gt; head element aggressively loads and renders content as if it were being displayed in a background tab of the browser, so it will be ready when the visitor switches to it.

### Measure, Measure, Measure
All of these approaches are subject to the needs of your visitors and the requirments of your particular application. It is important to measure actual usage to determine where your bottlenecks are, and whether they are affecting the experiences of users in a relevant way.
