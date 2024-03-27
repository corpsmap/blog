## Front-end Quickstart

This guide will lead you through the process of starting up a new front-end project using the [Groundwork](https://usace.github.io/groundwork/) component library and a few of our other recommended libraries.  If you are not familiar, Groundwork is designed to reflect the USACE web standards as a React library, allowing developers to build sites that match the enterprise.

### Vite

[Vite](https://vitejs.dev) is the build system we use for our front-end development.  It transpiles your code in Typescript or JavaScript into bundles of JavaScript that the browser can use efficiently.  It includes a development server that allows you to preview your site as you build it with included hot-reload.  It does lots more, but I'll let [the vite docs](https://vitejs.dev/guide/why.html) do the heavy lifting if you want more details.

Using your terminal, `cd` to the folder where you want your front-end project to live, in my case I often put projects in `~/code` for example.

Run `npm create vite@latest`. This will walk you through a few options:

1. Name your project, this will be the enclosing folder name that will be created in your current directory.
2. Pick a framework, we're using React
3. Pick a variant, I typically use JavaScript, TypeScript is a popular option, and the reasons I don't use it deserve their own post, so just pick JavaScript for now.

Vite will create your project with some basic scaffolding, which we can use to make sure the setup went right, but we'll clean out most of it as we start to build our app.

### Test the Setup

Follow the prompts from the Vite tool:

```
cd <project_name>
npm install
npm run dev
```

The install step can take a minute depending on network access. If everything went ok, you should see the following message in your console after running `npm run dev`:

```
VITE v5.2.6  ready in 248 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

Open your browser to the URL `http://localhost:5173/` (you can ctrl+click the URL in most consoles).

If you see a site with the Vite and React logos above a button that operates a simple counter then you're good to go!

Hit `ctrl+c` to shut down your development server, we're going to make some changes and then start it back up in a little bit.

### Install Dependencies

While we always want to limit our dependencies list, there are a few that we typically use in our projects.  The first being the Groundwork component library.  This library will give you a set of pre-built components you can use to lay out your site.  Check out the [Groundwork documentation](https://usace.github.io/groundwork/) for more information.

Run `npm install @usace/groundwork`.

#### Optional Dependencies

State management in the React ecosystem can be a complex subject, there are lots of approaches for managing application state.  We could spend countless hours debating the merits of the various libraries out there not to mention the built-in state management features provided by React. So in the interest of time and sanity, we're going to cover the solution we've landed on and have yet to set aside in favor of another.

Run `npm install redux-bundler redux-bundler-react redux-bundler-hook` to install the base set of [Redux Bundler](https://reduxbundler.com) libraries.  This provides a solution built on top of [Redux](https://redux.js.org/), a popular state management library with an easier developer experience than raw Redux.  It also includes utilities for page routing (displaying the right page based on the URL) and data caching.  

Run `npm install money-clip` for a simple cache that can be wired up to bundler.

Run `npm install internal-nav-helper` for a utility that will allow us to use simple `a` tags with `href` attributes rather than require that we use some abstract `<Link/>` component from a client-side-routing library.

Each of these will need to be added to the code for the app in order for us to start taking advantage of them.

#### Tailwind

While I have mixed feelings about [Tailwind](https://tailwindcss.com/) it provides enough utility that I have to admit I like using it.  There are numerous other options for styling/css/css-in-js but Tailwind is becoming very widely used and gives you some options.  Groundwork is built using Tailwind.  

More on installing Tailwind soon.

### Start using Groundwork

Open the project folder in the IDE of your choice, we typically use VSCode.

Open `src/index.css`. Delete everything here, we don't want these styles colliding with what we're doing.

Open `src/App.jsx`. We're going to replace the contents with the code block below:

```
import { SiteWrapper, Container, UsaceBox } from "@usace/groundwork";
import "@usace/groundwork/dist/style.css";

function App() {
  return (
    <SiteWrapper>
      <Container>
        <UsaceBox className="mt-8">
          <div title="My New Site">Hello World</div>
        </UsaceBox>
      </Container>
    </SiteWrapper>
  );
}

export default App;
```

There are lots of options for customizing the site wrapper, but this should get you a basic header, footer and styled content on the page.