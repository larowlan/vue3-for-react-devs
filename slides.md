<!-- .slide: data-background="./images/water.jpg" -->
### Keeping Drupal relevant 
#### in a world of Jamstack and CMS-as-a-service

---

### About me

@larowlan

Note:

- Web development for over 20 years
- Core framework manager, security team
- I came to Drupal from ancient JavaScript frameworks like YUI

---

### ️Overview

1) Background
2) Headless CMS landscape
3) Where are we now
4) Where do we want to go

Note:

- Brief jamstack overview
- Examine Headless CMS offerings, including SAAS
- Explore what this means for Drupal

---

<!-- .slide: data-background="./images/where.jpg" -->

### Chapter 1
#### Background

---

### First some truths

Note:

- Let's start with some concepts we hold true

---

### Javascript is eating the web

Note:

- JS is the lingua franca of the web
- But PHP still pays well

---

### Developers don't want to work in Twig

Note:

- FE developers want to use their framework of choice
- Can you blame them

---

### Content editors have requirements

Note:

- We can't throw the baby out with the bathwater
- Frameworks like Drupal have put a lot of power in the hands of content-editors
- We can't just take that away
- I want to pause on this because we will come back to this

---

<!-- .slide: class="jamstack" -->
### Jamstack

https://jamstack.org

Note:

- This is a decoupled conf, I'm talking to the converted
- Javascript APIs and Markup
- Pages are rendered at build time
- Deployed to a CDN
- Dynamic functionality added via JS and APIs

---

### Hard problems

Note:

- it's not all rainbows
- build times
- cache invalidation
- lack of context
- big sites are challenging (github issues my build takes 8hrs)

---

<!-- .slide: data-background="./images/landscape.jpg" -->

### Chapter 2
#### Headless CMS landscape

https://jamstack.org/headless-cms/

Note:

- The landscape is crowded, 71 API based (a further 16 use git for storage)
- 32 open source, the rest are SAAS
- Let's look at a few

---

### Strapi

* open source w/ SAAS version
* no features above what Drupal can do
* very nice content editor/site builder experience

Note:

- open source w/ enterprise edition
- very similar (smaller) feature set to Drupal with a slick UI
- plugins installed with yarn, which triggers a full rebuild
- graphql, defaults to v3 style, but you can customise it

---

![](./images/strapi/adding-a-field.png)

---

![](./images/strapi/field-types.png)

---

![](./images/strapi/relationships.png)

---

![](./images/strapi/edit-form.png)

---

![](./images/strapi/media.png)

---

![](./images/strapi/markdown-vs-wysiwyg.png)

---

![](./images/strapi/hooks.png)

---

### Contentful

* SAAS
* Commenting threads for editors
* Nice content-editor experience

Note: 
- Nice editing UI
- Relationship support
- Starter project had 4000+ security issues

---

![](./images/contentful/adding-a-field.png)

---

![](./images/contentful/content-types.png)

---

![](./images/contentful/content-type.png)

---

![](./images/contentful/editing.png)

---

![](./images/contentful/comment-thread.png)

---

![](./images/contentful/off-site-preview.png)

---

![](./images/contentful/webhooks.png)

---

![](./images/contentful/widgets.png)

---

![](./images/contentful/media-library.png)

---

### Sanity.io

* SAAS, with local studio
* Concurrent editing
* JSON to edit content model

Note:
- Concurrent editing is a killer feature
- JSON for the content model is powerful, but a step back for site-builders
- Very nice offering

---

![](./images/sanity/editing-model.png)

---

![](./images/sanity/item-list.png)

---

![](./images/sanity/editing.png)

---

![](./images/sanity/query-sandbox.png)

---

![](./images/sanity/media-library.png)

---

### Prismic.io

* SAAS
* Releases feature above Drupal
* Relationships
* Slices

Note:
- Releases is like workspaces
- Slices is like paragraph/block-types

---

![](./images/prismic/adding-fields.png)

---

![](./images/prismic/adding-fields2.png)

---

![](./images/prismic/editing.png)

---

![](./images/prismic/media-library.png)

---

![](./images/prismic/off-site-preview.png)

---

![](./images/prismic/slices.png)

---

![](./images/prismic/releases.png)

---

<!-- .slide: data-background="./images/now.jpg" -->

### Chapter 3
#### Where are we now

Note:
- What's the take home message? 
- These offerings are coming up fast behind us

---

### Where does this leave Drupal?

Note:

- Is it worth the hassle of maintaining a Drupal site
- And a codebase

---

### I don't have the answers

---

### But I have some thought starters

---

### Market gaps

Note: 
- I pointed out before about content editors having requirements
- This is important because monolith CMS features are lost
- There are hard-problems in Decoupled land need to be solved

---

### Webform

---

### Layout builder

---

### Contextual preview

---

### Contextual editing

---

### Staged deployments

---

### Workflows & Revisions

---

### i18n

---

### Access controls

---

### Customisation

---

### Our strengths

Note:

- 20 years
- The community
- Large organisations/investment
- Composition of modules

---

### Community

Note:

- When we work together everyone wins
- Orgs working in isolation is not the answer
- Rising tide lifts every boat

---

<!-- .slide: data-background="./images/go.jpg" -->

### Chapter 4
#### Where do we want to go

---

### Decoupled menus initiative

Note:

- The menu integration is not the key deliverable
- The key deliverable is working out how we as a community can ship npm packages for Decoupled drupal solutions

---

### We've done this before

Note:

- If this sounds familiar?
- Modules are the building blocks to greatness

---

### We just need to do it differently

Note:

- npm packages not modules
- npm install your features in

---

<!-- .slide: class="twocol" -->

### Examples

- Data binding
- Live editing
- Toggleable previews
- Moderation
- Form building
- Layout building

Note:

- Decoupled menus is the tip of the iceberg

---

### Solve the repetitive bits

Note:

- As a community we can solve the boring bits
- Focus on the parts that are unique to your site

---

### We have the tooling now

Note:

- Drupal.org now supports general projects
- We're shipping packages to npm from d.o

---

### So do we do that in React? Vue? 

---

### Avoid the framework wars

Note:

- Split out generic bits
- Build for every framework
- These SAAS offerings do

---

### This is our opportunity

Note:

- I don't have the answers
- But this is where I think we should focus our effort
- Collaborate on these features

---

### Summary

Note:

- So yes JS is eating the web
- But we have 20 years of hard-learned lessons that are valuable
- And a community of experts
- And we're getting our ducks in a row
- So is this how adapt Drupal for the next 20 years?
