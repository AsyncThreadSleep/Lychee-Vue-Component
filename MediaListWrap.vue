<!-- 
    MediaListWrap 电影卡片组件
    versions 260517

    Props:
        MediaData
            - 类型: Object
            - 默认: 无(必须传递)
            - 说明: 电影数据对象，包含海报路径、标题、描述、上映日期、评分等信息
            - 属性:
                - poster_path 海报路径 String
                - title 电影标题 String
                - overview 电影描述 String
                - release_date 上映日期 String
                - vote_average 电影评分 Number
        bgColor
            - 类型: String
            - 默认: #8ca68f
            - 说明: 背景颜色

    使用示例:
        <MediaListWrap
            bgColor="#2a2e33"
            :MediaData="data"
        />
-->

<template>
    <div class="movie-card">
        <div class="poster">
            <img :src="MediaData.poster_path" alt="海报" />
        </div>
        <div class="info">
            <h2 class="title">{{ MediaData.title }}</h2>
            <div class="divider"></div>
            <p class="desc">{{ MediaData.overview }}</p>
            <div class="bottom-row">
                <span class="date">{{ MediaData.release_date }}</span>
                <span class="score">✦ {{ MediaData.vote_average }}</span>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps({
    MediaData: {
        type: Object,
        required: true
    }
})
</script>

<style scoped>
.movie-card {
    display: flex;
    align-items: center;
    gap: 24px;
    width: 920px;
    padding: 32px;
    background-color: #2a2e33;
    border-radius: 20px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
    color: #ffffff;
}

.poster {
    flex-shrink: 0;
    width: 220px;
    height: 330px;
    border-radius: 12px;
    overflow: hidden;
}

.poster img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.info {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.title {
    font-size: 42px;
    font-weight: 600;
    margin: 0;
}

.divider {
    width: 280px;
    height: 4px;
    background-color: #4cd992;
    border-radius: 2px;
}

.desc {
    height: 155px;
    font-size: 24px;
    line-height: 1.6;
    color: #e0e0e0;
    margin: 0;
}

.bottom-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 12px;
}

.date {
    font-size: 24px;
    color: #cccccc;
}

.score {
    font-size: 32px;
    color: #ffd766;
    font-weight: bold;
}
</style>