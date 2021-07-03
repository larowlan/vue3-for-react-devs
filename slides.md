<!-- .slide: data-background="./images/water.jpg" -->
### Keeping Drupal relevant 
#### in a world of Jamstack and CMS-as-a-service

---

### About me

@larowlan

Note:

- Web development for over 20 years
- Core framework member, security team
- I came to Drupal from ancient JavaScript frameworks like YUI

---

### ️Overview

Note:

- Brief jamstack overview
- Examine Headless CMS offerings, including SAAS
- What role can Drupal play

---

### First some truths

Note:

- Let's start with some concepts we hold true

---

### Javascript is eating the web

Note:

- But JS is the lingua franca of the web
- PHP still pays well

---

### Developers don't want to work in Twig

Note:

- FE developers want to use their framework of choice

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

### Headless CMS landscape

https://jamstack.org/headless-cms/

Note:

- The landscape is crowded, 71 API based 
- 32 open source, the rest are SAAS

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

### Where does this leave Drupal?

Note:

---

### Market gaps

- Contextual editing
- Contextual preview
- Moderation
- Form building

Note: 
- I pointed out before about content editors having requirements
- This is important because monolith CMS features are lost
- There are hard-problems in Decoupled land need to be solved

---

### Our strengths

Note:

- 20 years
- The community
- Large organisations/investment
- Composition of modules

---

### We've done this before

Note:

- Modules are the building blocks to greatness
- Rising tide lifts every boat

---

### We just need to do it differently

Note:

- Smart components

---

<!-- .slide: class="twocol" -->

### Examples

- Data binding
- Live editing
- Preview functionality
- Moderation
- Form building
- Layout building

---

### Solve the repetitive bits

Note:

- Focus on the parts that are unique to your site

---

### We have the tooling now

Note:

- Drupal.org now supports general projects
- We're shipping packages to npm from d.o

---

### Avoid the framework wars

Note:

- Split out generic bits
- Build for every framework
- These SAAS offerings do

---

### This is our opportunity

Note:

- Collaborate on these features
- Stop creating siloed

---

### Summary

Note:
