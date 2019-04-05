# blogwise-lib

This library contains tools and documentation for the blogwise application. The
blogwise application is composed of the following components...

- [blogwise-landing](https://github.com/nputnam-hu/blogwise-landing)
- [blogwise-dash](https://github.com/nputnam-hu/blogwise-dash)
- [blogwise-api](https://github.com/nputnam-hu/blogwise-api)
- [blogwise-template-1-canonical](https://github.com/nputnam-hu/blogwise-template-1-canonical)

## Contents

- [Development](README.md#development)
  - [Prereqs and Setup](README.md#prereqs-and-setup)
    - [Test Db](README.md#test-db)
  - [Contribution](README.md#contribution)
  - [Styleguides and Linters](README.md#styleguides-and-linters)
  - [Approach to CSS](README.md#approach-to-css)
- [Application Architecture](README.md#application-architecture)

## Development

This section outlines everything about devloping for blogwise.

### Prereqs and Setup

Our tech stack uses the following:

- Node.js. One can find installers and binaries
  [here](https://nodejs.org/en/download/).
- npm. This should be automatically installed with Node.js
- React. Once npm is installed, you can install React with `sudo npm install -g
  create-react-app`.
- PostgreSQL. TODO: Add note on db installation.

While it's not a requirement, most people on our team use
[VSCode](https://code.visualstudio.com/download).

TODO: See if there are any other setup details.

#### Test Db

TODO: Add notes about how to test locally with your db.

### Contribution

We track broader work items in our [Trello
Board](https://trello.com/b/epw6kWdq/blogwise). Specific issues and features
will be tracked in each repo using Git Issues. Discussion related to the issues
will mostly occur in the Slack.

If you'd like to start working on a repo, follow these steps:

- Find a git issue in the project repository and assign yourself to it. If there
  isn't an issue for the problem you want to work on, create one.
- Create a branch for your issue. When thinking about the size and scope of your
  branch, use your best judgement. Err on the side of smaller.
- Make modular, well defined commits. Start your commit messages with present
  tense verbs (e.g. "Adds warning modal on login page.").
- Once you're done with development, submit a Pull Request. The message with the
  PR should mention...
  - Changes you made
  - Reasoning behind any non-trivial design decisions, and
  - Any new bugs or quirks

### Styleguides and Linters

We typically default to the Airbnb style guides for
[JavaScript](https://github.com/airbnb/javascript),
[React](https://github.com/airbnb/javascript/tree/master/react), and
[CSS/Sass](https://github.com/airbnb/css) with minor changes. The
`.eslintrc.json` and `.prettierrc` files associated with our projects are
located in the `linter-configs` folder.

We recommend using VSCode with the `formatOnSave` setting turned on.

### Approach to CSS

Our CSS strategy uses `sass` with CSS Modules. There are [many
benefits](https://raygun.com/blog/10-reasons-css-preprocessor/) to using `sass`.
We use it in combination with [CSS
Modules](https://css-tricks.com/css-modules-part-1-need/) so we never need to
worry about the scope of classes, especially when thinking about our components
in a modular, React mindset. This means that when nesting components, one's CSS
classes will never affect a subcomponent's classes.

Styling for pages should be stored in `/src/styles`. Styles for components
should be stored with the components. Global vars and classes use across
components can be imported from `global.sass`. Note that Gatsby v2 currently
doesn't support nesting pages in folders without the use of a plugin (see [issue #2](https://github.com/nputnam-hu/blogwise-lib/issues/2)).

## Application Architecture

The structure of the blogwise application is broken across 4 different
repositories. 

![architecture-graphic](https://github.com/nputnam-hu/blogwise-lib/blob/master/images/Architecture%20Graphic.jpg)

### blogwise-landing

The first repo that a client ends up interacting with a website that's stored in
the `blogwise-landing` repo. This page shows external facing information so our
clients can learn about our product (e.g. features, pricing, faqs, etc.). On
this landing page, there are also some links to the blogwise Dashboard, where
they can interact with the rest of the application. 

### blogwise-dash 

The blogwise Dashboard is a front-end where users do everything -- editing their
organization's information adding blog posts, managing team members, and
everything else. The dashboard is stored in the `blogwise-dash` repo. Under the
hood, the Dashboard is making calls to the blogwise API, which does the heavy
lifting with regards to managing the database and interfacing with the Netlify
hosting. 

### blogwise-api

The blogwise API ensures two things: 

1. The information in our database is updated with any changes in the Dashboard.
2. The Netlify instances of client websites are up to date with any changes. 

The code for the API is stored in the `blogwise-api` repo. The API runs on a
Heroku instance. 

When a client updates information about their blog, the API triggers a build
hook to the Netlify hosting service, which launchs a new build. The client's
website data (e.g. assets, blog content, etc.) are sent in the body of this
build hook. 

### blogwise-template-#-canonical

Once a Netlify instance's build hook has been triggered, it pulls code from a
repository corresponding with the template the client has selected. Currently,
we only support one template, which is stored in
`blogwise-template-1-canonical`. 

The Netlify instance takes the information from the build hook body, and imports
it into the template repo code right before it builds. This allows the Netlify
instance to create a blog with a given template that is populated with a
client's blog information. The result of the build is a static site that is
hosted at the Netlify instance.  
