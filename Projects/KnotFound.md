---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
KnotFound

404 Page we can do yarn rolling over or smthg ^MOriGTd3

I want to sell my crochet products and advertise them on a website:
    - Right now only these ideas I have
But basic image is:
    - FE driven website wherein we can add products based on config - No need of BE since free hosting is having slow start issues. We will showcase our products and redirect them to Instagram for DM and purchases. Got it?
    - So i need a prompt for this to give to Google stitch to generate slides to finalize the design. I hope you are getting the idea and vision Im going for - more like a person who is dev (who codes) buts crochets as well. ^ihP8DNpA

Frames:
 ^FzAjN07F

KnotFound — Production-Grade Frontend Implementation Specification

0. Project Overview

Role

You are an expert Frontend Architect and Senior UI Engineer responsible for building a production-quality, responsive web application.

Objective

Implement KnotFound, an artisanal crochet brand created by a developer, blending cozy, handcrafted amigurumi products with clean developer/terminal UI aesthetics.

The application should combine the warmth and personality of handmade crochet with the precision, visual language, and interactive elements of a modern developer environment.

The result must be visually polished, interactive, accessible, responsive, modular, and maintainable.

Reference Screenshots

Three high-fidelity reference screenshots are provided:

1. 01_home.png — Home Page
    * Landing page with a terminal showcase.
    * Live timers and studio telemetry.
    * Interactive stitch compiler state machine.
    * Featured product grid.
2. 02_shop_collection.png — Shop Collection
    * Product catalog with terminal-inspired filter pipeline.
    * Inventory telemetry.
    * Product cards with Instagram DM adoption CTAs.
3. 03_product_detail.png — Product Detail
    * Product detail page for strawberry_butter_bunny.obj.
    * IDE-style breadcrumbs and build telemetry.
    * Multi-angle product inspector.
    * Adoption protocol and studio recommendations.

Primary visual reference: Use the provided screenshots as the source of truth for the intended visual design, layout, proportions, spacing, component hierarchy, and overall aesthetic. The written specifications below define the exact content, design tokens, behavior, and implementation requirements.

Do not omit any specified feature merely because it is not immediately apparent in the screenshots.

⸻

1. Technology Stack and Engineering Standards

1.1 Framework

Use either of the following:

* Next.js with the App Router and React 18 or 19.
* Vite with React and TypeScript.

Enable TypeScript strict mode.

Choose the framework that best fits the existing repository. If starting from scratch, select one and use it consistently throughout the implementation.

Do not mix incompatible framework conventions.

1.2 Styling

Use:

* Tailwind CSS v3 or v4.
* Arbitrary Tailwind values where necessary.
* Extended theme tokens for the design system.

All colors, typography, borders, surfaces, and other recurring design properties should be centralized wherever practical.

Avoid scattered, inconsistent styling values.

1.3 Icons

Use either:

* Lucide React (lucide-react).
* Crisp inline SVGs with appropriate accessibility attributes.

Decorative icons must use aria-hidden="true" where appropriate. Interactive icon-only buttons must have descriptive accessible names through aria-label or equivalent accessible labeling.

1.4 Component Architecture

Build a modular component system with reusable, independently maintainable components.

Use TypeScript interfaces for component props where appropriate.

Example:

interface Props {
  title: string;
  price: number;
}

Avoid:

* Duplicated component implementations.
* Large monolithic page components.
* Unnecessary prop drilling.
* Untyped component props.
* Mixing unrelated responsibilities within the same component.

Shared UI patterns should be implemented once and reused across all applicable pages.

1.5 State Management

Use React state and lifecycle hooks as appropriate:

* useState
* useEffect
* useMemo

Implement real interactions for:

* Live counters and timers.
* Product category filtering.
* Product image selection.
* Stitch compiler state transitions.
* Auto-loop controls.
* Navigation between pages.
* Product detail navigation.

Interactive elements must produce meaningful behavior rather than acting as static visual decorations.

1.6 Accessibility

Use semantic HTML throughout the application.

Appropriate elements include:

* <header>
* <nav>
* <main>
* <section>
* <article>
* <footer>

Requirements:

* All interactive controls must be keyboard accessible.
* Icon-only buttons must have accessible names.
* Images must have appropriate alternative text.
* Text and interactive elements must maintain sufficient contrast.
* Focus states must be clearly visible.
* Navigation must use appropriate links.
* Actions must use buttons rather than clickable non-semantic elements.
* Interactive controls must communicate their purpose and state to assistive technologies where appropriate.

1.7 Production Quality

The application must be:

