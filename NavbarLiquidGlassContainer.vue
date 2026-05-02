<template>
    <div :class="['glass_container', props.direction, props.size]">
        <ul class="glass_container_list">
            <li :class="['glass_container_list_item', {'active': item === activeIndex}]" v-for="item in props.itemLength" :key="item" @click="handleClick($event,item)">
                <div class="item_container">
                    <slot :name="`item${item}`"></slot>
                </div>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { defineProps, defineEmits, ref } from 'vue';

const props = defineProps({
    itemLength: {
        type: Number,
        default: 0,
        validator: (value) => Number.isInteger(value) && value >= 0
    },
    direction: {
        type: String,
        default: 'vertical',
        validator: (value) => ['vertical', 'crosswise'].includes(value)
    },
    size: {
        type: String,
        default: 'medium',
        validator: (value) => ['small', 'medium', 'large'].includes(value)
    }
});

const activeIndex = ref(1);

const emit = defineEmits(['update:activeIndex']);

const handleClick = (e,index) => {
    activeIndex.value = index;
    emit('update:activeIndex', e,index);
};
</script>

<style scoped>
/* 组件大小（small/medium/large） */
.glass_container.small {
    --glass_container_item_size: 36px;
    --glass_container_paddingSize: 8px;
}

.glass_container.medium {
    --glass_container_item_size: 52px;
    --glass_container_paddingSize: 10px;
}

.glass_container.large {
    --glass_container_item_size: 68px;
    --glass_container_paddingSize: 12px;
}

/* 组件方向（vertical/crosswise） */
.glass_container.vertical {
    --glass_container_padding_up: calc(var(--glass_container_paddingSize) * 1.5);
    --glass_container_padding_left: var(--glass_container_paddingSize);
    --glass_container_list_display: column;
}

.glass_container.crosswise {
    --glass_container_padding_up: var(--glass_container_paddingSize);
    --glass_container_padding_left: calc(var(--glass_container_paddingSize) * 1.5);
    --glass_container_list_display: row;
}

.glass_container {
    /* 1. 盒模型基础（优先级最高，布局核心） */
    box-sizing: border-box;
    width: fit-content;
    height: fit-content;
    padding: var(--glass_container_padding_up) var(--glass_container_padding_left);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-top-color: rgba(255, 255, 255, 0.3);
    border-left-color: rgba(255, 255, 255, 0.3);
    border-radius: 999px;
    background-clip: padding-box;

    /* 2. 背景/颜色（视觉基础） */
    background-color: rgba(255, 255, 255, 0.15);

    /* 3. 视觉特效（毛玻璃/阴影，装饰性） */
    backdrop-filter: blur(16px);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08), 0 1px 3px rgba(255, 255, 255, 0.05) inset;

    /* 4. 过渡动画（交互相关，最后） */
    transition: all 0.3s ease;
}

.glass_container_list {
    display: flex;
    flex-direction: var(--glass_container_list_display);
    gap: 12px;
    list-style: none;
}

.glass_container_list_item {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    width: var(--glass_container_item_size);
    height: var(--glass_container_item_size);
}

.glass_container_list_item::after {
    /* 布局核心（定位相关） */
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    /* 盒模型（尺寸、边框、圆角） */
    width: calc(var(--glass_container_item_size) - 2px);
    height: calc(var(--glass_container_item_size) - 2px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 50%;

    /* 视觉基础（背景色） */
    background-color: rgba(255, 255, 255, 0.15);

    /* 视觉特效（透明度） */
    opacity: 0;

    /* 其他特殊属性（伪元素内容） */
    content: '';
}

.glass_container_list_item:hover::after {
    opacity: 1;
    cursor: pointer;
}

.item_container {
    width: 80%;
    height: 80%;
    overflow: hidden;
    border-radius: 50%;
}

.active {
    width: calc(var(--glass_container_item_size) - 2px);
    height: calc(var(--glass_container_item_size) - 2px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.15);
}
</style>