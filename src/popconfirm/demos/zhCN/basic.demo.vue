<markdown>
# 基础用法
</markdown>

<script lang="ts" setup>
import { useMessage } from 'naive-ui'

const message = useMessage()

function handlePositiveClick() {
  message.info('是的')
}

function handleNegativeClick() {
  message.info('并不')
}
</script>

<template>
  <n-popconfirm
    @positive-click="handlePositiveClick"
    @negative-click="handleNegativeClick"
  >
    <template #trigger>
      <n-button>引用</n-button>
    </template>
    一切都将一去杳然，任何人都无法将其捕获。
  </n-popconfirm>
</template>