* Responsive across desktop, tablet, and mobile.
* Type-safe.
* Modular and reusable.
* Accessible.
* Visually consistent.
* Free of avoidable console errors.
* Free of broken internal navigation.
* Free of placeholder interactions presented as completed functionality.

Use clean component exports and maintain a predictable project structure.

⸻

2. Design System

2.1 Design Direction

The visual language combines:

* Deep midnight slate surfaces.
* Catppuccin Mocha-inspired syntax colors.
* Editorial serif typography.
* Developer-oriented monospace labels.
* Mac terminal window chrome.
* Cozy, handcrafted crochet product imagery.
* Subtle IDE-inspired metadata and technical annotations.

The overall experience should feel like a developer’s personal crochet studio brought to life as an interactive digital workspace.

Important: Do not guess, substitute, or casually modify the specified colors. Use the exact hex values below.

2.2 Color Tokens

Define the following tokens in the global stylesheet.

:root {
  /* Canvas & Backgrounds */
  --bg-canvas: #10131c;
  --bg-surface: #161922;
  --bg-surface-elevated: #1e2230;
  --bg-surface-high: #252b3d;
  --bg-surface-input: #0e1117;
  /* Borders & Dividers */
  --border-subtle: #2b3145;
  --border-light: rgba(255, 255, 255, 0.08);
  --border-focus: #89b4fa;
  /* Syntax & Accent Colors */
  --accent-blue: #89b4fa;
  --accent-blue-hover: #b4befe;
  --accent-green: #a6e3a1;
  --accent-pink: #f38ba8;
  --accent-yellow: #f9e2af;
  --accent-lavender: #cba6f7;
  --accent-subtext: #9399b2;
  /* Typography Colors */
  --text-primary: #f5f5f7;
  --text-body: #cdd6f4;
  --text-muted: #7982a0;
  --text-subtle: #565d75;
}

Token Usage

Token        Intended Usage
--bg-canvas        Primary deep midnight slate canvas
--bg-surface        Primary page background
--bg-surface-elevated        Terminal windows, cards, and header chrome
--bg-surface-high        Hover states, active tabs, and secondary panels
--bg-surface-input        Embedded terminal consoles and code blocks
--border-subtle        Standard card and terminal borders
--border-light        Internal dividers
--border-focus        Focused interactive elements
--accent-blue        Primary buttons, active tabs, and breadcrumb highlights
--accent-blue-hover        Hover state for blue-accented elements
--accent-green        Passing builds, active status, and success states
--accent-pink        Editorial highlights, italic accents, and alerts
--accent-yellow        Warnings, caution indicators, and price accents
--accent-lavender        Special yarn attributes and tags
--accent-subtext        Secondary monospace comments
--text-primary        High-contrast headings and titles
--text-body        Standard body typography
--text-muted        Monospace comments and telemetry labels
--text-subtle        Dimmed labels and line numbers

2.3 Typography

Use three distinct typographic systems.

Editorial Headings

Use an editorial serif font:

* Playfair Display.
* Instrument Serif.
* Georgia as a fallback.

Apply serif typography to major headings and expressive brand statements.

Use italicized emphasis for selected words.

Example:

Life’s better when it’s a little knotty.

The phrase little knotty must use the coral-pink accent color #f38ba8 and italic styling.

UI and Body Text

Use:

* Inter.
* Plus Jakarta Sans.
* System sans-serif as a fallback.

Guidelines:

* Maintain clean, crisp, highly legible typography.
* Use approximately 14–16px for standard UI and body text.
* Establish a consistent hierarchy for headings, descriptions, labels, and supporting content.

Code and Telemetry

Use:

* JetBrains Mono.
* Fira Code.
* Monospace as a fallback.

Use monospace typography for:

* Repository paths.
* File names.
* Terminal tabs.
* Compiler steps.
* Status badges.
* Counters.
* Technical tags.
* Inventory telemetry.
* Build information.
* Product specification labels.
* Workflow identifiers.
* Code-like expressions.

2.4 Mac Terminal Chrome

Whenever a terminal-style window appears, follow a consistent macOS-inspired window pattern.

Window Controls

Include three circular controls:

Control        Color
Close        #f38ba8
Minimize        #f9e2af
Maximize        #a6e3a1

Terminal Header

Include:

1. Window controls.
2. A file or process tab.
3. Optional runtime or process metadata.
4. Right-aligned status information where appropriate.

Example tabs:

* showcase.yarn ● looping()
* filter_pipeline.sh
* strawberry_butter_bunny.obj

Example metadata:

* runtime: fiber_v2.4
* BUILD: PASSING

