---
layout: page
title: Books
---

<!-- Generated from _data/books.yml. Add a book there, not here. -->

<div class="cf" id="cf" hidden>
  <div class="cf-viewport" id="cfViewport" tabindex="0" role="listbox" aria-label="Book covers">
    <div class="cf-stage" id="cfStage"></div>
  </div>

  <div class="cf-meta">
    <div class="cf-year" id="cfYear"></div>
    <a class="cf-title" id="cfTitle" href="#"></a>
    <div class="cf-author" id="cfAuthor"></div>
    <p class="cf-note" id="cfNote"></p>
  </div>

  <div class="cf-controls">
    <button type="button" class="cf-arrow" id="cfPrev" aria-label="Previous book">&#8249;</button>
    <div class="cf-years" id="cfYears"></div>
    <button type="button" class="cf-arrow" id="cfNext" aria-label="Next book">&#8250;</button>
  </div>
</div>

{% assign groups = site.data.books | group_by: "year" %}
{% for g in groups %}
<h2 id="{{ g.name }}">{{ g.name }}</h2>

<ul class="book-list">
{% for b in g.items %}
<li class="book-item">
  <div class="book-header">
    <a href="/books/{{ b.slug }}">{{ b.title }}</a>
  </div>
  <img src="/assets/assets/images/books/{{ b.slug }}.jpg" alt="{{ b.title }} cover" class="book-thumb">
  <p class="book-note">{{ b.note }}</p>
</li>
{% endfor %}
</ul>
{% endfor %}

<script>window.BOOKS = {{ site.data.books | jsonify }};</script>

