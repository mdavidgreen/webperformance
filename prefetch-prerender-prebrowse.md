# Prefetch, Pre-Render, and Pre-browse
#### Make intelligent choices for the visitor based on common pathways

Steve Souders gave [a talk at Velocity NYC 2013](http://www.youtube.com/watch?v=Msqs1jIzgo4) about pre-browsing; adding hints to your pages so that the browser can intelligently start fetching and rendering content a visitor is likely to want next. Of course, as soon as you start making assumptions about what your visitor might want to do next, you run some risk of wasting bakdwith, requests, and CPU cycles, depending on how heavily you bet on that possibility.

Browser developers have been working on ways to optimize page loading behind the scenes for some time. It's a constant balancing act between how the developers behave, and how browser changes get reflected back in changes to developer practices. And when a developer optimizes content for a particular browser's behavior, it is likley those changes will have either a neutral or a negative impact on other browsers.

Currently, Chrome does a number of things to optimize loading, including performing a two-pass parse on each page, to begin downloading files references in src or href attributes. In some cases, this aggressive prefetching can run counter to older advice developers followed to control page performance.

For example, there is a long-standing pattern of encouraging developers to put all scripts at the bottom of the body element to delay their loading and processing until the visual content of the page has been loaded. By prefetching script src attributes, Chrome hoists those scripts up to the same level as any src attributes for images. As a result, some pages that rely on large hero images may appear not to populate the visual page as efficiently as the developer intended.

As in any performance issue, the critical thing is to measure what is actually happening now for your current visitors. Chrome recently amended its first-pass parse behavior to favor the src attributes of the first few img elements on a page. Small changes such as these happen all the time in browser development, and it is the responsibility of the alert developer to stay on top of which ones matter to the them and their visitors.

Among the upcoming enhancements to broser behavior that will allow developers to exercise some more control over loading and rendering behavior are three new meta elements; dns-prefech, prefetch, and prerender.

### DNS Prefetch
At the least invasive level of optimization is the DNS prefetching. Before visiting a page on a different host, the browser needs to do a DNS lookup, and this adds a little latency. Doing these lookups proactively for sites a visitor is likely to visit next can make the experience a little faster. The cost of doing a DNS lookup is relatively low, since browsers can generally do several of these in parallel in the background. 

In the head, you include this element:
```
<link rel="dns-prefetch" href="//example.com">
```

Doing a DNS prefetch may also initiate a TCP connection to that host, which will make the load even faster. A good example of where this may be useful is when a site uses a content distribution network (CDN) to host static content. A DNS prefetch for the various CDN hosts can make content within the page load more efficiently.

At the moment, DNS prefetching is supported on recent versions of Chrome and Firefox, and is anticipated in Internet Explorer 11.

### Prefetch
Slightly more aggressive, the prefetch directive actually recommends to the browser that a specifed file should be downloaded for the user ahead of time, so it will be available. Interestingly, the spec doesn't specify that this file must be downloaded, but only recommends that it should be made available. How a particular broser chooses to implement this is up to the browser developers. It is also not clear about how multiple prefetch items are supposed to be handled, or what priority a prefetch item should receive. Current browsers vary widely in how many prefetch items are loaded in parallel, and most browsers currently cancel prefetch requests when transitionaing from one page to the next. This is something a developer should be aware of and measure for the target platforms visitors rely upon.

In the head, you include this element:
```
<link rel="prefetch" href="//example.com/file.js">
```

Making these resources cacheable is important, so subsequent pages that prefetch the same resources will benefit from the prefetch. The most likely candidates for prefetching within a site will be the static CSS and JS files that are used across multiple pages. 

Keep in mind that you can add an "Accept-Ranges: bytes" header in your response to allow browsers that support this behavior to resume downloading of partially-loaded prefetched files.

### Prerender
The most aggressive of these tools is the prerender, which effectively forces the browser to load and render content as if it were being opened in an invisible tab of the same browser. This can steal not only connections and bandwidth, but also CPU and GPU cycles from the current page, so it should be reserved for resources a developer is quite certain a visitor will need.

One way to limit this technique to content that a visitor is almost certain to need is to inject prerender elements into the head based on mousedown or touchstart events that will lead a visitor to the requested content. This isn't as beneficial, but it is very conservative, and takes advantage of the time between the start and the end of a click or a touch event in cases where there may be some delay.

In the head, you include this element:
```
<link rel="prerender" href="//example.com/">
```

There is a page visibility api available to developers to allow the behavior of a page that is prerendered to wait until the content is actually in the foreground before executing complex visual behaviors. It's not widely supported yet, but using it judiciously will allow developers to identify expensive code, or tracking code that relies on counting active use, that should only be processed if the visitor is actively looking at the content.

Keeping these techniques in mind is useful, especially if your current visitors tend to use the most modern browsers. As more browser developers implement DNS prefetching, prefetching, and prerendering, developers will be able to see and measure the impact of these techniques on their actual visitor experiences.
