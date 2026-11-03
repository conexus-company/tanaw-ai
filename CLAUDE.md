# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

@AGENTS.md

## Project status

Tanaw is a voice-activated, multimodal AI assistant (wake word → speech → AI orchestrator → tools such as camera/vision, OCR, LinkGuard URL scanning → spoken + contextual UI responses). Android is the primary platform. `SPEC.md` is the full product/technical spec, including functional requirements (F-xxx IDs), the initial tool registry, and the phased roadmap. Read the relevant section before implementing a feature.

The code is currently still the unmodified `create-expo-app` starter (Home/Explore tabs). None of the spec's features exist yet.

## Spec vs. actual stack

`SPEC.md` was written for React Native CLI with hand-written Kotlin in `android/`. The repo is actually **Expo SDK 58 with Continuous Native Generation**, so the AGENTS.md rules take precedence:
- Do not hand-edit `android/`/`ios/`. Put the spec's native Kotlin pieces (wake-word foreground service, Porcupine, audio session, Share Intent handling) in Expo native modules and config plugins configured through `app.json`.
- Any native dependency (Porcupine, VisionCamera, etc.) needs a development build. It won't run in Expo Go.
- Use the spec's §15 repository layout (`src/features/`, `src/services/`, `src/agent/{orchestrator,tools,registry,schemas,policies}`, `backend/`) only as a guideline. Routes still live only in `src/app/`.

Architectural constraints from the spec that apply to all code:
- AI providers (Gemini by default) must sit behind provider abstractions. Provider API keys never go in client code; calls go through the backend.
- The agent is tool-based: only registered tools may run, their arguments are validated against schemas, permission checks and confirmation for sensitive actions are enforced, duplicate execution is prevented, and external/webpage content is treated as untrusted (prompt-injection risk).
- LinkGuard URL inspection must be SSRF-safe (block private IPs/localhost, guard against DNS rebinding and redirect loops) and return `Unknown` when analysis is incomplete.

## Commands

The project uses Bun (`bun.lock`), so substitute `bunx` for `npx`:

```bash
bun install
bunx expo start            # dev server (bun run android / ios / web)
bunx expo lint             # ESLint (config is generated on first run)
bunx tsc --noEmit          # typecheck
bunx expo install <pkg>    # add dependencies
```

There is no test runner configured yet. The spec calls for Jest + React Native Testing Library (see the Expo "Unit Testing with Jest" guide when setting it up).

## Code conventions in the starter

- The path aliases `@/*` → `src/*` and `@/assets/*` → `assets/*` are defined in `tsconfig.json`.
- Platform-specific files use the `.web.tsx` suffix (e.g. `app-tabs.tsx` uses `expo-router/native-tabs` on native, while `app-tabs.web.tsx` uses `expo-router/ui` headless tabs).
- Theming: `src/constants/theme.ts` defines `Colors` (light/dark), `Spacing`, and `Fonts`. `useTheme()` returns the active palette, and `ThemedText`/`ThemedView` take theme keys.
- `app.json` enables `typedRoutes` and the React Compiler (`reactCompiler: true`), so avoid manual `useMemo`/`useCallback` unless profiling shows they're needed.
