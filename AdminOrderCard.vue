<template>
  <div class="bg-white rounded-2xl p-4 shadow-sm border border-cream-200 space-y-3 relative">
    <!-- 單號與狀態 Badge -->
    <div class="flex justify-between items-center border-b border-cream-100 pb-2">
      <div class="flex items-center gap-2">
        <span class="font-extrabold text-sm text-espresso-900">{{ order.orderNum }}</span>
        <span 
          class="text-[10px] px-2 py-0.5 rounded-md font-bold"
          :class="{
            'bg-amber-100 text-amber-800': order.status === 'pending',
            'bg-blue-100 text-blue-800': order.status === 'ready',
            'bg-green-100 text-green-800': order.status === 'completed'
          }" 
        >
          {{ statusLabel }}
        </span>
      </div>
      <span class="text-xs font-bold text-terracotta">NT$ {{ order.total }}</span>
    </div>

    <!-- 訂購人與取貨關鍵資訊 -->
    <div class="text-xs space-y-1 bg-cream-50 p-2.5 rounded-xl border border-cream-200/60">
      <div class="flex justify-between">
        <span class="text-espresso-600">顧客姓名：</span>
        <span class="font-bold text-espresso-900">{{ order.name }} ({{ order.phone }})</span>
      </div>
      <div class="flex justify-between">
        <span class="text-espresso-600">取貨時間：</span>
        <span class="font-bold text-terracotta">{{ order.pickupDate }} {{ order.pickupTime }}</span>
      </div>
      <div class="flex justify-between">
        <span class="text-espresso-600">付款方式：</span>
        <span class="font-semibold text-espresso-800">
          {{ order.paymentMethod === 'store' ? '現場付款' : '已線上付款' }}
        </span>
      </div>
    </div>

    <!-- 備料品項與客製選單內容 (店員核對區) -->
    <div class="space-y-2 text-xs">
      <p class="font-bold text-espresso-800 text-[11px]">訂購餐點與客製說明：</p>
      <div 
        v-for="(item, i) in order.items" 
        :key="i" 
        class="pl-2 border-l-2 border-espresso-800 space-y-0.5"
      >
        <div class="font-bold text-espresso-900 flex justify-between">
          <span>{{ item.name }} x {{ item.qty }}</span>
          <span class="text-espresso-600">NT$ {{ item.price * item.qty }}</span>
        </div>

        <!-- 整塊蛋糕細節 -->
        <div 
          v-if="item.isWholeCake" 
          class="text-[11px] text-espresso-700 bg-amber-500/5 p-2 rounded-lg mt-1 space-y-0.5 border border-amber-500/20"
        >
          <p>・尺寸：<span class="font-semibold">{{ item.options.size }}</span></p>
          <p>・蠟燭：<span class="font-semibold">{{ item.options.candle }}{{ item.options.candleNum ? ` (${item.options.candleNum})` : '' }}</span></p>
          <p>・盤叉：<span class="font-semibold">{{ item.options.forks }}</span></p>
          <p>・提字：<span class="font-bold text-terracotta">{{ item.options.cardText || '無' }}</span></p>
          <p v-if="item.options.note">・備註：<span class="font-semibold text-red-600">{{ item.options.note }}</span></p>
        </div>

        <!-- 飲品細節 -->
        <div 
          v-if="item.isDrink" 
          class="text-[11px] text-espresso-700 bg-cream-50 p-2 rounded-lg mt-1 space-y-0.5 border border-cream-200/60"
        >
          <p>・規格：<span class="font-bold text-espresso-900">{{ item.options.temp }} / {{ item.options.sweetness }}</span></p>
          <p v-if="item.options.note">・備註：<span class="font-semibold text-red-600">{{ item.options.note }}</span></p>
        </div>
      </div>
    </div>

    <!-- 店員操作按鈕 (更新狀態) -->
    <div class="pt-2 flex gap-2 border-t border-cream-100">
      <button 
        v-if="order.status === 'pending'" 
        @click="$emit('update-status', order.orderNum, 'ready')"
        class="flex-1 py-2 bg-amber-600 hover:bg-amber-700 text-white rounded-xl text-xs font-bold transition flex items-center justify-center gap-1"
      >
        <i data-lucide="box" class="w-3.5 h-3.5"></i>
        <span>完成製作，通知取貨</span>
      </button>

      <button 
        v-if="order.status === 'ready'" 
        @click="$emit('update-status', order.orderNum, 'completed')"
        class="flex-1 py-2 bg-green-700 hover:bg-green-800 text-white rounded-xl text-xs font-bold transition flex items-center justify-center gap-1"
      >
        <i data-lucide="check-circle" class="w-3.5 h-3.5"></i>
        <span>顧客已來店核銷取貨</span>
      </button>

      <button 
        v-if="order.status === 'completed'" 
        @click="$emit('update-status', order.orderNum, 'pending')"
        class="py-2 px-3 bg-cream-200 text-espresso-800 rounded-xl text-xs font-bold transition"
      >
        重設為待處理
      </button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  order: {
    type: Object,
    required: true,
    default: () => ({
      orderNum: '',
      name: '',
      phone: '',
      pickupDate: '',
      pickupTime: '',
      paymentMethod: '',
      status: 'pending',
      total: 0,
      items: []
    })
  }
})

defineEmits(['update-status'])

// 狀態文字轉換對應
const statusLabel = computed(() => {
  const labels = {
    pending: '待製作',
    ready: '待取貨',
    completed: '已核銷取貨'
  }
  return labels[props.order.status] || props.order.status
})
</script>