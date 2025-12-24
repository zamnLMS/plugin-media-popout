# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a BigBlueButton plugin that adds popout buttons to webcams and screenshare, allowing video streams to open in separate browser windows. Built with React, TypeScript, and the BigBlueButton HTML Plugin SDK.

## Commands

- `npm install` - Install dependencies
- `npm start` - Start development server (runs on port 4701)
- `npm run build-bundle` - Build for production (outputs to `dist/MediaPopoutPlugin.js`)
- `npm run lint` - Run ESLint on src directory
- `npm run lint:fix` - Auto-fix linting issues

## Architecture

### Entry Point
- `src/index.tsx` - Renders the main plugin component, extracts `uuid` and `pluginName` from script attributes

### Core Plugin Logic
- `src/popout/component.tsx` - Main plugin component that:
  - Initializes the BBB Plugin SDK with `BbbPluginSdk.initialize(uuid)`
  - Subscribes to video streams, screenshare, and bot status via GraphQL
  - Registers helper buttons using `pluginApi.setUserCameraHelperItems()` and `pluginApi.setScreenshareHelperItems()`
  - Manages state of popped-out windows

### Services
- `src/popout/service.ts` - `togglePopout()` function that clones video MediaStream and opens in a new window
- `src/queries.ts` - GraphQL subscription queries for video streams, screenshare, and bot detection

### Types
- `src/popout/types.ts` - TypeScript interfaces for props and GraphQL response types

### Internationalization
- `locales/*.json` - Translation files (en, pt-BR)
- Uses `react-intl` with dynamic locale loading via `require.context('@locales')`

## Key SDK Patterns

The plugin uses `bigbluebutton-html-plugin-sdk`:
- `BbbPluginSdk.getPluginApi(uuid)` to get the plugin API
- `pluginApi.useCustomSubscription<T>()` for GraphQL subscriptions
- `pluginApi.useUiData()` for UI state like current locale
- `UserCameraHelperButton` and `ScreenshareHelperButton` for adding UI elements

## Webpack Configuration

- Output: UMD library named `MediaPopoutPlugin`
- Dev server: port 4701, no hot reload
- Path alias: `@locales` → `./locales`

## Resources

For more information on developing and improving BBB plugins, see the official documentation:
https://docs.bigbluebutton.org/plugins/
