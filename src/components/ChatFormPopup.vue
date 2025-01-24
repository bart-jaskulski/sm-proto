<template>
  <n-card class="chat-popup" :bordered="false">
    <template #header>
      <div class="popup-header">
        <h3>Chat</h3>
        <div class="header-buttons">
          <n-button quaternary @click="resetForm">
            <template #icon>
              <n-icon><refresh-icon /></n-icon>
            </template>
          </n-button>
          <n-button quaternary @click="closePopup">
            <template #icon>
              <n-icon><close-icon /></n-icon>
            </template>
          </n-button>
        </div>
      </div>
    </template>

    <div class="chat-messages" ref="chatMessages">
      <div v-for="(message, index) in messages" :key="index" 
           :class="['message', message.type]">
        {{ message.text }}
      </div>
    </div>

    <div v-if="showResponse" class="response-actions">
      <n-button @click="openAutomation">Open Automation</n-button>
      <div class="feedback-buttons">
        <n-button quaternary @click="submitFeedback('positive')">
          <template #icon>
            <n-icon><thumb-up-icon /></n-icon>
          </template>
        </n-button>
        <n-button quaternary @click="submitFeedback('negative')">
          <template #icon>
            <n-icon><thumb-down-icon /></n-icon>
          </template>
        </n-button>
      </div>
    </div>

    <template #footer>
      <div v-if="!userSubmitted" class="footer-content">
        <n-input 
          v-model:value="userInput" 
          type="textarea" 
          placeholder="Type your message..."
          :maxlength="1000"
          show-count
        />
        <n-button type="primary" @click="submitMessage" :disabled="!userInput.trim()">
          Send
        </n-button>
      </div>
    </template>
  </n-card>
</template>

<script setup>
import { ref, nextTick } from 'vue'
import { NCard, NButton, NInput, NIcon } from 'naive-ui'
import { CloseOutline as CloseIcon, ThumbsUpOutline as ThumbUpIcon, ThumbsDownOutline as ThumbDownIcon, RefreshOutline as RefreshIcon } from '@vicons/ionicons5'

const userInput = ref('')
const messages = ref([])
const showResponse = ref(false)
const userSubmitted = ref(false)
const chatMessages = ref(null)

const submitMessage = () => {
  if (userInput.value.trim()) {
    messages.value.push({ type: 'user', text: userInput.value.trim() })
    userSubmitted.value = true
    // Simulate response (replace with actual API call in real application)
    setTimeout(() => {
      messages.value.push({ type: 'system', text: 'Thank you for your message. How can I assist you further?' })
      showResponse.value = true
      scrollToBottom()
    }, 1000)
  }
}

const resetForm = () => {
  userInput.value = ''
  messages.value = []
  showResponse.value = false
  userSubmitted.value = false
}

const closePopup = () => {
  // Implement close functionality (e.g., emit an event to parent component)
  console.log('Close popup')
}

const openAutomation = () => {
  // Implement open automation functionality
  console.log('Open automation')
}

const submitFeedback = (type) => {
  // Implement feedback submission (e.g., send to server)
  console.log(`Feedback submitted: ${type}`)
}

const scrollToBottom = () => {
  nextTick(() => {
    if (chatMessages.value) {
      chatMessages.value.scrollTop = chatMessages.value.scrollHeight
    }
  })
}
</script>

<style scoped>
.chat-popup {
  width: 400px;
  height: 500px;
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-buttons {
  display: flex;
  gap: 8px;
}

.chat-messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.message {
  max-width: 80%;
  padding: 8px 12px;
  border-radius: 18px;
  word-wrap: break-word;
}

.message.user {
  align-self: flex-end;
  background-color: #007bff;
  color: white;
  border-bottom-right-radius: 4px;
}

.message.system {
  align-self: flex-start;
  background-color: #f0f0f0;
  color: #333;
  border-bottom-left-radius: 4px;
}

.response-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}

.feedback-buttons {
  display: flex;
  gap: 8px;
}

.footer-content {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.footer-content .n-button {
  align-self: flex-end;
}
</style>

