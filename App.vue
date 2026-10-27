<script setup>
import { ref, computed } from 'vue'

// 宣告身高（cm）與體重（kg）的響應式狀態
const height = ref('')
const weight = ref('')

// 使用 computed 計算 BMI 值：體重(kg) / (身高(m) * 身高(m))
const bmi = computed(() => {
  const h = parseFloat(height.value) / 100
  const w = parseFloat(weight.value)

  if (!h || !w || h <= 0 || w <= 0) {
    return null
  }

  // 計算並四捨五入保留一位小數
  return (w / (h * h)).toFixed(1)
})

// 使用 computed 自動判斷 BMI 體型狀態
const bmiStatus = computed(() => {
  if (!bmi.value) return null

  const val = parseFloat(bmi.value)
  if (val < 18.5) return { text: '體重過輕', color: '#3182ce' }
  if (val < 24) return { text: '健康體位', color: '#38a169' }
  if (val < 27) return { text: '過重', color: '#dd6b20' }
  return { text: '肥胖', color: '#e53e3e' }
})
</script>

<template>
  <div class="bmi-card">
    <h2>BMI 體重指數計算機</h2>

    <div class="input-group">
      <label>身高 (cm)：</label>
      <input
        v-model.number="height"
        type="number"
        placeholder="例如：170"
        min="0"
      />
    </div>

    <div class="input-group">
      <label>體重 (kg)：</label>
      <input
        v-model.number="weight"
        type="number"
        placeholder="例如：65"
        min="0"
      />
    </div>

    <div class="result-box">
      <div v-if="bmi">
        <p class="bmi-score">BMI 數值：<span>{{ bmi }}</span></p>
        <p class="bmi-status" :style="{ color: bmiStatus.color }">
          體型狀態：<strong>{{ bmiStatus.text }}</strong>
        </p>
      </div>
      <p v-else class="placeholder">請輸入有效的身高與體重資料</p>
    </div>
  </div>
</template>

<style scoped>
.bmi-card {
  max-width: 400px;
  margin: 40px auto;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
  font-family: system-ui, -apple-system, sans-serif;
}

h2 {
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
  margin-bottom: 24px;
}

.input-group {
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
}

.input-group label {
  font-weight: bold;
  margin-bottom: 6px;
  color: #4a5568;
}

.input-group input {
  padding: 10px 12px;
  font-size: 16px;
  border: 1px solid #cbd5e0;
  border-radius: 6px;
  outline: none;
  transition: border-color 0.2s;
}

.input-group input:focus {
  border-color: #42b883;
}

.result-box {
  margin-top: 24px;
  padding: 16px;
  background-color: #f7fafc;
  border-radius: 8px;
  text-align: center;
}

.bmi-score {
  font-size: 1.25rem;
  margin: 0 0 8px 0;
  color: #2d3748;
}

.bmi-score span {
  font-size: 1.75rem;
  font-weight: bold;
  color: #42b883;
}

.bmi-status {
  font-size: 1.1rem;
  margin: 0;
}

.placeholder {
  color: #a0aec0;
  margin: 0;
}
</style>
