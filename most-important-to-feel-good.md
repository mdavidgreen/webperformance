# It's Most Important to Feel Good
#### When measurement and metrics can get in the way of good visitor experience

At the October Velocity Conference in New York, [three speakers debunked](http://www.youtube.com/watch?v=mx0G8NPqNBc)  one of the most popular measures of page performance; time to load.

## Time to Load vs. Actual Availability
In order to evaluate the delivery of content, many developers have turned to measuring the time it takes for a page to load as a reasonable metric. The concept is that you set a marker when the page is requests, and another when all the content of the page has been downloaded and rendered. Optimizing for that number helps you improve your performance.

However, as Patrick Meenan of Google pointed out, optimizing for that raw value can sometimes be misleading. For a user, the important parts of the page may load long before the entire page is loaded and rendered. In fact, ideally the aspects of a page that are of the most interest to users should load and be available for interaction as quickly as possible, while the rest of the content can reasonably be loaded while the user is enjoying the intended content.

Meenan points out that optimizing for the entire page load may sometimes mean compromising the optimal visitor experience. After all, if you re-engineer your page to support the maximum speed for elements such as social interaction buttons and tertiary navigation, and that comes at the expense of loading the primary content a user has come to your page to see, you are providing a less than ideal experience.

The goal in most cases should be to identify what elements on the page are the most critical to the visitor. Tune the page to deliver these elements as quickly as possible, and then lazy-load the rest of the content afterward.

## Phase Testing
Chao (Ray) Fong took this concept a little further, defining the page load process as a series of identifiable phases. Most automated tools are unable to determine what is or is not critical to the visitor, although they can tell you when the content that is initially visible on the page has fully rendered. Chao has developed a tool that does image comparisons during page load to define when the page changes substantially. Each substantial change represents a phase of loading that can be targeted for optimization.

For example, the first phase may simply be the plain white background. The second phase may be a change in the background color and the presence of rudimentary navigation. The third phase may include some page-specific content elements, and the fourth phase may supplement that with related content links and social feedback buttons. A developer would want to optimize these phases to minimize the time until the third phase is complete.

## Promises for Phases
Keeping this concept in mind, Mike Petrovich demonstrated how a dating site he works on used this concept productively. The main page of the site features a central interactive element with functional buttons, and a variety of additional content a user may also be interested in. Petrovich developed a promise-based Javascript evaluation technique to determine when the important parts of his page had fully loaded, and to distinguish that from when the rest of the content on his page had loaded.

By optimizing for the way your visitors want to interact with your page, you can improve how speedy your site feels, even if the total time to load is bogged down by third-party scripts and remotely-hosted elements. Just define for each page type what a visitor is most interested in, and make sure those elements are served and available as quickly as possible, allowing the rest of the page to load afterward.