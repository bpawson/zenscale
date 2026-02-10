# ZenScale v1.1

Traditional responsive breakpoints often require maintaining multiple layouts.
ZenScale instead scales a single, intentional design across viewport ranges,
while still allowing profile-specific tuning (desktop, mobile, etc).

## What this does
--------------
ZenScale lets you design your whole page at ONE “root width” (e.g. 1530px).
You perfect your layout at that width, and ZenScale scales the entire page
up and down smoothly to fit any screen size.

You can have more than one root width (e.g. desktop + mobile):
ZenScale v1.1 now includes both profiles inside ONE snippet, using clean
media queries and separate config values for each profile.

ZenScale automatically clamps height to the CURRENT viewport height,
so you never touch height settings again.

Included:
- zenscale.liquid (the full engine)
- README.md (this file)

You will:
1) Add the ZenScale wrappers under <body>
2) Close them before </body>
3) Render {% render 'zenscale' %} before </body>
4) Edit only the DESIGN_W / MIN_WIDTH / MAX_WIDTH values for each profile


--------------------------------------
1. Add the ZenScale wrapper <div>s
--------------------------------------

Open theme.liquid.

1) Find <body>. Example:
   <body class="gradient ...">

2) Immediately AFTER <body>, paste:

   <div id="zenscale-root">
     <div id="zenscale-scroll">
       <div id="zenscale-stage-wrap">
         <div id="zenscale-stage">
           <div id="zenscale-content">

3) At the BOTTOM of the file, ABOVE </body>, paste:

           </div> <!-- zenscale-content -->
         </div>   <!-- zenscale-stage -->
       </div>     <!-- zenscale-stage-wrap -->
     </div>       <!-- zenscale-scroll -->
   </div>         <!-- zenscale-root -->

   {% render 'zenscale' %}

Rules:
- Do NOT place {% render 'zenscale' %} inside the wrapper divs.
- Do NOT remove or wrap {{ content_for_layout }}.
- Only wrap the visual structure inside <body>.


--------------------------------------
2. Create the ZenScale snippet
--------------------------------------

In Shopify admin:
Online Store → Themes → Edit code → Snippets → Add a new snippet  
Name it: zenscale

Open zenscale.liquid from the ZIP → copy all → paste → Save.

The snippet contains:
- A base CSS block
- Two media-query root-width profiles (one desktop, one mobile)
- Two scale engines (desktop + mobile)
- Each engine has:
    • auto-height clamp  
    • scale  
    • alignment  

You never edit JS logic.
You only edit the CONFIG numbers at the top of each profile.


--------------------------------------
3. Configure your ZenScale settings
--------------------------------------

Inside zenscale.liquid you will see two config groups:

DESKTOP PROFILE CONFIG:
  const ZENSCALE_MIN_WIDTH1 = 700;
  const ZENSCALE_DESIGN_W1  = 1530;
  const ZENSCALE_STRENGTH1  = 0.5; //DO NOT CHNAGE THIS VALUE//

And in CSS:
  @media (min-width: 700px) {
    :root { --design-width: 1530px; }
  }

MOBILE PROFILE CONFIG:
  const ZENSCALE_MAX_WIDTH2 = 699;
  const ZENSCALE_DESIGN_W2  = 400;
  const ZENSCALE_STRENGTH2  = 0.5; //DO NOT CHANGE THIS VALUE//

And in CSS:
  @media (max-width: 699px) {
    :root { --design-width: 400px; }
  }

You MUST change these for your own design(s).


DESIGN_W
  The exact width you designed the layout at.
  Must match #zenscale-stage width in CSS.

MIN_WIDTH / MAX_WIDTH
  The viewport ranges where each scaling mode activates.

HEIGHT_CLAMP
  No longer used. ZenScale auto-clamps to window.innerHeight.

That’s it.


--------------------------------------
4. Multi-root setups (beyond desktop + mobile)
--------------------------------------

You can add as many profiles as you want (tablet, ultrawide, etc).

1) Copy the block from:
   /* GLOBAL ZENSCALE CONFIG */
to the end of its closing script.

2) Paste below the mobile engine.

3) Change ONLY:
   - design width  
   - min/max width ranges  
   - and the CSS @media query for that width  

ZenScale v1.1 keeps every profile fully isolated,
so each one runs ONLY in its own viewport range.


--------------------------------------
5. Scaling viewport-only elements (advanced)
--------------------------------------

If you have floating UI or overlay graphics that need separate scaling:

1) Add a second wrapper above the main ZenScale wrapper:

   <div id="zenscale-vp-root">
     <div id="zenscale-vp-scroll">
       <div id="zenscale-vp-stage-wrap">
         <div id="zenscale-vp-stage">
           <div id="zenscale-vp-content">

2) Create: zenscale-viewport snippet

3) Copy the ENTIRE ZenScale snippet into it.

4) Change all IDs:
   zenscale-root        → zenscale-vp-root
   zenscale-scroll      → zenscale-vp-scroll
   zenscale-stage-wrap  → zenscale-vp-stage-wrap
   zenscale-stage       → zenscale-vp-stage
   zenscale-content     → zenscale-vp-content

5) Update the config values for this separate profile.

6) Render both before </body>:

   {% render 'zenscale-viewport' %}
   {% render 'zenscale' %}


--------------------------------------
6. Warning about Shopify media queries
--------------------------------------

Your theme likely has MANY responsive breakpoints like:
1200px, 990px, 750px, 600px, 480px, etc.

These CAN override your layout:
- hide sections
- change padding
- change flexbox layout
- switch images
- collapse blocks

ZenScale does NOT override your theme’s CSS.

If theme media queries conflict with ZenScale:
- remove or neutralize them, OR
- design your layout so they don’t break scaling, OR
- add additional ZenScale profiles.


--------------------------------------
7. Troubleshooting
--------------------------------------

Footer stops early:
- something is outside the ZenScale wrapper

Extra blank space appears:
- check the wrapper structure  
- check snippet is rendered AFTER the wrappers

Scaling only works at certain widths:
- viewport is outside that profile's min/max range

Nothing scales:
- snippet not rendered before </body>
- wrapper IDs don’t match
- theme media queries override transforms


--------------------------------------
Done.
--------------------------------------

Design once at your root width, plug in ZenScale,
and the whole page scales perfectly on every device.