Create a reusable terminal chrome component instead of independently implementing this pattern in every section.

⸻

3. Application Architecture and Shared Components

Implement the application as three connected pages:

1. Home.
2. Shop Collection.
3. Product Detail.

All pages must share the same design system and reusable UI components.

3.1 Required Shared Components

Create individually exported components for at least the following:

* Header.tsx
* Footer.tsx
* TerminalChrome.tsx
* ProductCard.tsx
* StudioDaemon.tsx
* LoopCounter.tsx

Additional components should be created wherever they improve separation of concerns, consistency, or maintainability.

Recommended component responsibilities:

Component        Responsibility
Header.tsx        Brand identity, navigation, and status indicators
Footer.tsx        Build information, copyright, and social contact details
TerminalChrome.tsx        Reusable terminal-window frame and header
ProductCard.tsx        Reusable product card and its associated actions
StudioDaemon.tsx        Studio timer, live telemetry, and associated information
LoopCounter.tsx        Interactive crochet stitch state machine

Use shared components wherever possible instead of duplicating UI across the three pages.

3.2 Suggested Project Structure

Adapt the exact structure to the chosen framework while preserving these logical boundaries.

src/
├── app/ or pages/
│   ├── Home
│   ├── ShopCollection
│   └── ProductDetail
├── components/
│   ├── Header.tsx
│   ├── Footer.tsx
│   ├── TerminalChrome.tsx
│   ├── ProductCard.tsx
│   ├── StudioDaemon.tsx
│   └── LoopCounter.tsx
├── data/
│   └── products.ts
├── hooks/
│   └── timer and interaction hooks
├── types/
│   └── product types
├── styles/
│   └── global styles
└── assets/
    └── product images

This is a logical organization guideline, not a requirement to create unnecessary abstraction.

For Next.js, use the App Router conventions. For Vite, implement routing using an appropriate React routing solution or an equivalent clean routing structure.

⸻

4. Screen 1 — Home Page

Reference: 01_home.png

Implement the landing page with the following sections and interactions.

4.1 Persistent Top Navigation

Create a polished, persistent header.

Left Section

Display:

* Brand name: KnotFound.
* Serif typography for the brand.
* Badge: v2.4 CRAFT.

Center Section

Display navigation pills:

* Home — active state with a blue-tinted background.
* Shop // Collection.

The navigation must work and direct users to the appropriate pages.

Right Section

Display:

* Telemetry status pill: ● craft_status: active.
* Profile avatar icon.

The profile avatar may be an appropriate decorative or navigational element, depending on the supplied design, but it must not imply nonexistent account functionality.

4.2 Hero Section

Create an expressive hero section that introduces the brand.

Version Badge

Display:

✨ // version 2.4.0 — fresh off the hook

Use monospace typography.

Main Headline

Display:

Life’s better when it’s a little knotty.

Requirements:

* Use large editorial serif typography.
* Emphasize little knotty using italic coral-pink text.
* Preserve the intended visual hierarchy.

Supporting Description

Include a monospace and sans-serif description explaining the intersection of software development and crochet.

The copy should establish KnotFound as a handcrafted crochet brand with a developer-inspired personality.

Primary CTA

Display:

Explore the Collection 🧶 ↗

Link to the Shop Collection page.

Secondary CTA

Display:

☕ Meet the Maker

Use an outlined button style.

Code-Like Loop Condition

Display:

● while (creativity) { crochet(); }

Use monospace typography and an appropriate active-status accent.

4.3 Terminal Showcase

Create a single, visually prominent terminal frame.

Terminal Chrome

Use the reusable terminal component with:

* macOS window controls.
* File tab: showcase.yarn ● looping().
* Consistent border, background, and typography tokens.

Showcase Layout

Create an asymmetric product showcase containing:

Left — Hero Product

* Name: Bespoke Bunny No. 14
* Price: $42
* Material: milk cotton
* Badge: round 48

Top Middle — Floral Product

* Name: Everlasting Daisies
* Price: $38
* Badge: never wilts

Bottom Middle — Tote Bag

* Name: Palette Tote Bag
* Price: $54
* Badge: zero bugs, only loops

Right — Pattern Logger

Include a live-styled PATTERN LOGGER terminal widget.

Display row-by-row status information, including:

* READY_TO_ADOPT
* tension: relaxed

Bottom Right — Desk Companion

* Name: Boba Desk Pal
* Price: $24

Preserve the asymmetrical composition and visual relationships shown in the reference screenshot.

Use real product imagery where available. If assets are unavailable, use appropriately styled image placeholders without breaking the intended layout.

4.4 Live Studio Daemon and Loop Counter