<style>
/* ---------- coverflow ---------- */
.cf{
  --cf-fg:#e8eef7; --cf-dim:#93a4bd; --cf-line:#2a3446; --cf-accent:#7dd3fc;
  margin:0 0 3em; user-select:none;
}
@media (prefers-color-scheme: light){
  .cf{ --cf-fg:#16202e; --cf-dim:#5b6b80; --cf-line:#d7dee8; --cf-accent:#0369a1; }
}
.cf[hidden]{display:none}

.cf-viewport{
  position:relative; height:470px; perspective:1250px; perspective-origin:50% 38%;
  overflow:hidden; outline:none; cursor:grab; touch-action:pan-y;
}
.cf-viewport:active{cursor:grabbing}
.cf-viewport:focus-visible{outline:2px solid var(--cf-accent); outline-offset:3px; border-radius:8px}

.cf-stage{position:absolute; inset:0; transform-style:preserve-3d}

.cf-item{
  position:absolute; top:22px; left:50%; width:190px; height:285px;
  margin-left:-95px; transform-style:flat; backface-visibility:visible;
  transition:transform .45s cubic-bezier(.22,.68,.31,1), opacity .45s ease;
  will-change:transform; text-decoration:none; display:block;
}
.cf-item .cover{
  width:100%; height:100%; object-fit:cover; display:block; border-radius:3px;
  box-shadow:0 18px 34px rgba(0,0,0,.55); background:#222;
}
/* the reflection: a flipped copy sitting directly under the cover, faded out.
   scaleY(-1) about the default centre origin keeps the box in place and only
   mirrors the pixels, so the cover's BOTTOM edge meets the cover. */
.cf-item .refl{
  position:absolute; left:0; top:100%; width:100%; height:100%;
  object-fit:cover; display:block; border-radius:3px;
  transform:scaleY(-1); opacity:.32; pointer-events:none;
  /* scaleY(-1) mirrors the mask along with the pixels, so this gradient reads
     inverted: the opaque stop sits at the gradient's start, which after the
     flip lands against the bottom of the cover. Verified by pixel sampling. */
  -webkit-mask-image:linear-gradient(to top, rgba(0,0,0,.95), transparent 58%);
  mask-image:linear-gradient(to top, rgba(0,0,0,.95), transparent 58%);
}
.cf-item.is-hidden{opacity:0; pointer-events:none}
.cf-item.is-center .cover{box-shadow:0 22px 46px rgba(0,0,0,.65), 0 0 0 1px rgba(255,255,255,.08)}

/* ---------- caption ---------- */
.cf-meta{text-align:center; margin-top:.4em; min-height:6.2em}
.cf-year{
  font-size:.72em; letter-spacing:.18em; color:var(--cf-dim); margin-bottom:.35em;
}
.cf-title{
  display:inline-block; font-size:1.35em; font-weight:700; color:var(--cf-fg);
  text-decoration:none; line-height:1.2;
}
.cf-title:hover{text-decoration:underline}
.cf-author{color:var(--cf-dim); font-size:.9em; margin-top:.15em}
.cf-note{
  color:var(--cf-dim); font-size:.86em; max-width:56ch; margin:.7em auto 0; line-height:1.5;
}

/* ---------- controls ---------- */
.cf-controls{display:flex; align-items:center; justify-content:center; gap:14px; margin-top:1em; flex-wrap:wrap}
.cf-arrow{
  font:inherit; font-size:1.5em; line-height:1; color:var(--cf-dim); background:transparent;
  border:1px solid var(--cf-line); border-radius:50%; width:40px; height:40px; cursor:pointer;
  display:flex; align-items:center; justify-content:center; padding:0 0 3px;
}
.cf-arrow:hover{color:var(--cf-fg); border-color:var(--cf-accent)}
.cf-years{display:flex; gap:7px}
.cf-years button{
  font:inherit; font-size:.8em; letter-spacing:.06em; color:var(--cf-dim); background:transparent;
  border:1px solid var(--cf-line); border-radius:999px; padding:6px 13px; cursor:pointer;
}
.cf-years button:hover{color:var(--cf-fg)}
.cf-years button[aria-current="true"]{color:var(--cf-fg); border-color:var(--cf-accent)}

@media (max-width:640px){
  .cf-viewport{height:370px; perspective:900px}
  .cf-item{width:150px; height:225px; margin-left:-75px; top:18px}
  .cf-note{display:none}
}
@media (prefers-reduced-motion: reduce){
  .cf-item{transition:none}
}

/* ---------- the list below ---------- */
.book-list{list-style:none; padding:0; margin:0}
.book-item{margin-bottom:1.5em; padding:1em; border:1px dashed #444}
.book-header{display:flex; justify-content:space-between; align-items:baseline; gap:1em; margin-bottom:0.6em}
.book-thumb{width:100px; display:block; margin-bottom:0.6em}
.book-note{margin:0; font-size:0.9em}
</style>

<script>
(function(){
  "use strict";
  var books = window.BOOKS || [];
  if (!books.length || !window.requestAnimationFrame) return;   // list-only fallback

  var cf      = document.getElementById("cf"),
      stage   = document.getElementById("cfStage"),
      viewport= document.getElementById("cfViewport"),
      elYear  = document.getElementById("cfYear"),
      elTitle = document.getElementById("cfTitle"),
      elAuthor= document.getElementById("cfAuthor"),
      elNote  = document.getElementById("cfNote"),
      elYears = document.getElementById("cfYears");

  var idx = 0, items = [];

  /* geometry: how far each card sits from the one in front of it */
  var GAP = 148, DEPTH = 150, TILT = 54, VISIBLE = 4;

  books.forEach(function(b, i){
    var a = document.createElement("a");
    a.className = "cf-item";
    a.href = "/books/" + b.slug;
    a.setAttribute("role","option");
    a.setAttribute("aria-label", b.title + ", " + b.author);
    var src = "/assets/assets/images/books/" + b.slug + ".jpg";
    var img = new Image();
    img.src = src; img.alt = b.title + " cover"; img.className = "cover"; img.loading = "lazy";
    var refl = new Image();
    refl.src = src; refl.alt = ""; refl.className = "refl"; refl.loading = "lazy";
    refl.setAttribute("aria-hidden", "true");
    a.appendChild(img); a.appendChild(refl);
    a.addEventListener("click", function(e){
      /* a side cover centres itself; only the centre one follows its link */
      if (i !== idx){ e.preventDefault(); go(i); }
    });
    stage.appendChild(a);
    items.push(a);
  });

  var N = books.length;

  /* shortest signed distance around the ring, so the strip never runs out of
     covers on one side */
  function delta(i){
    var d = (i - idx) % N;
    if (d >  N / 2) d -= N;
    if (d < -N / 2) d += N;
    return d;
  }

  function layout(){
    items.forEach(function(el, i){
      var d = delta(i), ad = Math.abs(d);
      if (ad > VISIBLE){ el.classList.add("is-hidden"); el.style.transform = "translateX(0) translateZ(-900px)"; return; }
      el.classList.remove("is-hidden");
      el.classList.toggle("is-center", d === 0);

      var x, z, ry, sc;
      if (d === 0){ x = 0; z = 0; ry = 0; sc = 1; }
      else {
        var sign = d > 0 ? 1 : -1;
        /* first neighbour steps out furthest, the rest stack tightly behind it */
        x  = sign * (GAP + (ad - 1) * 40);
        z  = -DEPTH - (ad - 1) * 70;
        ry = -sign * TILT;
        sc = 1 - Math.min(ad * 0.055, 0.2);
      }
      el.style.zIndex = String(100 - ad);
      el.style.transform =
        "translateX(" + x + "px) translateZ(" + z + "px) rotateY(" + ry + "deg) scale(" + sc + ")";
      el.setAttribute("aria-selected", d === 0 ? "true" : "false");
    });

    var b = books[idx];
    elYear.textContent   = b.year;
    elTitle.textContent  = b.title;
    elTitle.href         = "/books/" + b.slug;
    elAuthor.textContent = b.author;
    elNote.textContent   = b.note;
    viewport.setAttribute("aria-activedescendant", "");

    Array.prototype.forEach.call(elYears.children, function(btn){
      btn.setAttribute("aria-current", btn.dataset.year === String(b.year) ? "true" : "false");
    });
  }

  function go(i){
    idx = ((i % N) + N) % N;
    layout();
  }

  /* year chips jump to the first book of that year */
  var seen = {};
  books.forEach(function(b, i){
    if (seen[b.year] !== undefined) return;
    seen[b.year] = i;
    var btn = document.createElement("button");
    btn.type = "button"; btn.textContent = b.year; btn.dataset.year = b.year;
    btn.addEventListener("click", function(){ go(i); viewport.focus(); });
    elYears.appendChild(btn);
  });

  document.getElementById("cfPrev").addEventListener("click", function(){ go(idx - 1); });
  document.getElementById("cfNext").addEventListener("click", function(){ go(idx + 1); });

  viewport.addEventListener("keydown", function(e){
    if (e.key === "ArrowLeft"){ e.preventDefault(); go(idx - 1); }
    else if (e.key === "ArrowRight"){ e.preventDefault(); go(idx + 1); }
    else if (e.key === "Home"){ e.preventDefault(); go(0); }
    else if (e.key === "End"){ e.preventDefault(); go(N - 1); }
    else if (e.key === "Enter"){ location.href = "/books/" + books[idx].slug; }
  });

  /* drag and swipe */
  var down = null;
  function start(x){ down = { x:x, from:idx, moved:false }; }
  function move(x){
    if (!down) return;
    var step = Math.round((down.x - x) / 78);
    if (step !== 0) down.moved = true;
    var want = down.from + step;
    if (want !== idx) go(want);
  }
  function end(){ down = null; }

  viewport.addEventListener("pointerdown", function(e){ start(e.clientX); viewport.setPointerCapture(e.pointerId); });
  viewport.addEventListener("pointermove", function(e){ if (down) { e.preventDefault(); move(e.clientX); } });
  viewport.addEventListener("pointerup", end);
  viewport.addEventListener("pointercancel", end);
  /* a drag must not also open the book underneath the cursor */
  viewport.addEventListener("click", function(e){
    if (down && down.moved){ e.preventDefault(); e.stopPropagation(); }
  }, true);

  /* horizontal wheel / trackpad */
  var wheelLock = 0;
  viewport.addEventListener("wheel", function(e){
    var d = Math.abs(e.deltaX) > Math.abs(e.deltaY) ? e.deltaX : 0;
    if (!d) return;
    e.preventDefault();
    var now = Date.now();
    if (now - wheelLock < 220) return;
    wheelLock = now;
    go(idx + (d > 0 ? 1 : -1));
  }, { passive:false });

  cf.hidden = false;
  layout();
})();
</script>
