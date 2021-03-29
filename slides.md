<!-- .slide: data-background="./images/brushes.jpg" -->
### ⚡️draft.js, editor.js, slate.js 
#### choosing the best text editor for your React project

---

### ️👟 Quick overiew

Note:

- Lightning talk
- Introduce you to three options
- Discuss their strengths
- Data model

---

### ✅ The editors

<ul class="pencil-list">
<li class="fragment fade-in-then-semi-out">draft.js</li>
<li class="fragment fade-in-then-semi-out">editor.js</li>
<li class="fragment">slate.js</li>
</ul>

---

### 🔦 Criteria

<ul class="two-columns criteria">
<li class="💰 fragment fade-in-then-semi-out">sustainability</li>
<li class="👵 fragment fade-in-then-semi-out">maturity</li>
<li class="⚖️ fragment fade-in-then-semi-out">license</li>
<li class="✨ fragment fade-in-then-semi-out">editor features</li>
<li class="♻️ fragment fade-in-then-semi-out">release cycle</li>
<li class="🏗 fragment fade-in-then-semi-out">data structure</li>
<li class="🧱 fragment fade-in-then-semi-out">ecosystem</li>
<li class="🌏 fragment fade-in-then-semi-out">browser support</li>
<li class="📦 fragment fade-in-then-semi-out">used by</li>
<li class="⭐ fragment">github stars</li>
</ul>

---

### ✏️ Editor features

<ul class="two-columns criteria">
<li class="yep fragment fade-in-then-semi-out">block styles</li>
<li class="yep fragment fade-in-then-semi-out">inline styles</li>
<li class="yep fragment fade-in-then-semi-out">undo/redo</li>
<li class="yep fragment fade-in-then-semi-out">paste</li>
<li class="yep fragment fade-in-then-semi-out">lists</li>
<li class="yep fragment fade-in-then-semi-out">nested blocks</li>
<li class="yep fragment fade-in-then-semi-out">media</li>
<li class="yep fragment fade-in-then-semi-out">tables</li>
<li class="yep fragment">links</li>
</ul>

---

<!-- .slide: class="draftjs" -->
### draft.js

https://draftjs.org/

Note:

- Used by facebook messenger, comments, status and notes in production
- A lot of the features will require custom code, this isn't CKEditor

---

<!-- .slide: class="draftjs" -->
### draft.js

<ul class="two-columns criteria">
<li class="💰 fragment fade-in-then-semi-out">Facebook</li>
<li class="👵 fragment fade-in-then-semi-out">0.11.7<small>🤔</small></li>
<li class="⚖️ fragment fade-in-then-semi-out">MIT</li>
<li class="✨ fragment fade-in-then-semi-out">Bare metal🤘</li>
<li class="♻️ fragment fade-in-then-semi-out">Semver</li>
<li class="🏗 fragment fade-in-then-semi-out">JSON</li>
<li class="🧱 fragment fade-in-then-semi-out">Full editor</li>
<li class="🌏 fragment fade-in-then-semi-out"><del>IE11</del>❓Mobile</li>
<li class="📦 fragment fade-in-then-semi-out">83.8k</li>
<li class="⭐ fragment">20k</li>
</ul>

---

<!-- .slide: class="draftjs" -->
### draft.js - data

<pre><code class="javascript">
{
  "entityMap": {
    "0": {
      "type": "SOMETHING",
      "mutability": "IMMUTABLE",
      "data": {
        some: 'prop',
      }
    },
  },
  "blocks": [
    {
      "key": "34egr",
      "text": "This is an immutable entity: hi there.",
      "type": "unstyled",
      "depth": 0,
      "inlineStyleRanges": [],
      "entityRanges": [
        {
          "offset": 29,
          "length": 8,
          "key": 0
        }
      ],
      "data": {}
    }
  ]
}
</code></pre>

---

<!-- .slide: class="draftjs" -->
### draft.js - features

<ul class="two-columns criteria">
<li class="yep fragment fade-in-then-semi-out">block styles</li>
<li class="yep fragment fade-in-then-semi-out">inline styles</li>
<li class="yep fragment fade-in-then-semi-out">undo/redo</li>
<li class="yep fragment fade-in-then-semi-out">paste</li>
<li class="yep fragment fade-in-then-semi-out">lists</li>
<li class="🤔 fragment fade-in-then-semi-out">nested blocks</li>
<li class="yep fragment fade-in-then-semi-out">media</li>
<li class="nope fragment fade-in-then-semi-out">tables</li>
<li class="yep fragment">links</li>
</ul>

---

<!-- .slide: class="editorjs" -->
### editor.js

https://editorjs.io/

Note:

- Powers a few media orgs, Russian language
- Vanilla JavaScript but with React bridges