Create a dedicated studio telemetry section.

Section Header

Display:

●●● // continuous_thread_timer.daemon

Include the active process indicator:

currently_looping PID #2048-HOOK

Live Counters

Display four counters in a four-column layout on sufficiently wide screens.

Counter        Initial Display
DAYS_ELAPSED        1,827
HOURS        14
MINUTES        44
SECONDS        Ticking in real time

The seconds counter must update dynamically.

Implement the ticker using useEffect and an appropriate timer mechanism. Clean up the timer when the component unmounts.

Preserve the specified initial values. Do not silently replace the specified display with unrelated numbers.

Studio Statistics Bar

Display:

* total_stitches_cast: 638,823
* tea_steeped: 984 mugs
* skeins_untangled: 312 balls

Use monospace typography for the labels and technical presentation.

Interactive State Machine

Create a stitch compiler interface identified as:

yarn_compiler.stitch

Display the following initial rounds:

Round        Initial State
RND 26        ✓ completed
RND 27        ✓ completed
RND 28        ● in progress
RND 29        queued

Use distinct visual treatments for completed, active, and queued states.

Step Next Stitch

Provide a button labeled:

Step Next Stitch

On click:

1. Mark the current in-progress round as completed.
2. Advance the next queued round to the in-progress state.
3. Preserve the completed states of earlier rounds.
4. Update the displayed state immediately.

The interaction must be stateful and repeatable rather than a one-time visual effect.

Auto-Loop Toggle

Provide:

Auto-Loop: ON

Implement this as a functional toggle.

When enabled, the state machine should automatically advance at a reasonable interval.

When disabled, automatic advancement must stop.

The user must still be able to advance the state machine manually using Step Next Stitch.

Clean up any timers when the component unmounts or the toggle is disabled.

4.5 Featured Catalog Teaser

Create a product grid containing four featured products:

1. Strawberry Butter Bunny
2. Perpetual Daisy Bouquet
3. Lavender Mist Bucket Hat
4. Boba Desk Companion

Each card must include:

* Product image.
* Category pill.
* Product title.
* Description.
* Stock status.
* Price.
* Enquire on Instagram ↗ CTA.

Use the reusable ProductCard.tsx component.

Where product data is shared with the Shop Collection page, source it from the centralized product dataset.

The CTA should open the appropriate Instagram contact or enquiry destination.

4.6 Footer

Display:

* Monospace commit hash: commit_hash: 7a8f3b
* Build status.
* Copyright information.
* DM handles.

Maintain the terminal-inspired visual language.

Use the same footer component across the application.

⸻

5. Screen 2 — Shop Collection

Reference: 02_shop_collection.png

Implement the product catalog page with interactive filtering, detailed product cards, and a terminal-inspired adoption workflow.

5.1 Page Header and Context

Display the title:

Handcrafted Drops & Ready-to-Ship Artifacts

Include the monospace inventory status:

● 5 IN STOCK // 3 MADE TO ORDER

Preserve the specified initial status display.

5.2 Terminal Filter Pipeline

Create a terminal-style filtering panel using the shared terminal chrome.

Terminal File

filter_pipeline.sh

Filter Categories

Display the following interactive filter pills with their specified item counts:

Filter        Count
ALL_OBJECTS        6
AMIGURUMI // BUDDIES        3
DESK_COMPANIONS        2
FLORAL_SYSTEMS        1
WEARABLES        1

Set ALL_OBJECTS (6) as the initial active filter.

Use a blue-tinted active state consistent with the design system.

Functional Filtering

Clicking a filter must dynamically update the displayed product grid.

Requirements:

* ALL_OBJECTS displays all six products.
* Other filters display products belonging to the corresponding category.
* The active filter state must be visually obvious.
* The displayed product count should reflect the active result set where appropriate.
* The interface must update without a full page reload.

Use centralized product data and useMemo where appropriate to derive the filtered product list.

The category counts above are the specified reference values and must be preserved. Because products may belong to multiple categories, category counts do not necessarily add up to the total number of unique products.

Filter Metadata

Display:

sort: by_recency() [DESC]

And:

stream: live

Use monospace styling.

The interface should visually communicate that the catalog is a live inventory stream, without implying real backend synchronization where none exists.

5.3 Product Catalog Grid

Display exactly six catalog products.

Product 1 — Strawberry Butter Bunny

* Price: $42.00
* Availability: Ready to ship

Product 2 — Perpetual Daisy Bouquet

* Price: $38.00
* Description: Ceramic pot included

Product 3 — Boba Desk Companion v1.0

* Price: $24.00
* Description: Desk buddy certified

