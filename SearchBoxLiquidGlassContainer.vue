<!--
  SearchBoxLiquidGlassContainer 带防抖的毛玻璃搜索框组件
  versions 260504
  
  Props:
    search_form_submit
      - 类型: Function(searchValue[string]: inputValue)
      - 默认: () => {}
      - 说明: 表单提交回调，组件内部已做 500ms 防抖
    delay
      - 类型: Number
      - 默认: 500
      - 说明: 输入框输入内容后，触发表单提交回调的延迟时间，单位为毫秒
    max_width
      - 类型: Number
      - 默认: 500
      - 说明: 组件宽度 范围150-
    max_height
      - 类型: Number
      - 默认: 35
      - 说明: 组件高度 范围35-50

  使用示例:
    <SearchBoxLiquidGlassContainer
      :search_form_submit="search_form_submit"
      :delay="500"
      :max_width="500"
      :max_height="35"
    />
    const search_form_submit = (searchValue) => {
        console.log(searchValue);
    };
-->

<template>
    <div class="glass_container" @click="() => inputRef.focus()">
        <div class="icon">
            <svg viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" width="100%" height="100%" opacity="0.5">
                <path
                    d="M446.112323 177.545051c137.567677 0.219798 252.612525 104.59798 266.162424 241.493333 13.562828 136.895354-78.778182 261.818182-213.617777 289.008485-134.852525 27.203232-268.386263-52.156768-308.945455-183.608889s25.018182-272.252121 151.738182-325.779394A267.235556 267.235556 0 0 1 446.112323 177.545051m0-62.060607c-182.794343 0-330.989899 148.195556-330.989899 330.989899s148.195556 330.989899 330.989899 330.989899 330.989899-148.195556 330.989899-330.989899-148.195556-330.989899-330.989899-330.989899z m431.321212 793.341415a30.849293 30.849293 0 0 1-21.94101-9.102223l-157.220202-157.220202c-11.752727-12.179394-11.584646-31.534545 0.37495-43.50707 11.972525-11.972525 31.327677-12.140606 43.494141-0.37495l157.220202 157.220202a31.036768 31.036768 0 0 1 6.723232 33.810101 31.004444 31.004444 0 0 1-28.651313 19.174142z m0 0"
                    fill="#ffffff"></path>
            </svg>
        </div>
        <form class="search_form" @submit.prevent="handleSearch(searchValue)">
            <input type="text" placeholder="搜索..." class="searchInput" name="search" ref="inputRef" v-model="searchValue" />
        </form>
    </div>
</template>

<script setup>
import { defineProps, ref, onUnmounted, useTemplateRef } from 'vue';

const timer = ref(null);
const inputRef = useTemplateRef('inputRef');
const searchValue = ref('');

const props = defineProps({
    search_form_submit: {
        type: Function,
        default: () => {}
    },
    delay: {
        type: Number,
        default: 500
    },
    max_width: {
        type: Number,
        default: 500
    },
    max_height: {
        type: Number,
        default: 35
    }
});

const handleSearch = function (...args) {
    if (timer.value) clearTimeout(timer.value);

    timer.value = setTimeout(() => {
        props.search_form_submit(...args);
    }, props.delay);
};

onUnmounted(() => {
    if (timer.value) {
        clearTimeout(timer.value);
        timer.value = null;
    }
});
</script>

<style scoped>
.glass_container {
    position: relative;
    box-sizing: border-box;
    width: v-bind('max_width + "px"');
    min-width: 150px;
    height: v-bind('max_height + "px"');
    min-height: 35px;
    max-height: 50px;
    padding: 0 10px;
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-top-color: rgba(255, 255, 255, 0.3);
    border-left-color: rgba(255, 255, 255, 0.3);
    border-radius: 999px;
    background-clip: padding-box;
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 10px;
    background-color: rgba(120, 120, 120, 0.15);
    backdrop-filter: blur(16px);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08), 0 1px 3px rgba(255, 255, 255, 0.05) inset;
    transition: all 0.3s ease;
}

.icon {
    width: 24px;
    height: 24px;
}

.search_form { 
    width: 90%;
}

.searchInput {
    background-color: transparent;
    border: none;
    display: block;
    color: rgba(255, 255, 255, 0.8);
    width: 95%;
    height: 16px;
    line-height: 16px;
}

.searchInput:focus {
    outline: none;
}
</style>
