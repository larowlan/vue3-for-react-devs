
### Improving CWVs with Drupal 🪄

---

### ℹ️ Overview

<ul>
<li class="fragment fade-in-then-out">❓️What <span class="underline">CWVs</span> are</li>
<li class="fragment fade-in-then-out">💧 How to improve them with <span class="underline">Drupal</span></li>
</ul> ️

---

### ⚠️ TLAs ahead

<div class="fragment fade-in"><p>Play along with <span class="underline">TLA bingo</span></p>
<p>https://bit.ly/cwv-bingo</p>
<p>Win great* prizes</p>
</div>
<div class="footnote fragment fade-in small">*Prizes may not be great</div>

Note:

- Lots of TLAs ahead (three letter acronyms) ahead
- Knowing what they are and what they mean are table stakes for those in the web industry such as yourself

---

### 🤔 WTF are CWVs

<div class="fragment fade-in">(and why should I care 🤷)</div>

<div class="fragment fade-in"><span class="underline">C</span>ore <span class="underline">W</span>eb <span class="underline">V</span>itals</div>
<div class="fragment fade-in">https://web.dev/vitals-business-impact/</div>

Note:

- Why should I care
- Core Web Vitals
- Numerous studies show that improving them leads to better business outcomes
- Series of measures that quantify a good experience for users of your site
- You've all visited that site that takes forever, or feels to take forever, so you abandon it
- Vodafone 31% improvement lead to 8% increase in sales
- Increased page rank (esp on mobile)
- This link has more
- Made up of three key measures - so let's meet them

---

### 1️⃣ LCP 🖼️

<div class="fragment fade-in"><span class="underline">L</span>argest <span class="underline">C</span>ontentful <span class="underline">P</span>aint</div>

Note:

- Largest contentful paint
- Measure of when the user sees the browser render the largest piece of content
- Page feels 'loaded'
- Measured in seconds
- Under 2.5 sec is good
- Over 4 is poor
- This is where we'll spend most of our time when we get to talk about Drupal

---

### 2️⃣ FID 🖱️

<div class="fragment fade-in"><span class="underline">F</span>irst <span class="underline">I</span>nput <span class="underline">D</span>delay</div>

Note:

- First input delay
- How long does it take from when the page is loaded until you can click on stuff, interact with forms etc
- Not such an issue with Drupal as we're doing SSR (server side rendering)
- But for a SPA (single page app) with a high amount of CSR (client side rendering) it might take a while for JS to parse, app to rehydrate and in that time clicking doesn't do anything
- Site feels laggy after it loads
- A bit like Microsoft Windows on startup, good at looking ready, but you can't actually do anything for a while
- Measured in milliseconds
- Under 100ms is good
- Over 300ms is poor
- Note all tools can measure this and sometimes substitute TBT (total blocking time) as a proxy

---

### 3️⃣ CLS 👇️

<div class="fragment fade-in"><span class="underline">C</span>umulative <span class="underline">L</span>ayout <span class="underline">S</span>hift</div>

Note:

- Cumulative Layout shift
- You're about to click on something, and then something loads and pushes the content down, the link jumps and you click on the wrong thing
- Very annoying
- Measured by two fraction, impact and distance which get multiplied, so a bigger item moving further is worse, a small item moving a little bit, not so bad
- As a percentage of the viewport area
- Under 0.1 is good
- Over 0.25 is poor
- Think to lookout for here are banners/alerts that push the body down after the page has loaded and images/media that don't have a fixed size and push content after they load

---

### Measuring your baseline 📏

web.dev/vitals-measurement-getting-started/

Note:
- Lots of great tools here for ongoing measurement
- Chrome anonymously collects data for PSI (Page Speed Insights) and the GSC (Google Search console)
- Web vitals chrome extension
- There's a Javascript API for it, and an npm package `web-vitals`
- Chrome devtools and Lighthouse (these use TBT - total blocking time) instead of FID
- Various RUM tools (real user monitoring) like new-relic

---

### Understanding lighthouse 🔦

Note:
- I'm going to focus on Lighthouse here because its the most accessible for developers and covers other things like SEO and accessibility
- Another option is the Web Vitals chrome extension
- Those are the two that work best in your local environment when you're making changes
- The others are intended for real world monitoring of actual user experience

---

### Automation 🤖

web.dev/lighthouse-ci

Note:
- You want to make catching regressions in your CWV built into your CI/CD chain
- For this there's lighthouse-ci npm package
- Set your targets and your build will fail if there's a regression
- Setting this up could be a talk on its own so we won't cover it here, but I'm hoping to get a blog post out about it in the coming months
- We've got recipes at PNX for running this on our CI platform of choice CircleCI but we've also run it on gitlab and with govcms PAAS

---

### Low hanging fruit 🍉

web.dev/fast/

Note:
- So when you first run the report if you're starting from zero, there's going to be some low hanging fruit
- The lighthouse report can detect you're running Drupal and will give you Drupal specific recommendations
- Let's run through a few of those

---

### Minimzing the size of the dom 🧩

Theming with Bundle Classes, Layout builder and Twig

Note:
- My colleague Daniel presented yesterday on new approaches to theming w/ Drupal
- Drupal is notorious for outputting div soup
- If you have a design system and wire up Drupal to provide data and not markup, you can reduce the number of elements which improves parsing speed

---

### Aggregation 🗜️

drupal.org/project/advagg

Note:
- There's a lot of options here, so be sure to measure before and after and find what works for your site

---

### Image sizes 📐

* 💧 issues/3192234
* 💧 issues/3247795

