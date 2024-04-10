This is an old post from the ghost site:

About twice a year, I like to put together web development training to try and give folks at the Districts and other labs a taste of web app development.  In the past I've offered an introduction to web development class, focused on the basics of HTML, CSS and JavaScript.  I think you should get to know the web platform without looking through the lens of a framework so that as the cool kids decide that framework X is the new hotness, you can better understand the pros and cons of moving on or sticking with the architecture that you're using.

That said, the advanced course will be framework focused.  You can do a lot with vanilla JS, HTML and CSS, but at a certain point your application will introduce enough complexity that frameworks start to make sense, abstracting you away from some of that complexity, and making your life easier.  

Once you introduce frameworks, you introduce opinions.  There are almost as many JavaScript frameworks as there are developers out there and just as many angry twitter handles that will tell you that your preferred framework is the wrong framework.  That is to say that the architecture that we're going to be exploring here isn't the "right" answer for everyone, it's where I've landed as the approach that makes the most sense to me.  I've built my share of web apps over the years, and I've been spiraling in on this particular combination of frameworks over time.  If you're ready to get into more advanced web apps, I think this approach can work for you.

Course Overview
I'm going to be rolling out posts covering these topics as I finalize the material for the advanced course.  You're welcome to read through these posts, and that'll get you the gist of what we're going to cover in the class, but the class will be focused on hands-on application of the concepts covered here, i.e. you get to build something cool!

Here are some of the high-level concepts that we're going to cover.

Static Client-side Apps vs. Full-stack Apps
Historically client-side web applications have been tightly coupled with the back-end applications, serving new pages on each navigation event through the app and maintaining a lot of state on the server side.  I'm a big proponent of splitting the client from the server, moving toward a more "micro-service-y" (yay, bring on the jargon!) type approach.  

This takes the form of a completely separate client-side application loosely coupled to server-side services using Ajax requests to load data without triggering a full-page reload.  These client-side applications can then be served simply as static HTML, JS and CSS resources, earning their name Static Apps.  I don't want to get into the weeds on Single-Page Apps (SPAs) vs multi-page apps, just know that we can build our app as a pile of text files that can be served by any static file server to the browser.

This approach brings with it a number of pros and cons when compared to the more traditional full-stack approach.

Pros:

Loosely-coupled applications are more resilient to change, if one third party service stops working for you, swap it out for another.
You don't have to know how to build a server-side app, just wire into an existing service and you're good to go.
Serving the app as static files means that you can serve many more clients simultaneously vs maintaining session state for each user on the server.
Cons:

If you are building a server-side component to the application, you end up with multiple code projects to keep track of.
Server-side frameworks like Grails, Rails, Apex and others let you publish an app without having to worry too much about the client-side logic, you can write your app in the native server-side language of your choice without having to worry about JavaScript, kind of...
Frameworks
Since we're exploring Advanced Web Development, it's time to talk frameworks. You can find frameworks that purport to meet all your needs, but I'm allergic to lock-in so I tend to be a bigger fan of a more modular approach.  In order to figure out what frameworks we'll need, we have to get a handle on the problems that we face and need a framework to help us deal with.

Presentation
Presenting data and content in a user-friendly way while keeping the app developer-friendly and easy to understand and modify is arguably the first and most important challenge of web development.  As the size and complexity of an application grows this challenge becomes more and more apparent.  We've all seen applications that start out making certain assumptions about the functionality that the users are looking for, and after months of decisions by committee morph into fragile tangled piles of code that no-one wants to touch for fear of the cascading effects one change might make.

React.js  helps us deal with this complexity by breaking our application into modular components that can be composed together to form an application.

Create-react-app (CRA)is a project that helps us scaffold out new React.js app without having to deal with the configuration headache that can come along with transpiled client-side apps.  CRA makes a lot of assumptions and can sometimes feel like it's getting in the way, but provides a good 80% solution for getting an app started.

State Management
With the more complex interface comes more complex state, and keeping track of that state can sometimes become a problem.  In this context state can include data about the layout of the interface, data about the current user, and data obtained through Ajax requests to our or third-party services.

Redux is a library that provides a centralized data store that all parts of your application can use to store and retrieve state.  Out of the box Redux is a pretty good solution for state management, but requires a lot of boilerplate code, too much for my taste.

I share this opinion with the creator of ReduxBundler, a thin abstraction on top of Redux that makes life a lot easier, especially when paired with React.js.  We'll dive into the details in a subsequent post, but the beauty of using Bundler with React is that we can build our application logic in modular bundles, then wire up UI components that are always up-to-date with the current state of our application.

Connectivity
A stand-alone client-side application is fun, but it's even more fun if we can wire it up to communicate with one or more servers so that we can interact with data or other users.  

XHR is a library that sits on top of the XMLHttpRequest object available in the browser for making Ajax requests.  XHR isn't the only option out there, you arguably don't even need a separate library for this, but it's the one that I like, so that's the one we're going to use.

Most applications that we build will lean solely on Ajax requests for moving data in and out of the browser, but if we want truly real-time functionality we want to use a technology called web sockets.  Socket.io is the most commonly used JavaScript web socket library providing both client and server-side libs that are easy to integrate with the rest of our stack.

Come to the class
If you made it this far through the post, I'm thinking you just might be the type of person that would find it interesting to actually build something using the frameworks I mentioned above.

We'll be using all of these libraries and frameworks to build a collaborative mapping application, think Google Docs, but on a map.

I'll be posting portions of the app here, highlighting some of the aspects that I find most interesting, but for the full experience, you should think about coming to the course in person.

June 3-6 (Monday - Thursday) in Denver, CO at the Risk Management Center (RMC) offices.
If you are interested in attending in person, let me know and I'll send you the full details.  Hope I see you there!
