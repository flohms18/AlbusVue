<script setup>
import ChatComponent from './ChatComponent.vue';
</script>

<template>
  <ChatComponent />
</template>

<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg: #1a1a1a;
  --surface: #2b2b2b;
  --border: #383838;
  --text: #e8e6e3;
  --text-muted: #8c8c8c;
  --violet: #5c6bff;
}

html, body { height: 100%; background: var(--bg); }

@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Light.ttf') format('truetype');
  font-weight: 300;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Regular.ttf') format('truetype');
  font-weight: 400;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Medium.ttf') format('truetype');
  font-weight: 500;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-SemiBold.ttf') format('truetype');
  font-weight: 600;
}
@font-face {
  font-family: 'Space Grotesk';
  src: url('./assets/fonts/SpaceGrotesk-Bold.ttf') format('truetype');
  font-weight: 700;
}

* { font-family: 'Space Grotesk', sans-serif; }
</style>

