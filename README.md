# 🎮 Zentry Clone App

A pixel-perfect, animation-heavy clone of the Zentry gaming landing page, built with **React 19**, **Tailwind CSS 4**, and **GSAP 3**. The project recreates the award-winning Zentry UI in full — interactive video transitions in the hero, scroll-triggered clip-path animations, an expanding image reveal, a 3D tilt Bento grid, a mouse-tracked parallax image in the story section, a GSAP-animated navigation bar, and a responsive mobile hamburger menu — all composed from reusable React components.

---

## What This Project Does

The app renders a single-page layout with seven composable sections: a sticky navigation bar, a full-screen video hero, an about section with a scroll-pinned expanding image, a black bento-card features grid, a story parallax section, a call-to-action join section, and a minimal footer. Every visual effect is built on GSAP's `ScrollTrigger` plugin and React's `useGSAP` hook, meaning animations are tied to scroll position and cleaned up automatically on component unmount. The project ships with five custom `.woff2` typefaces, 10 `.webp` images, and 9 looping `.mp4` video files — all served from the `/public` directory via Vite.

---

## Features

**1. Video Hero with Click-to-Transition** — The hero section (`Hero.jsx`) manages three simultaneous `<video>` elements: a full-screen background video (`backgroundVideoRef`), a miniature floating preview video (`miniVideoRef`), and a transitioning expansion video (`nextVideoRef`). Hovering the center of the screen reveals the mini-video preview. Clicking it sets `hasClicked` to `true`, increments `currentIndex`, and triggers a GSAP animation that expands the next video from a 256×256px center thumbnail to 100% width and height with a `power1.inOut` ease — seamlessly replacing the background. A `loadedVideos` counter gates the loading screen: when at least two videos have fired `onLoadedData`, the spinner disappears.

**2. Scroll-Driven Clip-Path Transform on the Hero** — A `useGSAP` hook sets the `#video-frame` element to an irregular polygon clip-path and `borderRadius` on mount, then uses `ScrollTrigger` to reverse both back to a full rectangle as the user scrolls past the hero — creating the signature Zentry "peeling" effect.

**3. Loading Spinner** — While videos load, a CSS-only three-body orbital spinner (styled via inline `<style>` with `@keyframes spin78236`, `wobble1`, and `wobble2`) is rendered in an absolutely-positioned overlay. It disappears as soon as `loadedVideos >= 2`.

**4. GSAP-Animated Navigation Bar** — `NavBar.jsx` uses three separate `useGSAP` hooks: one that hides/shows the navbar by animating `y: -120` on scroll direction changes via `ScrollTrigger`; one that transitions the nav background from transparent to `#000` once the page scrolls past 10px; and one that animates the mobile dropdown `height` from 0 to auto (open) or 0 (close) using `gsap.fromTo`. Desktop nav links have a CSS underline sweep on hover using a `scale-x` transition.

**5. Scroll-Pinned Expanding Image in About** — `About.jsx` pins the `#clip` container to the top of the viewport for 800px of scroll progress. During that pin, GSAP animates a `maskRef` div from a centered 450×500px rounded rectangle to `100vw × 100vh` with `borderRadius: 0` — revealing a full-bleed `about.webp` image beneath the text.

**6. Animated Section Titles** — `AnimatedTitle.jsx` is a shared component that accepts a `title` string with embedded `<br>` tags and bold HTML. It splits the string by line and word, renders each word as an `.animated-word` `<span>`, and drives a GSAP stagger animation — `rotateY: 45 → 0`, `rotateX: -25 → 0`, `x: 150 → 0`, `y: 150 → 0`, `opacity: 0 → 1` — triggered when the container enters the viewport at 80% scroll depth.

**7. Bento Card Grid with 3D Tilt** — `Features.jsx` renders five feature cards in a responsive CSS grid. Each card is wrapped in `BentoTilt`, which listens to `mousemove` events, calculates normalized cursor position within the card bounds, and applies a `perspective(1000px) rotateX() rotateY()` transform in real time for a physics-like tilt effect. `BentoCard` renders a looping muted `<video>`, an HTML title (with embedded `<b>` tags), and a description — all absolutely and relatively positioned for layering.

**8. Mouse-Tracked Parallax Image in Story** — `Story.jsx` displays an `entrance.webp` image clipped to an irregular polygon using `clipPath: "polygon(10% 0, 100% 20%, 100% 80%, 0% 105%)"`. The image is slightly scaled up and rotated by default (`scale-130 -rotate-5`). On `mousemove`, the component calculates cursor position relative to the image bounds and applies a real-time `perspective(1000px) rotateX() rotateY()` transform — resetting smoothly on `mouseleave`.

**9. "Join Zentry" CTA Section** — `StoryExtend.jsx` renders a dark slab with a large headline, a contact button, and two decorative character images (`swordman.webp` and `swordman-partial.webp`) — the second using a `clipPath` polygon to create a layered partial reveal effect. Two contact illustration images (`contact-1.webp` and `contact-2.webp`) are also polygon-clipped and positioned absolutely at the left edge.

**10. Skew-Animated Button** — `Button.jsx` is a reusable pill-shaped button that uses two stacked `<div>` elements with `skew-y-12` and `translate-y-[164%]` as starting states. On hover, a `group-hover:` CSS class translates one up and the other into view — creating a skewed slide-in text swap. Accepts `leftIcon`, `rightIcon`, `title`, and `containerClass` props.

