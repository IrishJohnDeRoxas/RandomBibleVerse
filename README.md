# Random Bible Verse Vue Component
A Vue 3 + TypeScript + Shadcn Vue component for fetching and displaying random Bible verses.

## Features
- Random verse generator – Fetches a Bible verse on load and button click
- Modern UI – Built with Shadcn Vue components
- Smooth transitions – Fade-scale animation when verses change
- Skeleton loader – Displays placeholders while fetching data
- TypeScript support – Strong typing for API responses
- Error handling – Handles API errors gracefully

## Prerequisites
- [Node.js](https://nodejs.org/) v18+
- [Vue 3](https://vuejs.org/) project initialized
- [TypeScript](https://www.typescriptlang.org/) configured
- [Axios](https://axios-http.com/) installed
- [Shadcn Vue](https://shadcn-vue.com/) installed

## Installation

1.  Install Required Packages.

  ```
  npm install axios shadcn-vue
  ```

2.  Setup Shadcn Vue (if not already).

  ```
  npx shadcn-vue init
  ```

3.  Generate the UI components:

  ```
  npx shadcn-vue add card button skeleton
  ```

This will create:
  ```
  @/components/ui/card
  @/components/ui/button
  @/components/ui/skeleton
  ```

## File Structure

Place the component file inside your project:

  ```
  src/
   └── components/
        └── RandomBibleVerse.vue

  ```

## Usage

Import and use the component in your page:
  ```
  <template>
    <RandomBibleVerse />
  </template>
  
  <script setup lang="ts">
  import RandomVerse from '@/components/RandomBibleVerse.vue'
  </script>
  ```

## License

[MIT](https://choosealicense.com/licenses/mit/) – Free for personal and commercial use.
