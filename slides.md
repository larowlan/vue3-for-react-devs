
## Avoiding CSRF with Remix

---

### About me

<span class="underline">@larowlan</span>

<div class="fragment fade-in">💻️We're hiring!</div>

Note:

- Senior Developer at PreviousNext
- PHP Development for 22 years
- Major contributor to Drupal, part of the Drupal security team
- Lots of Javascript last 4-5 years

---

### WTF is CSRF

<div class="fragment fade-in"><span class="underline">Cross Site Request Forgery</span></div>
<blockquote class="fragment fade-in-then-out">
... is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated
</blockquote>
<span class="small">https://owasp.org/www-community/attacks/csrf</span>


Note:

- Like most security exploits, it has an acronym
- Let's start by talking about what it's not
- And I know you're saying, yes those are English words, but what do they mean

---

### Simplest (worst) example

<pre class="fragment fade-in-then-out">
    <code class="language-javascript hljs">
        import express from "express"
        import { checkLogin } from "@app/user/auth"
        import { favouritesRepository } from "@app/model/favourites"
        const app = express()
    
        app.get("/favourites/delete/:favouriteId", (req, res) => {
            if (!checkLogin()) {
                return res.sendStatus(403);
            }
            favouritesRepository.deleteById(req.params.favouriteId)
            res.send("OK!")
        })
    </code>
</pre>

Note:
- And before you say no-one would write that code, I've seen this in apps I've audited
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
<!-- Visited site -->
<iframe height="0" width="0" src="https://evil.com/trigger.html"></iframe>

<!-- trigger.html -->
<form id="form" action="https://yoursite.com/favourites/delete/3">
    <input name="favouriteId" value="3" type="hidden"/>
    <input type="submit" />
