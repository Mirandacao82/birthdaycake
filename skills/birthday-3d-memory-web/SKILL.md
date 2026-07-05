---
name: birthday-3d-memory-web
description: Create, modify, deploy, or document a cinematic interactive birthday web experience using React, TypeScript, Three.js, React Three Fiber, photo assets, candle blowing interactions, 3D cake scenes, draggable Polaroid memory walls, Vercel public URLs, and QR-code sharing. Use when the user asks to build a Vibe Coding birthday webpage, online birthday surprise, candle blowing page, memory photo wall, deploy/share the page, or turn the birthday project workflow into reusable steps.
---

# Birthday 3D Memory Web

Use this skill to build or update a shareable interactive birthday webpage centered on a 3D cake, candle blowing, and a revealable memory photo wall.

## Core workflow

1. **Confirm project location**
   - Put all generated files under the current workspace folder.
   - If no workspace folder exists, use Desktop.
   - Create a new project folder unless the user explicitly asks to modify an existing one.

2. **Use a short design gate for creative changes**
   - For new creative builds or behavior changes, briefly confirm the interaction and visual direction before coding.
   - Keep the design concise: scene, state flow, assets, interactions, deployment target.

3. **Scaffold the app**
   - Prefer Vite + React 19 + TypeScript.
   - Use React Three Fiber for the 3D scene and `@react-three/drei` for helpers such as `Float`, `Text`, `Image`, `Environment`, `Stars`, and `Sparkles`.
   - Use `framer-motion` for 2D UI transitions and lightbox overlays.
   - Pin React 19 dependencies explicitly when npm peer resolution is unstable.

4. **Implement the birthday state machine**
   - Use a typed scene state: `WAITING | BLOWING | EXTINGUISHED | REVEAL`.
   - `WAITING`: candles lit and gently flickering.
   - `BLOWING`: press-and-hold interaction, stronger flame shake, subtle camera push-in, lighting shift.
   - `EXTINGUISHED`: flames disappear, smoke particles rise.
   - `REVEAL`: smoke clears and the photo memory wall appears.

5. **Use reusable components**
   - `BirthdayScene`: Canvas, camera, lights, background, orchestration.
   - `Cake`: layered cake, cream, sprinkles, candle placement, optional drag rotation.
   - `Candle`: candle body, wick, `Flame`, `SmokeParticles`.
   - `Flame`: animated emissive flame and point light.
   - `SmokeParticles`: translucent rising smoke loops.
   - `PhotoMemoryWall`: 3D Polaroid cards, drag rotation, click-to-aim, double-click-to-open.
   - `PhotoLightbox`: enlarged photo overlay with close controls.
   - `BlowButton`: press-and-hold CTA and reset action.

6. **Handle photo assets safely**
   - Copy browser-friendly images from the provided asset folder into `public/memories`.
   - Prefer JPG/JPEG/PNG/WebP.
   - Skip HEIC unless conversion tooling is available and tested.
   - Generate a small `memories.ts` list with photo URLs and short birthday messages.
   - For public deployments, create compressed thumbnails such as `public/memories/thumbs/*.jpg` and use thumbnails in the 3D wall.
   - Keep the original image URL separately, for example `fullSrc`, and load it only in the enlarged lightbox.

7. **Add high-value interactions**
   - Make the cake draggable in 3D with pointer events and a little inertia.
   - Make the photo wall draggable horizontally with momentum.
   - Click a Polaroid card to rotate/aim the wall toward that photo.
   - Double-click or quick double-tap a card to open a large preview.
   - Support ESC, backdrop click, and close button to dismiss the preview.

8. **Validate locally**
   - Run `npm install --legacy-peer-deps` when npm peer dependency resolution fails.
   - If npm global cache has permission issues, use a project cache: `npm_config_cache=./.npm-cache <command>`.
   - Run `npm run build` before handoff.
   - Clean temporary `.npm-cache` after successful commands.

9. **Protect the core scene from async hangs**
   - Do not wrap the whole scene in one `Suspense fallback={null}` when it contains async helpers or image textures.
   - Keep core objects such as cake, candles, lights, stage, stars, and CTA-driven animation outside async boundaries.
   - Put `Environment` in its own `Suspense` boundary because it can load fine locally but suspend online.
   - Mount `PhotoMemoryWall` only after `REVEAL`, inside a separate `Suspense`, so photo textures cannot blank the initial cake scene.
   - If Vercel shows text and stars but no cake, suspect an async R3F helper suspending later siblings; isolate or remove that helper first.
   - If Vercel shows cake locally but not online after reveal, suspect large photo textures; switch the wall to thumbnails and keep originals for lightbox only.

10. **Deploy for public sharing**
   - Explain that `npm run dev` is local-only unless exposed by a tunnel.
   - Prefer Vercel for public sharing: `npx vercel login`, then `npx vercel --prod --yes`.
   - If global npm cache fails, use `npm_config_cache=./.npm-cache npx vercel ...`.
   - Return both the aliased production URL and fallback deployment URL when available.

11. **Generate a QR code when requested**
    - After obtaining the public URL, generate a QR image in the project folder:
      `npm_config_cache=./.npm-cache npx qrcode "<public-url>" -o birthday-memory-qr.png`
    - Verify the PNG exists and report its absolute path.

## Recommended copy and visual direction

- Use the main phrase: `吹灭蜡烛后，回忆被点亮`.
- Keep the style dreamy, warm, cinematic, and polished rather than overly cartoonish.
- Use a large CTA: `按住吹气`.
- During reveal, label the memory interaction clearly, for example: `拖拽旋转相册 · 双击放大照片`.

## Deployment caveats

- Vercel login may require browser/device-code authorization by the user.
- Deployment cannot complete with an invalid token; ask the user to log in and then retry.
- Large local photo assets can make uploads slow. Continue patiently and report upload/deploy status.
- A public URL means anyone with the link can view the page; ask before adding private or sensitive photos.

## Local usage handoff

Give the user the simplest local commands:

```bash
cd <project-folder>
npm install --legacy-peer-deps
npm run dev
```

Then tell them to open `http://localhost:5173/`.