Product 4 — Lavender Mist Bucket Hat

* Price: $46.00
* Description: 100% organic cotton

Product 5 — Palette Granny Square Tote

* Price: $54.00
* Availability: Made to order

Product 6 — Matcha Latte Desk Pal

* Price: $24.00
* Description: Plant-based fibers

Product Card Requirements

Each product card must include:

1. Product image.
2. Category badge.
3. Product name.
4. Product description.
5. Price.
6. Availability status.
7. Technical specification table.

The technical specification table must include:

* Yarn composition.
* Tension.
* Dimensions or gauge.
* Status.

Use consistent monospace styling for technical labels and a clear hierarchy for product descriptions and pricing.

Do not invent claims about product materials, dimensions, or construction that are not supported by available product data. Where exact values have not been provided, use clearly identifiable placeholder values in the data model until the actual specifications are available.

Product Actions

Provide an availability-appropriate CTA:

* Adopt on DM ↗
* Pre-order via DM ↗

Use the adoption action for ready-to-ship products and the pre-order action for made-to-order products, according to their availability.

These actions should direct users toward Instagram DM rather than attempting to process payments within the application.

5.4 Adoption Protocol Terminal

Create a dedicated terminal-inspired section using the filename:

how_to_adopt.md

Display three numbered workflow cards.

Step 01

step_01() inspect_artifact()

Represents reviewing the product and its specifications.

Step 02

step_02() dispatch_dm("@knotfound")

Represents initiating a direct message to the brand.

Step 03

step_03() compile_shipping()

Represents confirming order details and arranging shipping.

Use consistent terminal styling, monospace labels, and clear visual progression between steps.

⸻

6. Screen 3 — Product Detail

Reference: 03_product_detail.png

Implement a detailed product inspection page for:

strawberry_butter_bunny.obj

The page should feel like inspecting a handcrafted artifact inside a developer-oriented IDE.

6.1 Monospace Breadcrumb Chrome

Display the breadcrumb:

// return_to_collection(shop) / artifacts / amigurumi / strawberry_butter_bunny.obj

The return_to_collection(shop) portion must be clickable and navigate back to the Shop Collection page.

Use monospace typography throughout the breadcrumb.

Status Indicators

Display:

READY_TO_SHIP // 1 in studio

And:

BUILD: PASSING (0 ERRORS)

Use the green accent for passing status and appropriate contrasting styles for inventory information.

6.2 Artifact Inspection Viewport

Create a prominent product inspection area.

Viewport Chrome

Display:

artifact_preview_pane.tsx [READONLY]

Include the technical metadata:

* hook: 3.50mm alu
* tension: calibrated_firm

Use the shared terminal chrome pattern where appropriate.

Main Product Viewport

Display a large product image with technical HUD overlays.

Required overlays:

* [BUILD: PASSING]
* tension_inspector: active
* scale: 1.0x
* polycount: 4,890 loops

Use small monospace labels positioned around the image without obstructing the main product.

The polycount value is a crochet-themed visual metaphor and should be preserved exactly as specified.

Thumbnail Strip

Display four selectable thumbnails:

1. 01_front.raw
2. 02_hat_macro.raw
3. 03_overalls.raw
4. 04_desk_scale.raw

Each thumbnail must have its own image and visible label.

Functional Image Selection

Clicking a thumbnail must update the active product image in the main viewport.

Requirements:

* Highlight the currently selected thumbnail.
* Keep the active image synchronized with the selected thumbnail.
* Provide accessible names for thumbnail buttons.
* Use appropriate alternative text for the product images.

If the exact images are unavailable, create appropriately styled image placeholders that preserve the expected layout and interaction.

Verification Bar

Display:

Loop Precision Verified // commit: #4e82b1

6.3 Adoption and Specifications Console

Create the right-hand information column on desktop layouts.

Product Badges

Display:

* // AMIGURUMI
* DESK_COMPANION
* v2.1-craft

Product Title

Display:

Strawberry Butter Bunny

Use editorial serif typography.

Price Display

Display:

$42.00 USD / piece

Place the price in a prominent elevated slate container.

Product Description

Include copy describing:

* The hand-stitched construction.
* The product’s handcrafted character.
* Its floppy ears.
* Its role as a desk companion.

The copy should feel warm and appealing while preserving the developer-inspired tone of the brand.

Primary CTA

Display:

Adopt on Instagram DM ↗

Requirements:

* Full-width button.
* Catppuccin blue background.
* Appropriate hover and focus states.
* Clear external navigation behavior.
* Link to the brand’s Instagram DM destination.