Note:
- This one might be a no-brainer but sometimes things start out ideal and change over time
- Make sure you're using the best sizes for your viewport
- Configure responsive images styles appropriately
- Beware of the fallback option. You definitely don't want 'original image'. I've seen a site that was serving 14Mb of images to iphone users on a single page because of this
- There's no easy way to do this, I like to get all the breakpoints in the theme, and measure each component's images at the top and bottom of each breakpoint
- I recommend writing some cli tools to let you auto-build image styles so they're repeatable and you're not getting RSI from clicking
- There's some CLS considerations here too - be sure to set width and height, you need a core patch for this on the picture element
- Since 9.4 most image formatters in core let you nominate if an image should be lazy loaded, which is a nice easy win, work is afoot to support this with WYSIWYG media embeds too

---

### Image formats 📸

* webp
* avif

Note:
- JPG and PNG are probably not the image formats to use for the web
- The picture element supports multiple types allowing for fallbacks  
- AVIF not yet supported in Safari and Edge (71% of browsers)
- webp should be safe now that IE11 is dead (97% of browsers)
- there are drupal modules for both, but they collide a bit
- the avif module struggles if you have a jpg and png image with the same name
- for avif your server may not support converting to that format, some custom code may be required to wire it up - e.g. a plugin
- its worth the effort - the size reductions are noticeable
- php 8.2 will support avif with GD and make things simpler

---

### Code splitting ✂️

* https://youtu.be/LSSzMwOeuCw

Note:
- my colleague Rikki gave a whole session on this at DrupalSouth 2021 which you can watch online
- it boils down to using a bundler like webpack or rollup to split your javascript up, and embracing JS modules now IE11 is dead
- don't send the user JS they don't need - if there's no carousel on the page, don't send the JS

---

### Deferring scripts 🧑‍💻

Note:
- A key tenet of improving LCP is eliminating resource load delay, this should be <10% of your LCP
- A bonus of code-splitting and using JS modules is that modules are automatically are defer and async
- Check your html for script tags that have no async or defer attributes, these are render-blocking
- If you have JS that needs to run as early as possible, inline it, but only for small scripts

---

### Lazy loading 💤

* Optimise Your Page Loads with Lazy Loading JS
* d.o/project/lazy

Note:

- Hopefully you saw Rikki's excellent session yesterday on this topic, but if not, be sure to watch the recording
- If you've got a Vue or React app that's powering your mega-menu or an interactive component, don't load React dom on page load
- Only load it when the user scrolls to the app
- But as I mentioned before this also applies to images and you can also put the lazy attribute on iframes
- Video embed (e.g YouTube) is a killer, 1MB of JS - using the Lazy contrib module can put this on a diet and only load it as required

---

### Optimizing fonts 🔠

Note:
- font-display: swap to show text immediately
- preload fonts (link rel=preload)
- optimize characterset - don't serve every character in the UTF range if you don't need it
- serve locally (because each new domain requires a DNS lookup and impacts your resource cascade and doesn't make use of persistent connections)

---

### Going deeper 🔬

Note:
- OK, so what if you've got this far and you're still not happy with your CWV scores
- Well things are about to get a technical here folks

---

### Third-party scripts 🐘

😭
<div class="fragment fade-in">partytown.builder.io 🎉</div>

Note:
- First let's talk about the elephant in the room, your site is lean, you've optimised your images, and then the marketing department comes along and adds HotJar, Facebook Pixel and copious amounts of tracking JS via 
 Google Tag Manager and your site is now parsing more fetching and parsing 5MB of Javascript before it can even render the page  
- In this scenario you need to go out and around the default GTM module and go bare metal
- A promising solution in this space is Partytown 🎉 which offloads third party scripts to web-workers but you're going to need to get your hands dirty here, there's no drop in module

---

### Critical CSS 🎨

* npmjs.com/package/critical

Note: 

- We spoke before about render blocking JS but every CSS file is render blocking
- So large stylesheets will kill your LCP
- If you're using sass to bundle one mega style.css you're in trouble
- It is recommended that you extract critical CSS (Above the fold) and inline it and then the rest is deferred
- But there's no way to do this with Drupal out of the box
- So you need to reach for some nodejs here
- Do this with the `critical` npm package, it uses puppeteer to load pages from your site, and extract the CSS needed for a given viewport, and nothing else
- At PNX we're generating one critical CSS file per template as part of our front-end build and then conditionally loading them - which brings me to the next slid

---

### Asset rendering 🖌️

<div class="fragment fade-in"><pre><code>asset.css.collection_renderer</code></pre></div>

Note:

- So before we spoke about pre-loading fonts
- And deferring stylesheets
- And inlining critical.css
- But how do you do this in Drupal, you can swap out the asset.css.collection_renderer service

---

### Fine grained control ⚙️

<pre>
<code class="highlightjs php">
public function render(array $css_assets): array {
  // In here you have full control
}
</code>
</pre>

Note:
- Things you can do here
- preconnect hints for external DNS lookup
- preload for font files
- inline critical CSS based on the current route
- use the swap method for other CSS

---

### CSS swap 🔄

<pre>
<code class="highlightjs html">
<link rel="preload" href="{{ href }}" as="style" data-stylesheet-swap>
<noscript>
 <link rel="stylesheet" href="{{ href }}">
</noscript>
</code>
</pre>

Note:
- Instead of attaching your CSS as link rel=stylesheet, attach it as preload
- when the page is ready, swap it to rel=stylesheet
- critical.css has been inlined so the page looks styled and loads fast
- this is a variation of the recommended approach (the recommended approach isn't compaitble with CSP or content security policy)

---

### Resources 📚️

web.dev/learn-core-web-vitals

---

### Questions ❓️

🗨️ larowlan #australia-nz / drupal.slack.org

