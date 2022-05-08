
## Avoiding CSRF with Remix

---

### About me

@larowlan

Note:

- Senior Developer at PreviousNext
- PHP Development for 22 years
- Major contributor to Drupal, part of the Drupal security team
- Lots of Javascript last 4-5 years

---

### WTFBBQ is CSRF

Note:

- Like most security exploits, it has a cool acronym
- Let's start by talking about what it's not

---

### <del>Crack Squad Rambo Ferrets</del>

---

### <del>Co-ordinated Sentient Roomba Fleet</del>

---

### <del>Collection of Striped Rooster Feathers</del>

---

### Cross Site Request Forgery

Note:

- And I know you're saying, yes those are English words, but what do they mean

---

### Cross Site Request Forgery

> Cross-Site Request Forgery (CSRF) is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated

https://owasp.org/www-community/attacks/csrf

---

### Simplest (worst) example

<pre class="fragment fade-in-then-out">
    <code class="language-javascript hljs">
        import express from "express"
        import { checkLogin } from "@app/user/auth"
        import { favouritesRepository } from "@app/model/favourites"
        const app = express()
    
        app.get('/favourites/delete/:favouriteId', (req, res) => {
            if (!checkLogin()) {
                return res.sendStatus(403);
            }
            favouritesRepository.deleteById(req.params.favouriteId)
            res.send('OK!')
        })
    </code>
</pre>

Note:
- And before you say no-one would right that code, I've seen this in apps I've audited
- Obviously the first thing wrong here it is using GET!
- But there's no check that the user intended to perform this action if we switch it to POST

---

### Exploiting the simple example

<pre class="fragment fade-in-then-out">
<code class="language-html hljs">
<img src="https://yoursite.com/favourites/delete/3" />
</code>
</pre>

Note:
- So this is fairly low impact, its deleting a favourite, but what if it was a more important action?

---

### What if we used POST?

<pre class="fragment fade-in-then-out">
<code class="language-html hljs">
<form id="form" action="https://yoursite.com/favourites/delete/3">
    <input name="favouriteId" value="3" />
    <input type="submit" />
</form>
<script type="text/javscript">
    window.onload = function() {
      document.getElementById('#form').submit();
    };
</script>
</code>
</pre>

Note:
- Sure this ends up on your site so the user has some idea

---

### Obfuscating further

<pre class="fragment fade-in-then-out">
<code class="language-html hljs">
<!-- Visited site -->
<iframe height="0" width="0" src="https://evil.com/trigger.html" />

<!-- trigger.html -->
<form id="form" action="https://yoursite.com/favourites/delete/3">
    <input name="favouriteId" value="3" />
    <input type="submit" />
</form>
<script type="text/javscript">
    window.onload = function() {
      document.getElementById('#form').submit();
    };
</script>
</code>
</pre>

---

### How does this work?

<ul>
<li class="fragment fade-in-then-out">🤔 If you think about it, that's how the web works</li>
<li class="fragment fade-in-then-out">🍪 Cookies are baked into its DNA</li>
</ul>

Note:
- Its just links or imgs in the case of GET - so that's why we don't use GET for mutations
- Its just forms in the case of POST - so that's why you need CSRF protection

---

### What about CORS?

<ul>
<li class="fragment fade-in-then-out">❌ <code>Access-Control-Allow-Origin: *</code></li>
<li class="fragment fade-in-then-out">📒 https://jakearchibald.com/2021/cors/</li>
</ul>

Note:
- If you've been building apps with Javascript for some time, you've probably heard of CORS
- Cross Origin Resource Sharing
- If you're configuring CORS with * you're basically in the same boat
- CORS is to prevent this happening via Javascript, e.g. with fetch
- How to Win at CORS is everything you need to know about getting it right

---

### Cookie level protection

<ul>
<li class="fragment fade-in-then-out">It depends on your cookies 🍪</li>
<li class="fragment fade-in-then-out">Specifically the SameSite attribute</li>
<li class="fragment fade-in-then-out">Strict <pre>Set-Cookie: SESSIONID=xxxxx; SameSite=Strict</pre></li>
<li class="fragment fade-in-then-out">Lax or None</li>
</ul>

Note:
- If you've got your cookies set to Strict, you're OK, however anyone landing on a page from another site will appear to be logged out - fine for banks
- Particularly difficult for oauth flows

---

### Enter Remix

Note:
- Remix gets you thinking about actions in terms of the building blocks of the web, forms.
- So because we're submitting forms via straight up submit again, we need to think about CSRF if we've not got our cookies configured to SameSite: strict

---

### Remix example

@todo screenshots

Note: 
- Here I've setup the jokes example app
- There's a delete button on each joke

---

### Crafting the attack

todo show the demo here

Note:
- Here's the attack code
- Trick a logged in user into visiting the page
- Voila the joke is deleted without the user being aware
- Big deal is a joke, but what it was a banking transaction or something valuable?

---

### Mitigations

Enter the <em>Synchronizer Token Pattern</em>

Note:
- @todo describe the mechanics here
- form approach, custom header approach (even better)

---

### Implementing for Remix

@todo code samples

---

### 🚧 Warning: opinions ahead

---

### Remix needs this baked in

TODO update link
<ul>
<li class="fragment fade-in-then-out">https://github.com/remix-run/remix/issues/XXX</li>
</ul>

Note:
- I think Remix should bake this in using the more secure custom request
- It should be easy to generate a CSRF token on each form render, and attach it to the request
- The clientside code should handle sending it a custom header
- The action handler on the backend should validate it and reject invalid values
- @todo add an issue to add it

---

### Credits

- https://flickr.com/photos/tgerus/3807995646
- http://slides.com/gaborhojtsy/state-of-drupal-10-readiness-sept-2021