**11. Responsive Mobile Menu** — On viewports below `md` breakpoint, the desktop nav links are hidden and a hamburger button appears. The three `<span>` lines animate into an X (rotate + translate) when `menuOpen` is true, controlled by Tailwind conditional classes. The mobile dropdown is GSAP-animated between `height: 0` and `height: auto`.

**12. Custom Typeface System** — Five `.woff2` fonts (`zentry-regular`, `circularweb-book`, `general`, `robert-medium`, `robert-regular`) are declared via `@font-face` in `index.css` and registered as Tailwind `@theme` font tokens (`--font-zentry`, `--font-circular`, etc.), making them available as utility classes like `font-zentry`.

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| React | 19.2.4 | Component architecture, JSX rendering, `useState`, `useRef`, `useEffect` |
| Tailwind CSS | 4.2.2 | All utility-class styling — layout, spacing, colors, responsive prefixes, dark mode |
| GSAP | 3.14.2 | All animations — scroll triggers, clip-path morphing, navbar transitions, title entrances |
| @gsap/react | 2.1.2 | `useGSAP` hook for GSAP integration with React lifecycle and automatic cleanup |
| react-icons | 5.6.0 | `TiLocationArrow`, `MdNearMe`, `FaDiscord`, `FaGithub`, `FaInstagram`, etc. |
| clsx | 2.1.1 | Conditional className composition in `Button.jsx` |
| Vite | 8.0.4 | Build tool and development server |
| ESLint | 9.39.4 | Linting with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh` |

---

## Project Structure

```
Zentry_Clone_App/
├── index.html                    # Vite entry point — single div#root mount target
├── package.json                  # Dependencies: react 19, tailwindcss 4, gsap, @gsap/react, react-icons, clsx, vite 8
├── package-lock.json             # Exact dependency lockfile
├── vite.config.js                # Vite configuration — React plugin
├── eslint.config.js              # ESLint flat config — react-hooks and react-refresh rules
├── .gitignore                    # node_modules, dist excluded
├── public/
│   ├── audio/
│   │   └── loop.mp3              # Background ambient audio asset
│   ├── fonts/
│   │   ├── circularweb-book.woff2
│   │   ├── general.woff2
│   │   ├── robert-medium.woff2
│   │   ├── robert-regular.woff2
│   │   └── zentry-regular.woff2  # Custom display font for hero headlines
│   ├── img/
│   │   ├── about.webp            # Full-bleed scroll-reveal background
│   │   ├── contact-1.webp        # Clip-path illustration (StoryExtend left)
│   │   ├── contact-2.webp        # Clip-path illustration (StoryExtend left lower)
│   │   ├── entrance.webp         # Mouse-parallax image in Story section
│   │   ├── gallery-1.webp … gallery-5.webp  # Decorative gallery assets
│   │   ├── logo.png              # Navbar brand mark
│   │   ├── play.svg              # Play icon
│   │   ├── stones.webp           # Atmospheric texture
│   │   ├── swordman.webp         # CTA section character (full)
│   │   └── swordman-partial.webp # CTA section character (partial clip-path layer)
│   └── videos/
│       ├── hero-1.mp4 … hero-4.mp4       # Hero background + transition videos
│       └── feature-1.mp4 … feature-5.mp4 # Bento card background videos
└── src/
    ├── main.jsx                  # React DOM root render — mounts <App /> into #root
    ├── App.jsx                   # Root component — composes all sections in order
    ├── App.css                   # Root-level CSS resets
    ├── index.css                 # Tailwind @import, @theme font tokens, @font-face declarations
    └── components/
        ├── NavBar.jsx            # Sticky header with scroll-hide, transparent-to-black background, GSAP mobile menu
        ├── Hero.jsx              # Full-screen video hero — mini preview, click-to-expand transition, clip-path scroll morph
        ├── About.jsx             # Scroll-pinned expanding image reveal with AnimatedTitle
        ├── Features.jsx          # Bento grid — six cards using BentoTilt + BentoCard
        ├── Story.jsx             # Mouse-parallax entrance image with clip-path polygon
        ├── StoryExtend.jsx       # "Join Zentry" CTA with clip-path character illustrations
        ├── Footer.jsx            # Minimal footer with social icons from react-icons
        ├── AnimatedTitle.jsx     # Reusable scroll-triggered word-by-word 3D entrance animation
        ├── BentoCard.jsx         # Video background card — title and description with HTML rendering
        ├── BentoTilt.jsx         # Mouse-position 3D tilt wrapper for any child component
        └── Button.jsx            # Pill button with skew-slide text-swap hover animation
```

---

## How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/tripathipawan/Zentry_Clone_App.git
   ```

2. Move into the project directory
   ```bash
   cd Zentry_Clone_App
   ```

3. Install dependencies
   ```bash
   npm install
   ```

4. Start the development server
   ```bash
   npm run dev
   ```

5. Open `http://localhost:5173` in your browser

6. To build for production
   ```bash
   npm run build
   ```
   Output goes into `dist/`, ready for Vercel, Netlify, or GitHub Pages.

---

## Repository

[https://github.com/tripathipawan/Zentry_Clone_App](https://github.com/tripathipawan/Zentry_Clone_App)
