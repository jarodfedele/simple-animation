# Oscillating Circle Test

This repository is a minimal reproduction of an intermittent animation problem. A solid circle
moves continuously between the left and right edges of the viewport, but it can sometimes appear
to hold briefly and then catch up despite using a compositor-friendly CSS transform.

The page contains one animated element and one CSS animation. There is no authored client-side
JavaScript, animation library, timer, event listener, image, font, media, API request, analytics
script, or diagnostic logger.

## Run the production build

Use the production build when judging motion. Development mode adds framework tooling and work that
is not part of this reproduction.

```bash
npm install
npm run build
npm start
```

Then open [http://localhost:3000](http://localhost:3000).

For ordinary development only:

```bash
npm run dev
```

The project can also be imported into Vercel without configuration or environment variables.

## Complete reproduction code

`app/page.tsx`:

```tsx
export default function HomePage() {
  return (
    <main className="relative h-full w-full overflow-hidden bg-white">
      <span aria-hidden="true" className="circle" />
    </main>
  );
}
```

`app/globals.css`:

```css
@import "tailwindcss";

:root {
  --circle-size: min(20vh, 80vw);
  color-scheme: light;
  background: #fff;
}

@supports (height: 100dvh) {
  :root {
    --circle-size: min(20dvh, 80vw);
  }
}

* {
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  overflow: hidden;
  background: #fff;
}

.circle {
  position: absolute;
  top: 50%;
  left: 0;
  display: block;
  width: var(--circle-size);
  height: var(--circle-size);
  border-radius: 50%;
  background: #1266d3;
  animation: circle-motion 2s cubic-bezier(0.3628, 0.10044, 0.6372, 0.89956)
    infinite alternate;
  will-change: transform;
}

@keyframes circle-motion {
  from {
    transform: translate3d(0, -50%, 0);
  }

  to {
    transform: translate3d(calc(100vw - var(--circle-size)), -50%, 0);
  }
}
```

## Observation protocol

1. Run the production build with browser hardware acceleration enabled.
2. Keep the computer connected to power and close developer tools.
3. Use a normal browser window on the display being tested.
4. Watch the circle continuously for at least two minutes.
5. Note whether it ever visibly pauses, repeats a position, or jumps forward to catch up.

Browser animation timing APIs may continue advancing normally even when a physical display repeats
the previous pixels. Visual observation or a high-frame-rate camera recording is therefore the
acceptance test for this reproduction.

## Tester response template

```text
Jitter observed: Yes / No
Approximate time before it appeared:
Browser and version:
Operating system:
Computer/GPU:
Display model and refresh rate:
Hardware acceleration enabled: Yes / No
Connected to power: Yes / No
Test duration:
Development or production build:
Other relevant details:
```

## Framework versions

- Next.js 16.2.4
- React 19.2.4
- Tailwind CSS 4.2.4
- TypeScript 5.9.3

These versions are intentionally pinned for reproduction parity. `npm audit` currently reports
known advisories for Next.js 16.2.4, so this exact dependency set should not be reused as a general
production starter.