---

<!-- .slide: class="editorjs" -->
### editor.js

<ul class="two-columns criteria">
<li class="💰 fragment fade-in-then-semi-out">CodeX</li>
<li class="👵 fragment fade-in-then-semi-out">2.19.3</li>
<li class="⚖️ fragment fade-in-then-semi-out">Apache 2.0</li>
<li class="✨ fragment fade-in-then-semi-out">Block based editor</li>
<li class="♻️ fragment fade-in-then-semi-out">Semver</li>
<li class="🏗 fragment fade-in-then-semi-out">JSON</li>
<li class="🧱 fragment fade-in-then-semi-out">Lots of plugins</li>
<li class="📦 fragment fade-in-then-semi-out">1.5k</li>
<li class="⭐ fragment">15k</li>
</ul>

---

<!-- .slide: class="editorjs" -->
### editor.js - data

<pre><code class="javascript">
{
   "time": 1550476186479,
   "blocks": [
      {
         "type": "header",
         "data": {
            "text": "Editor.js",
            "level": 2
         }
      },
      {
         "type": "paragraph",
         "data": {
            "text": "Hey. Meet the new Editor. On this page you can see it in action — try to edit this text. Source code of the page contains the example of connection and configuration."
         }
      },
    ]
}
</code></pre>

---

<!-- .slide: class="editorjs" -->
### editor.js - features

<ul class="two-columns criteria">
<li class="yep fragment fade-in-then-semi-out">block styles</li>
<li class="yep fragment fade-in-then-semi-out">inline styles</li>
<li class="yep fragment fade-in-then-semi-out">undo/redo</li>
<li class="yep fragment fade-in-then-semi-out">paste</li>
<li class="yep fragment fade-in-then-semi-out">lists</li>
<li class="yep fragment fade-in-then-semi-out">nested blocks</li>
<li class="yep fragment fade-in-then-semi-out">media</li>
<li class="🤞 fragment fade-in-then-semi-out">tables</li>
<li class="yep fragment fade-in-then-semi-out">links</li>
</ul>

---

<!-- .slide: class="slatejs" -->
### slate.js

https://docs.slatejs.org/

Note:

- Specifically built to address pain points
- Nesting support
- Collaborative as a first-class citizen
- Not specifically React, but first-class library

---

<!-- .slide: class="slatejs" -->
### slate.js

<ul class="two-columns criteria">
<li class="💰 fragment fade-in-then-semi-out">ianstormtaylor</li>
<li class="👵 fragment fade-in-then-semi-out">0.19.0<small>🤔</small></li>
<li class="⚖️ fragment fade-in-then-semi-out">MIT</li>
<li class="✨ fragment fade-in-then-semi-out">Bare Metal🤘</li>
<li class="♻️ fragment fade-in-then-semi-out">Semver</li>
<li class="🏗 fragment fade-in-then-semi-out">JSON</li>
<li class="🧱 fragment fade-in-then-semi-out">Example code</li>
<li class="📦 fragment fade-in-then-semi-out">55.2k</li>
<li class="⭐ fragment">20.2k</li>
</ul>

---

<!-- .slide: class="slatejs" -->
### slate.js - data

<pre><code class="javascript">
[{
  type: 'paragraph',
  children: [
    { text: 'An opening paragraph with a ' },
    {
      type: 'link',
      url: 'https://example.com',
      children: [{ text: 'link' }],
    },
    { text: ' in it.' },
  ],
},
{
  type: 'quote',
  children: [{ text: 'A wise quote.' }],
},
{
  type: 'paragraph',
  children: [{ text: 'A closing paragraph!' }],
}]
</code></pre>

---

<!-- .slide: class="slatejs" -->
### slate.js - features

<ul class="two-columns criteria">
<li class="yep fragment fade-in-then-semi-out">block styles</li>
<li class="yep fragment fade-in-then-semi-out">inline styles</li>
<li class="yep fragment fade-in-then-semi-out">undo/redo</li>
<li class="yep fragment fade-in-then-semi-out">paste</li>
<li class="yep fragment fade-in-then-semi-out">lists</li>
<li class="yep fragment fade-in-then-semi-out">nested blocks</li>
<li class="yep fragment fade-in-then-semi-out">media</li>
<li class="yep fragment fade-in-then-semi-out">tables</li>
<li class="yep fragment fade-in-then-semi-out">links</li>
</ul>

Note:

- Like draftjs, most of these will require following along with examples

---

### Summary 📋

Note:

* if you want stability of a large org, use Facebook's draftjs, don't expect nesting
* if you want to get going fast and a rich set of features out of the box - use editorjs
* if you want ultimate control, nested support and leave the door open for collaboration - use slatejs
