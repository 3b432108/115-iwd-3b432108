<script setup>
import { ref, computed } from 'vue'

// 使用 ref 宣告綁定輸入框的字串資料
const inputText = ref('')

// 使用 computed 計算輸入內容的字數長度
const textLength = computed(() => {
  return inputText.value.length
})
</script>

<template>
  <div class="container">
    <h2>即時字數計數器</h2>

    <!-- 使用 v-model 雙向綁定 inputText -->
    <input
      v-model="inputText"
      type="text"
      placeholder="請在此輸入文字..."
      class="input-field"
    />

    <!-- 即時顯示輸入內容 -->
    <div class="result-box">
      <p><strong>目前內容：</strong> {{ inputText || '（尚未輸入）' }}</p>
      <!-- 使用 computed 算出的字數 -->
      <p><strong>字數統計：</strong> <span class="highlight">{{ textLength }}</span> 個字</p>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 480px;
  margin: 30px auto;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
  font-family: system-ui, -apple-system, sans-serif;
}

h2 {
  color: #2c3e50;
  margin-top: 0;
  margin-bottom: 20px;
  text-align: center;
}

.input-field {
  width: 100%;
  padding: 10px 14px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 6px;
  box-sizing: border-box;
  outline: none;
  transition: border-color 0.2s;
}

.input-field:focus {
  border-color: #42b883;
}

.result-box {
  margin-top: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 6px;
  line-height: 1.6;
}

.result-box p {
  margin: 8px 0;
  color: #333;
}

.highlight {
  color: #42b883;
  font-weight: bold;
  font-size: 1.2rem;
}
</style>