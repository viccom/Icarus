<template>
<!-- 搞不懂为什么有时候加载未完成，那边就会触发渲染了，不挡一下的话会报错 -->
<span v-if="item && item.id">
    <!-- 用户 -->
    <template v-if="type === state.misc.POST_TYPES.USER">
        <span v-if="showType" class="type-name" :class="{'bold': typeBold}">{{state.misc.POST_TYPES_TXT[type]}}</span>
        <router-link :to="{ name: 'account_userpage', params: {id: item.id} }" :title="getPostTitle(item, 'nickname')">
            <template v-if="!useSlot">
                <template>{{text(getPostTitle(item, 'nickname')) || '错误的值'}}</template>
                <span v-if="goto">🔗</span>
            </template>
            <slot v-else />
        </router-link>
    </template>

    <!-- 板块 -->
    <template v-else-if="type === state.misc.POST_TYPES.BOARD">
        <span v-if="showType" class="type-name" :class="{'bold': typeBold}">{{state.misc.POST_TYPES_TXT[type]}}</span>
        <router-link :to="{ name: 'forum_board', params: {id: item.id} }" :title="getPostTitle(item, 'name')">
            <template v-if="!useSlot">
                <template>{{text(getPostTitle(item, 'name')) || '错误的值'}}</template>
                <span v-if="goto">📮</span> <!--💬-->
            </template>
            <slot v-else />
        </router-link>
    </template>

    <!-- 主题 -->
    <template v-else-if="type === state.misc.POST_TYPES.TOPIC">
        <span v-if="showType" class="type-name" :class="{'bold': typeBold}">{{state.misc.POST_TYPES_TXT[type]}}</span>
        <router-link :to="{ name: 'forum_topic', params: {id: item.id} }" :title="getPostTitle(item, 'title')">
            <template v-if="!useSlot">
                <template>{{text(getPostTitle(item, 'title')) || '错误的值'}}</template>
                <span v-if="goto">📝</span>
            </template>
            <slot v-else />
        </router-link>
    </template>

    <!-- 评论 -->
    <template v-else-if="type === state.misc.POST_TYPES.COMMENT">
        <span v-if="showType" class="type-name" :class="{'bold': typeBold}">{{state.misc.POST_TYPES_TXT[type]}}</span>
        <template v-if="!useSlot">
            <span class="limited-title">“{{text(item.content)}}”</span>
        </template>
        <slot v-else />
    </template>
</span>
</template>

<style scoped>
.bold {
    font-weight: bold;
}

.limited-title {
    max-width: 120px;
    display: inline-flex;
    white-space: nowrap;
    word-break: keep-all;
    text-overflow: ellipsis;
    overflow: hidden;
}
</style>

<script>
import state from '@/state.js'

export default {
    data () {
        return {
            state
        }
    },
    props: {
        item: {},
        type: null,
        showType: {
            default: false
        },
        typeBold: {
            default: false
        },
        textLimit: {
            default: 0
        },
        goto: {
            default: false
        },
        useSlot: {
            default: false
        }
    },
    methods: {
        getPostTitle: function (i, key) {
            if (i['post_title']) return i['post_title']
            return i[key]
        },
        text: function (txt) {
            if (this.textLimit > 0) {
                let suffix = this.textLimit >= txt.length ? '' : '…'
                return txt.slice(0, this.textLimit) + suffix
            }
            return txt
        }
    }
}
</script>
