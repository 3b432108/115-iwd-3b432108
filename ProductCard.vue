<template>
  <div 
    @click="$emit('select', item)" 
    class="bg-white rounded-2xl shadow-sm border border-cream-200/80 overflow-hidden hover:shadow-md transition cursor-pointer flex flex-col justify-between group"
  >
    <!-- 上方圖片區塊 -->
    <div class="relative w-full h-48 bg-cream-100 overflow-hidden">
      <img 
        :src="item.image" 
        :alt="item.name" 
        class="w-full h-full object-cover group-hover:scale-105 transition duration-300"
      >
      <!-- 需預訂 Badge -->
      <span 
        v-if="item.isWholeCake" 
        class="absolute top-2.5 right-2.5 bg-amber-500/90 backdrop-blur-xs text-white text-[10px] font-bold px-2.5 py-1 rounded-md shadow-sm"
      >
        需預訂
      </span>
    </div>

    <!-- 下方內容區塊 -->
    <div class="p-4 flex-1 flex flex-col justify-between space-y-3">
      <div>
        <h3 class="font-bold text-base text-espresso-900 leading-tight mb-1.5">
          {{ item.name }}
        </h3>
        <p class="text-xs text-espresso-600/80 line-clamp-2 leading-relaxed">
          {{ item.description }}
        </p>
      </div>
      
      <!-- 價格與按鈕區 -->
      <div class="pt-3 border-t border-cream-100 flex items-center justify-between mt-auto">
        <div>
          <span class="text-[10px] text-espresso-600 block">起價</span>
          <span class="font-bold text-espresso-900 text-sm">
            NT$ {{ item.isWholeCake ? item.basePrice : item.price }} 
            <span class="text-xs font-normal text-espresso-600">起</span>
          </span>
        </div>
        
        <button 
          @click.stop="$emit('select', item)" 
          class="px-3.5 py-2 rounded-xl bg-espresso-800 hover:bg-espresso-900 text-cream-50 font-semibold text-xs transition flex items-center gap-1 shadow-sm active:scale-95"
        >
          <span>{{ item.isWholeCake ? '選擇規格' : '詳細內容' }}</span>
          <!-- Lucide Icon 或圖示即可放此 -->
          <i data-lucide="chevron-right" class="w-3.5 h-3.5"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
// 定義傳入的 Props
const props = defineProps({
  item: {
    type: Object,
    required: true,
    default: () => ({
      id: 0,
      name: '',
      description: '',
      price: 0,
      basePrice: 0,
      isWholeCake: false,
      image: ''
    })
  }
})

// 定義向外觸發的事件
defineEmits(['select'])
</script>