</form>
<script type="text/javscript">
    window.onload = () => {
      document.forms[0].submit();
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

### Enter Remix

Note:
- Remix gets you thinking about actions in terms of the building blocks of the web, forms.
- If you've been building apps with Javascript you've probably been using fetch or axios, and in those scenarios CORS protects you
unless you're configuring CORS with * you're basically in the same boat
- So because we're submitting forms via straight up submit again, we need to think about CSRF again

---

### Remix example

<pre>
<code class="language-jsx hljs">
const Form = ({}) => {
  return (
    <Form method="post">
      <label htmlFor="name">Name</label>
      <input name="name" type="text" id="name"/>
      <input type="submit" value="Add attendee"/>
    </Form>
  );
}
</code>
</pre>

Note:
- So lets consider a primitive Remix form
- We've got a text field and a submit button

---

### Remix example

<pre>
<code class="language-javascript hljs">
export const loader = async ({ request }) => {
  const userId = await getUserSession(request);
  if (!userId) {
    throw new Response("Unauthorized", { status: 401 });
  }
  return json({});
};
</code>
</pre>

Note:
- And we're assuming we've got this behind an authentication layer
- We're not going into details here, but tl;dr our loader function checks that the user has a session

---

### Remix example

<pre>
<code class="language-javascript hljs">
export const action = async({request}) => {
  const formData = await request.formData();
  // Act on the form submission.
  // E.g. write to the database
  return null;
}

</code>
</pre>

Note:
- And of course we've go an action function to handle our submission.
- Nothing specific here, but assume you're performing some sort of action like writing to the DB

---

### Crafting the attack

Nothing special here
<pre>
<code class="language-html hljs">
<!-- Visited site -->
<iframe height="0" width="0" src="https://evil.com/trigger.html"></iframe>

<!-- trigger.html -->
<form action="https://yoursite.com/that-form">
    <input name="name" value="danny" type="hidden"/>
    <input type="submit" />
</form>
<script type="text/javscript">
    window.onload = () => {
      document.forms[0].submit();
    };
</script>
</code>

</pre>

Note:
- Here's the attack code
- Trick a logged in user into visiting the page
- Voila the action is completed without the user being aware

---

### Avoiding CSRF with Remix

<pre><code>
🍪 SetCookie: session=...; SameSite=Lax
🍪 SetCookie: session=...; SameSite=Strict
</code></pre>
<div class="fragment">*Used in tutorials, but not in docs</div>

---

### But...

> This attribute should not replace having a CSRF Token.<br>
> Instead, it should co-exist with that token in order to protect the user in a more robust way.

<p class="small">OWASP</p>

---

### Browser support

https://caniuse.com/same-site-cookie-attribute

#### <span class="underline">93%</span> of users

<div class="fragment">We're almost there!</div>

---

### None, Lax or Strict?

When from a different domain:
<ul>
<li class="fragment fade-in">SameSite:<span class="underline">None</span> - always send</li>
<li class="fragment fade-in">SameSite:<span class="underline">Lax</span> - GET only</li>
<li class="fragment fade-in">Samesite:<span class="underline">Strict</span> - send nothing</li>
</ul>

Note:
- If you've got your cookies set to Strict and you can be sure your user's browsers support SameSite attribute, you're OK
- Strict - landing on a page from another site will appear to be logged out - fine for banks
- Particularly difficult for oauth flows

---

### Token based mitigation

Enter the <em>Synchronizer Token Pattern</em>

<ul>
<li class="fade-in fragment">Generate a <span class="underline">unique token</span> per session per form</li>
<li class="fade-in fragment">Transmit that with the form</li>
<li class="fade-in fragment">Send it back</li>
<li class="fade-in fragment"><span class="underline">Securely</span> validate the token server side</li>
</ul>

Note:
- Use a secure random library to generate a strong token
- Either send it as a custom header or as a hidden field in the form

---

### Start with secure random keys

<ul>
<li class="fade-in fragment">A <span class="underline">hash salt</span></li>
<li class="fade-in fragment">A <span class="underline">private key</span></li>
</ul>
<pre class="fade-in fragment">
<code class="language-javascript hljs">
import { randomBytes } from "crypto";
randomBytes(55).toString("base64");
</code>
</pre>

Note:
- the hash salt should not be stored in/with your database 
- should have one private key per environment

---

### Implementing for Remix

Generate a <span class="underline">unique random seed</span> per session

<pre class="fade-in-then-out fragment">
<code class="language-javascript hljs">
import { randomBytes } from "crypto";

const getCSRFSeed = (session) => {
  let seed = session.get("csrf_seed");
  if (seed) {
    return seed;
  }
  seed = randomBytes(32).toString("base64");
  session.set("csrf_seed", seed);
  return seed;
}
</code>
</pre>

---

### Implementing for Remix

Generate a unique token <span class="underline">per session and identifier</span>

<pre class="fade-in-then-out fragment">
<code class="language-javascript hljs">
import { creatHmac } from "crypto";

export const createCSRFToken = (identifier, session) => {
  const { HASH_SALT, PRIVATE_KEY } = process.env;
  const seed = getCSRFSeed(session);
  const secret = `${HASH_SALT}${PRIVATE_KEY}${seed}`
  const hmac = crypto.createHmac("sha256", secret);
  const data = hmac.update(identifier);
  return data.digest("hex");
}
    </code>
</pre>

Note:
- In this scenario identifier would be e.g a unique identifier for the form or operation
- This prevents token reuse for the same session

---

### Implementing for Remix

Transmit with the form - Step 1 - loader

<pre class="fade-in-then-out fragment">
<code class="language-javascript hljs">
export const loader = async ({ request }) => {
  const session = await getSession(request.headers.get("Cookie"));
  const csrf = createCSRFToken(__filename, session);
  return json(
    { csrf },
    { headers: { "Set-Cookie": await commitSession(session) } }
  );
};
</code>
</pre>

---

### Implementing for Remix

Transmit with the form - step 2 - form

<pre class="fade-in-then-out fragment">
<code class="language-jsx hljs">
const Form = () => {
  const { csrf } = useRouteData();
   return (
    <Form method="post">
      <input type="hidden" name="form_token" value={csrf} />
      {/* Rest of the form */}
    </Form>
  );
};

}
</code>
</pre>

---

### Implementing for Remix

Validation - the code

<pre class="fade-in-then-out fragment">
<code class="language-javascript hljs">
import { timingSafeEqual } from "crypto";
import { Buffer } from "buffer";
export const validateCSRFToken = async (actual, identifier, session) => {
  const expected = createCSRFToken(identifier, session);
  if (!timingSafeEqual(Buffer.from(expected), Buffer.from(actual))
    throw new Error("CSRF tokens do not match.");
  }
}
</code>
</pre>

Note:
- timingSafeEquals here to avoid timing attacks

---


### Implementing for Remix

Validation - in the action

<pre class="fade-in-then-out fragment">
<code class="language-javascript hljs">
export const action = async ({ request }) => {
  const session = await getSession(request.headers.get("Cookie"));
  try {
    const body = await request.formData();
    await validateCSRFToken(body.get("form_token"), __filename, session);
  } catch (error) {
    session.flash("error", "Invalid security token");
    return redirect("/error-page", {
      headers: { "Set-Cookie": await commitSession(session) },
    });
  }
}
</code>
</pre>

---

### Questions?

