<template>
  <div class="min-h-screen bg-cream-100 p-4 md:p-6 text-espresso-900">
    <!-- 頂部 Header & 頁籤切換按鈕 -->
    <header class="max-w-5xl mx-auto flex justify-between items-center bg-cream-50 p-4 rounded-2xl border border-cream-200 mb-6 shadow-sm">
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 rounded-full bg-espresso-800 text-cream-50 flex items-center justify-center font-serif font-bold text-sm">
          C
        </div>
        <h1 class="font-bold text-lg text-espresso-900">Café Lumière</h1>
      </div>

      <!-- 切換 菜單 / 後台 按鈕 -->
      <button 
        @click="currentTab = (currentTab === 'menu' ? 'admin' : 'menu')"
        class="px-3.5 py-1.5 rounded-xl text-xs font-bold transition flex items-center gap-1 shadow-sm"
        :class="currentTab === 'admin' ? 'bg-amber-600 text-white' : 'bg-cream-200 text-espresso-800 hover:bg-cream-300'"
      >
        <span>{{ currentTab === 'menu' ? '切換至店員後台' : '切換至顧客菜單' }}</span>
      </button>
    </header>

    <!-- 主要內容區域 -->
    <main class="max-w-5xl mx-auto">
      
      <!-- ==================== 頁面 1: 菜單瀏覽頁 (顧客端) ==================== -->
      <section v-if="currentTab === 'menu'" class="space-y-4">
        <h2 class="text-xl font-bold border-b border-cream-200 pb-2">精選菜單</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4.5">
          <ProductCard 
            v-for="product in sampleProducts" 
            :key="product.id" 
            :item="product" 
            @select="handleProductSelect"
          />
        </div>
      </section>

      <!-- ==================== 頁面 2: 店員後台頁 (管理端) ==================== -->
      <section v-else-if="currentTab === 'admin'" class="space-y-4 max-w-2xl mx-auto">
        <h2 class="text-xl font-bold border-b border-cream-200 pb-2">店員預訂管理中心</h2>
        
        <div class="space-y-3">
          <AdminOrderCard 
            v-for="order in sampleOrders" 
            :key="order.orderNum" 
            :order="order" 
            @update-status="handleUpdateStatus"
          />
        </div>
      </section>

    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 引入先前建立好的兩個元件
import ProductCard from './ProductCard.vue'
import AdminOrderCard from './AdminOrderCard.vue'

// 目前切換的頁籤 ('menu' | 'admin')
const currentTab = ref('menu')

// 1. 菜單模擬資料
const sampleProducts = ref([
  {
    id: 1,
    name: '手沖耶加雪菲 (日曬)',
    description: '展現豐富花香與水蜜桃甜香，水果酸甜調性平衡，現點現磨手沖萃取。',
    price: 160,
    isWholeCake: false,
    image: 'https://images.unsplash.com/photo-1514432324607-a09d9b4aefdd?auto=format&fit=crop&w=600&q=80'
  },
  {
    id: 5,
    name: '酪梨燻雞巴馬森可頌',
    description: '現烤酥脆可頌夾入新鮮酪梨、煙燻雞肉與特調醬汁，份量充足清爽無負擔。',
    price: 190,
    isWholeCake: false,
    image: 'https://images.unsplash.com/photo-1550547660-d9450f859349?auto=format&fit=crop&w=600&q=80'
  },
  {
    id: 7,
    name: '法式經典草莓千層蛋糕',
    description: '選用大湖草莓與香草籽卡士達，多層酥脆手工派皮，香濃酸甜不膩口。含盤叉與蠟燭選項。',
    basePrice: 880,
    isWholeCake: true,
    image: 'https://images.unsplash.com/photo-1535141192574-5d4897c13136?auto=format&fit=crop&w=600&q=80'
  }
])

// 2. 訂單模擬資料
const sampleOrders = ref([
  {
    orderNum: '#A1001',
    name: '陳小姐',
    phone: '0912345678',
    pickupDate: '2026-10-15',
    pickupTime: '14:00 - 14:30',
    paymentMethod: 'linepay',
    status: 'pending',
    total: 1040,
    items: [
      {
        name: '法式經典草莓千層蛋糕',
        qty: 1,
        price: 880,
        isWholeCake: true,
        isDrink: false,
        options: {
          size: '6 吋 (適合 4-6 人)',
          candle: '數字蠟燭',
          candleNum: '28',
          forks: '預設含 1 組 (5人份)',
          cardText: 'Happy 28th Bday!',
          note: '請附火柴'
        }
      }
    ]
  },
  {
    orderNum: '#A1002',
    name: '林先生',
    phone: '0987654321',
    pickupDate: '2026-10-13',
    pickupTime: '15:00 - 15:30',
    paymentMethod: 'store',
    status: 'ready',
    total: 340,
    items: [
      { 
        name: '酪梨燻雞巴馬森可頌', 
        qty: 1, 
        price: 190, 
        isWholeCake: false, 
        isDrink: false, 
        options: { note: '去堅果' } 
      }
    ]
  }
])

// 事件處理邏輯
const handleProductSelect = (selectedItem) => {
  console.log('點擊了商品：', selectedItem)
}

const handleUpdateStatus = (orderNum, newStatus) => {
  const targetOrder = sampleOrders.value.find(o => o.orderNum === orderNum)
  if (targetOrder) {
    targetOrder.status = newStatus
  }
}
</script>