Do not implement an in-app checkout or claim that an order has been placed.

6.4 Adoption Protocol Workflow

Create a section titled:

// ADOPTION_PROTOCOL_WORKFLOW

Display three cards describing:

1. Specification Verification — Review the artifact and confirm the selected product.
2. DM Confirmation — Contact the brand through Instagram and confirm the order details.
3. Physical Compilation — Confirm shipping details and prepare the physical product.

Maintain the technical, workflow-oriented naming convention while making the actual process understandable to customers.

6.5 Frequently Paired in Studio

Display three related product cards:

1. Boba Desk Companion
2. Perpetual Daisy Bouquet
3. Lavender Mist Bucket Hat

Each card must include a:

view_spec >

action link.

The link must navigate to the corresponding product detail page or relevant product information.

Use the shared product card system and consistent product data.

⸻

7. Functional Implementation Requirements

The visual design alone is not sufficient. Implement the following interactions as real, working functionality.

7.1 Live Timer

Implement the studio daemon timer using useEffect.

Requirements:

* Preserve the specified initial counter values.
* Update the seconds counter in real time.
* Use appropriate timer cleanup.
* Avoid unnecessary component re-renders where practical.
* Ensure the component behaves correctly when mounted and unmounted.

The displayed initial values are part of the design specification. The timer implementation must not silently discard them.

7.2 Stitch Compiler State Machine

Implement a real state machine for the crochet loop counter.

Initial states:

RND 26 — completed
RND 27 — completed
RND 28 — in progress
RND 29 — queued

Manual Advancement

The Step Next Stitch button must:

1. Complete the currently active round.
2. Advance the next queued round to in progress.
3. Preserve all completed rounds.
4. Update the UI immediately.

Automatic Advancement

The Auto-Loop: ON toggle must control automatic progression.

* When enabled, the state machine advances automatically at a reasonable interval.
* When disabled, automatic advancement stops.
* Manual advancement remains available regardless of the toggle state.
* Timers must be cleaned up correctly.

Keep state transitions deterministic and predictable.

7.3 Product Filtering

Implement dynamic catalog filtering.

* Display six products initially.
* Support all five specified filter options.
* Update the product grid when a filter is selected.
* Visually distinguish the active filter.
* Use the specified category counts.
* Avoid page reloads for filtering.

7.4 Product Image Inspector

Implement the thumbnail interaction on the product detail page.

* Four selectable thumbnails.
* One active main image.
* Synchronized active-thumbnail styling.
* Accessible keyboard-operable controls.

7.5 Navigation

Implement working navigation for:

* Home.
* Shop Collection.
* Product details.
* Return-to-collection breadcrumb.
* Related products.
* Featured product cards where product navigation is appropriate.

Use consistent routes and avoid broken links.

7.6 Instagram CTAs

The application is a product showcase and enquiry experience.

All adoption and enquiry CTAs should direct customers toward Instagram DM.

Use the brand handle:

@knotfound

Do not add a backend, payment processing, cart checkout, or account system unless explicitly requested separately.

The goal is to keep the storefront frontend-driven and avoid unnecessary backend infrastructure.

7.7 Image Handling

Use accessible image containers with:

* object-cover.
* Rounded borders such as rounded-xl or rounded-2xl.
* Consistent aspect ratios within equivalent product cards.
* Responsive sizing.
* Appropriate alternative text.

Use local product assets when available.

If local assets are unavailable, use styled placeholders that match the intended image dimensions, background palette, and layout.

Do not let missing image assets break the application or collapse the intended layout.

7.8 Product Data

Keep product information centralized in a typed data module.

The data model should support:

* Product identifier or slug.
* Product name.
* Price.
* Description.
* Category.
* Availability.
* Yarn composition.
* Tension.
* Dimensions or gauge.
* Product images.
* Image labels.
* Related products.
* Instagram CTA destination.

Use the same source of truth across the Home, Shop Collection, and Product Detail pages wherever products overlap.

Preserve all specified product names, prices, badges, initial inventory text, and other explicitly defined values.

Where details are unspecified, use clearly marked placeholders rather than presenting fabricated technical specifications as verified facts.

⸻

8. Responsive Design Requirements

The application must remain visually coherent across desktop, tablet, and mobile layouts.

8.1 Desktop

Preserve the intended multi-column compositions.

Examples:

* Asymmetric Home Page terminal showcase.
* Four-column studio counter layout.
* Multi-column product grids.
* Two-column product detail layout with the inspector and adoption console.

Maintain visual hierarchy, whitespace, and the terminal-inspired aesthetic.

8.2 Tablet

