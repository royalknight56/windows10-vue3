<template>
    <div class="notify-center">
        <div class="message-title">
            <!-- <span @click="allClear" class="allclear">×</span> -->
        </div>
        <div class="message-group scroll-bar">
            <div v-if="notifyGroup.current.length === 0" class="no-message">
                {{ i18n('no.message') }}
            </div>
            <div class="message-item" v-for="notify in notifyGroup.current" :key="notify.id">
                <div class="message-item-title">
                    <span>{{ notify.title }}</span>
                </div>
                <div class="message-item-body">
                    <span>{{ notify.content }}</span>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup lang="ts">
import { System } from '@packages/kernel';
import { inject, ref } from 'vue';
import { i18n } from '@/packages/plug';

const sys = inject<System>('system')!;
const rootState = sys.stateManager;
const notifyGroup = rootState.notify;

const isPopShow = ref(false);
sys.mountEvent('messagecenter.show', () => {
    isPopShow.value = !isPopShow.value;
});
sys.mountEvent('messagecenter.hidden', () => {
    isPopShow.value = false;
});
function allClear() {
    rootState.notify.clear();
}
</script>
<style lang="scss" scoped>
.notify-center {
    height: 400px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    border-bottom: var(--border-gray);
}
.message-title {
    padding: 10px 20px;
    font-size: 16px;
    font-weight: bold;
    border-bottom: var(--border-gray);
    display: flex;
    justify-content: space-between;
    align-items: flex-end;

    .allclear {
        font-size: small;
        // float: right;
        cursor: pointer;

        &:hover {
            color: var(--color-blue);
        }
    }
}


.message-group {
    height: 100%;
    overflow: auto;
}

.no-message {
    padding: 10px 16px;
    width: var(--message-inner-width);
    overflow: hidden;
    margin: 4px auto;
    transform: translateY(50%);
    transition: all 0.2s ease;
    font-size: 14px;
    font-weight: bold;
    text-align: center;
    color: #494949;
}

.message-item {
    padding: 10px 16px;
    background: var(--color-gray-active);
    width: var(--message-inner-width);
    overflow: hidden;
    margin: 4px auto;
    border: var(--border-transparent);
    transition: all 0.2s ease;

    .message-item-title {
        font-size: 16px;
        font-weight: bold;
        margin-bottom: 2px;
        text-overflow: ellipsis;
        overflow: hidden;
    }

    .message-item-body {
        font-size: 14px;
    }
}

.message-item:hover {
    border: var(--border-gray);
}
</style>