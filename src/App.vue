<template>
  <n-config-provider>
    <n-message-provider>
      <div class="app-container">
        <h1>Chat Form Popup Preview</h1>
        <chat-form-popup />
      </div>
    </n-message-provider>
  </n-config-provider>
</template>

<script setup>
import { NConfigProvider, NMessageProvider } from 'naive-ui'
import ChatFormPopup from './components/ChatFormPopup.vue'
</script>

<style>
.app-container {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}
</style>