Adapt column counts and spacing to available width.

Prevent:

* Horizontal overflow.
* Overlapping terminal headers.
* Cramped product cards.
* Unreadable monospace labels.
* Clipped CTAs.

8.3 Mobile

Use responsive layouts that collapse to a single column when necessary.

Use responsive Tailwind patterns such as:

grid-cols-1
md:grid-cols-2
lg:grid-cols-3

Apply these according to the needs of each section rather than forcing every component into the same grid.

Ensure:

* Navigation remains usable.
* Product images retain suitable proportions.
* Buttons remain easy to tap.
* Terminal chrome adapts to narrow screens.
* Technical labels wrap or scroll appropriately.
* Product detail content follows a logical reading order.
* Timers and state machine controls remain usable.
* No accidental horizontal scrolling occurs.

⸻

9. Code Quality and Implementation Constraints

9.1 Clean Exports

Export reusable components individually.

Required component files:

Header.tsx
Footer.tsx
TerminalChrome.tsx
ProductCard.tsx
StudioDaemon.tsx
LoopCounter.tsx

Follow the chosen framework’s conventions for page components, layouts, routes, and shared UI.

9.2 Maintainability

Use:

* Strict TypeScript.
* Explicit interfaces and types.
* Reusable UI components.
* Centralized product data.
* Centralized theme tokens.
* Small, focused components.
* Clear state ownership.
* Consistent naming conventions.

Avoid:

* Duplicated product data across pages.
* Unnecessary dependencies.
* Hardcoded repeated styling.
* Uncontrolled side effects.
* Unnecessary abstractions.
* Dead code.
* Unused imports.
* Broken image references.
* Nonfunctional buttons.
* Placeholder navigation presented as real functionality.

9.3 Dependency Discipline

Use the required framework and styling dependencies.

Add only the additional libraries needed to implement the specified functionality.

Do not introduce a backend, database, authentication provider, or external service unnecessarily.

9.4 Performance

Implement the application with reasonable frontend performance practices.

* Use optimized image handling appropriate to the chosen framework.
* Avoid unnecessary re-renders.
* Use useMemo where it meaningfully improves derived-data computation.
* Clean up intervals and other side effects.
* Keep components modular.
* Avoid loading unnecessary dependencies.

⸻

10. Implementation Sequence

Follow this order to maintain consistency and minimize rework.

Phase 1 — Repository Inspection and Foundation

1. Inspect the existing repository and identify the framework, package manager, dependencies, and current application structure.
2. If an existing project is present, adapt the implementation to it rather than replacing working infrastructure unnecessarily.
3. If starting from scratch, scaffold the selected framework with strict TypeScript.
4. Configure Tailwind CSS.
5. Define the complete color palette and typography tokens.
6. Establish the global layout and responsive foundations.
7. Configure the required fonts and icons.

Deliverable: A working application foundation with the complete design system.

Phase 2 — Shared Components

Implement:

1. Header.tsx
2. Footer.tsx
3. TerminalChrome.tsx
4. ProductCard.tsx
5. StudioDaemon.tsx
6. LoopCounter.tsx

Create the typed product data model and centralized product dataset.

Deliverable: Reusable components and data structures that can support all three pages.

Phase 3 — Home Page

Implement the complete Home Page:

1. Persistent navigation.
2. Hero section.
3. Single terminal showcase.
4. Featured products.
5. Live studio daemon.
6. Interactive stitch compiler.
7. Catalog teaser.
8. Footer.

Verify the live timer and state machine interactions before moving forward.

Deliverable: A responsive Home Page with working interactions.

Phase 4 — Shop Collection

Implement:

1. Collection page heading.
2. Inventory status.
3. Terminal filter pipeline.
4. Five interactive filters.
5. Six product cards.
6. Product specification tables.
7. Adoption and pre-order CTAs.
8. Adoption protocol terminal.

Verify that every filter returns the expected products and that product actions behave correctly.

Deliverable: A working catalog page with dynamic filtering.

Phase 5 — Product Detail

Implement:

1. IDE-style breadcrumb.
2. Build and inventory indicators.
3. Artifact inspection viewport.
4. Technical HUD overlays.
5. Four-image thumbnail selector.
6. Product information and price.
7. Adoption CTA.
8. Adoption protocol workflow.
9. Related products.

Verify thumbnail selection, navigation, and related-product links.

Deliverable: A functional product detail experience.

Phase 6 — Responsive Refinement

Test all three pages at desktop, tablet, and mobile widths.

Refine:

* Grid behavior.
* Typography scaling.
* Terminal header layouts.
* Image sizing.
* Button dimensions.
* Navigation behavior.
* Product detail column stacking.
* Overflow handling.

