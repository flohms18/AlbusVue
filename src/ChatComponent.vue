<script setup>
import { ref, nextTick } from 'vue';
import ollama from 'ollama';

const messages = ref([]);
const userInput = ref('');
const isLoading = ref(false);
const chatWindow = ref(null);

const suggestions = [
  'What is machine learning?',
  'Explain LLMs in simple terms',
  'How does data engineering work?',
  'What is a neural network?',
];

async function sendMessage(text) {
    const content = (text !== undefined ? text : userInput.value).trim();
    if (!content || isLoading.value) return;

    messages.value.push({ role: 'user', content });
    userInput.value = '';
    isLoading.value = true;

    await nextTick();
    scrollToBottom();

    try {
        const res = await ollama.chat({
            model: 'albus',
            messages: messages.value.map(m => ({ role: m.role, content: m.content })),
        });
        messages.value.push({ role: 'assistant', content: res.message.content });
    } catch {
        messages.value.push({ role: 'assistant', content: 'Sorry, something went wrong. Make sure Ollama is running.' });
    } finally {
        isLoading.value = false;
        await nextTick();
        scrollToBottom();
    }
}

function scrollToBottom() {
    if (chatWindow.value) {
        chatWindow.value.scrollTop = chatWindow.value.scrollHeight;
    }
}

function handleKeydown(e) {
    if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        sendMessage();
    }
}
</script>

<template>
<div class="chat-page">

  <!-- Welcome screen -->
  <div v-if="messages.length === 0" class="welcome">
    <div class="welcome-avatar">A</div>
    <h1 class="welcome-title">What can I help with?</h1>
    <p class="welcome-sub">Ask me anything about data & AI</p>
    <div class="suggestion-grid">
      <button
        v-for="s in suggestions"
        :key="s"
        class="suggestion-chip"
        @click="sendMessage(s)"
        :disabled="isLoading"
      >{{ s }}</button>
    </div>
  </div>

  <!-- Chat messages -->
  <div v-else class="chat-window" ref="chatWindow">
    <div v-for="(msg, i) in messages" :key="i" class="message-row" :class="msg.role">
      <div v-if="msg.role === 'assistant'" class="avatar">A</div>
      <div class="msg-content" :class="msg.role">
        <p>{{ msg.content }}</p>
      </div>
    </div>

    <!-- Typing indicator -->
    <div v-if="isLoading" class="message-row assistant">
      <div class="avatar">A</div>
      <div class="msg-content assistant typing">
        <span class="dot"></span>
        <span class="dot"></span>
        <span class="dot"></span>
      </div>
    </div>
  </div>

  <!-- Input area -->
  <div class="input-wrap">
    <div class="input-box">
      <textarea
        v-model="userInput"
        @keydown="handleKeydown"
        placeholder="Message Albus..."
        rows="1"
        class="chat-input"
        :disabled="isLoading"
      ></textarea>
      <button
        @click="sendMessage()"
        class="send-btn"
        :disabled="isLoading || !userInput.trim()"
      >
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
          <path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/>
        </svg>
      </button>
    </div>
    <p class="disclaimer">Albus can make mistakes. Verify important information.</p>
  </div>

</div>
</template>

<style scoped>
.chat-page {
  flex: 1;
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: hidden;
  background: var(--bg);
  color: var(--text);
}

/* Welcome screen */
.welcome {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem 1.5rem;
  gap: 1rem;
}

.welcome-avatar {
  width: 52px;
  height: 52px;
  border-radius: 50%;
  background: var(--violet);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
  font-weight: 700;
}

.welcome-title {
  font-size: 1.75rem;
  font-weight: 600;
  color: var(--text);
  text-align: center;
}

.welcome-sub {
  font-size: 0.9rem;
  color: var(--text-muted);
  text-align: center;
}

.suggestion-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.6rem;
  margin-top: 0.5rem;
  max-width: 540px;
  width: 100%;
}

.suggestion-chip {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 10px;
  color: var(--text);
  cursor: pointer;
  font-family: inherit;
  font-size: 0.82rem;
  padding: 0.75rem 1rem;
  text-align: left;
  line-height: 1.4;
  transition: background 0.15s, border-color 0.15s;
}

.suggestion-chip:hover:not(:disabled) {
  background: #333;
  border-color: #484848;
}

/* Chat window */
.chat-window {
  flex: 1;
  overflow-y: auto;
  padding: 2rem 0;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  scrollbar-width: thin;
  scrollbar-color: var(--border) transparent;
}

.chat-window::-webkit-scrollbar { width: 5px; }
.chat-window::-webkit-scrollbar-thumb {
  background: var(--border);
  border-radius: 4px;
}

/* Message rows — centered column like Claude */
.message-row {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  padding: 0 max(1.5rem, calc(50% - 360px));
}

.message-row.user {
  flex-direction: row-reverse;
}

.avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: var(--violet);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.75rem;
  font-weight: 700;
  flex-shrink: 0;
  margin-top: 2px;
}

.msg-content {
  font-size: 0.9rem;
  line-height: 1.7;
  color: var(--text);
  max-width: 100%;
}

.msg-content.user {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 18px;
  border-bottom-right-radius: 4px;
  padding: 0.65rem 1rem;
  max-width: 75%;
}

.msg-content.assistant {
  padding-top: 0.1rem;
}

.msg-content p {
  margin: 0;
  white-space: pre-wrap;
  word-break: break-word;
}

/* Typing dots */
.typing {
  display: flex;
  align-items: center;
  gap: 4px;
  padding-top: 0.5rem;
}

.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--text-muted);
  animation: bounce 1.2s infinite;
}
.dot:nth-child(2) { animation-delay: 0.2s; }
.dot:nth-child(3) { animation-delay: 0.4s; }

@keyframes bounce {
  0%, 80%, 100% { transform: translateY(0); opacity: 0.4; }
  40%           { transform: translateY(-5px); opacity: 1; }
}

/* Input area */
.input-wrap {
  padding: 1rem max(1.5rem, calc(50% - 380px));
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  flex-shrink: 0;
}

.input-box {
  display: flex;
  align-items: flex-end;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 0.5rem 0.5rem 0.5rem 1rem;
  gap: 0.5rem;
  transition: border-color 0.2s;
}

.input-box:focus-within {
  border-color: #555;
}

.chat-input {
  flex: 1;
  background: transparent;
  border: none;
  outline: none;
  color: var(--text);
  font-family: inherit;
  font-size: 0.9rem;
  line-height: 1.5;
  resize: none;
  max-height: 160px;
  overflow-y: auto;
  padding: 0.35rem 0;
}

.chat-input::placeholder {
  color: var(--text-muted);
}

.chat-input:disabled {
  opacity: 0.6;
}

.send-btn {
  background: var(--text);
  border: none;
  border-radius: 8px;
  color: var(--bg);
  cursor: pointer;
  padding: 0.45rem 0.55rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  align-self: flex-end;
  transition: opacity 0.2s;
}

.send-btn:disabled {
  opacity: 0.2;
  cursor: not-allowed;
}

.send-btn:hover:not(:disabled) {
  opacity: 0.8;
}

.disclaimer {
  font-size: 0.72rem;
  color: var(--text-muted);
  text-align: center;
}

@media (max-width: 640px) {
  .welcome-title { font-size: 1.4rem; }
  .suggestion-grid { grid-template-columns: 1fr; }
  .input-wrap { padding: 1rem 0.75rem; }
  .message-row { padding: 0 0.75rem; }
}
</style>