Deliverable: A responsive application consistent with the reference screenshots.

Phase 7 — Validation and Cleanup

Run the available project checks.

At minimum:

1. Run the production build.
2. Run TypeScript checks.
3. Run linting if configured.
4. Resolve compilation errors.
5. Resolve avoidable console errors.
6. Verify internal navigation.
7. Verify product filtering.
8. Verify the live timer.
9. Verify manual and automatic stitch advancement.
10. Verify product thumbnail selection.
11. Verify Instagram CTA destinations.
12. Confirm that no specified sections or products have been omitted.

Do not report checks as passing unless they have actually been executed successfully.

Deliverable: Complete, compilable frontend code with validated interactions and a clean project structure.

⸻

11. Definition of Done

The implementation is complete only when all of the following conditions are satisfied.

Visual Fidelity

* All three provided reference screenshots have been used.
* The Catppuccin-inspired color tokens match the specified hex values.
* Serif, sans-serif, and monospace typography are used for their intended roles.
* Terminal chrome is consistent across applicable sections.
* All specified headings, labels, badges, prices, and status indicators are present.
* The asymmetric Home Page showcase preserves its intended composition.
* The Shop Collection and Product Detail pages preserve their intended hierarchy.
* Missing images do not break the layout.

Home Page

* Persistent navigation works.
* Hero copy and CTAs are present.
* All five terminal showcase elements are represented.
* The studio timer displays the specified initial values and updates the seconds counter.
* All three studio statistics are displayed.
* The stitch compiler begins with the specified four round states.
* Step Next Stitch works.
* Auto-Loop: ON is a functional toggle.
* The four featured catalog products are displayed.
* Footer metadata is present.

Shop Collection

* The page heading and inventory status are present.
* All five filter options and their specified counts are displayed.
* ALL_OBJECTS (6) is initially active.
* Filtering works without a page reload.
* All six specified products are displayed in the complete catalog.
* Product specification tables are present.
* Adoption and pre-order actions are available as appropriate.
* The three-step adoption protocol is displayed.

Product Detail

* The specified breadcrumb is displayed.
* The breadcrumb returns to the Shop Collection page.
* Inventory and build status indicators are present.
* The artifact inspection viewport contains all four HUD overlays.
* All four thumbnails are available.
* Selecting a thumbnail updates the main image.
* The verification bar is present.
* Product badges, title, price, and description are displayed.
* The Instagram adoption CTA works.
* The three-step adoption workflow is present.
* All three related products are displayed.
* Related-product actions navigate appropriately.

Engineering Quality

* TypeScript strict mode is enabled.
* Required shared components are individually exported.
* Product data is centralized and typed.
* Styling tokens are centralized.
* Responsive layouts work across supported screen sizes.
* Interactive controls are keyboard accessible.
* Timers and side effects are cleaned up.
* No unnecessary backend or checkout infrastructure has been added.
* Production build and available validation checks have been executed.
* No specified functionality has been silently omitted.

⸻

12. Final Instructions to the Coding Agent

Proceed to scaffold and implement the complete, compilable application.

Use the three supplied PNG screenshots as visual references and follow this document as the implementation specification.

Do not stop after producing a plan, mockup, component skeleton, or static UI. Implement the actual pages, interactions, routing, reusable components, styling, and data structures.

Preserve all specified content and behavior.

If a detail is unspecified, make a reasonable implementation decision consistent with the design system. Do not remove an explicit requirement to simplify the implementation.

If product images are unavailable, use styled placeholders rather than blocking implementation.

If an existing repository is present, inspect it first and preserve compatible infrastructure. If starting from scratch, establish the foundation before building the pages.

Upon completion, provide a concise summary of:

1. What was implemented.
2. The main files and components created.
3. The interactions implemented.
4. Any missing image assets or external configuration that still require attention.
5. The build and validation checks that were actually executed, including any failures that remain.

The final result must be a complete, responsive, interactive frontend for KnotFound, faithful to the supplied references and ready for further product and branding refinement. ^EuBUxsic

Prompt ^rG7mppVR

## Embedded Files
fb3f6845eeea41a845ada94cbb7f4da102c27137: [[Pasted Image 20261017132336_826.png]]

eee86e9a6028272e8b86785be31489c9bd1b83b5: [[Pasted Image 20261017132349_209.png]]

3ab0cd5887cb81ef4df659c0d9f17fb24ad9d8c2: [[Pasted Image 20261017132506_455.png]]

%%
## Drawing
```compressed-json
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
```
%%