<markdown>
# Control option visibility

In some cases, different roles will see different options. You can use the `show` field to control the visibility of menu options.
</markdown>

<script lang="ts" setup>
import type { Component } from 'vue'
import {
  BookOutline as BookIcon,
  PersonOutline as PersonIcon
} from '@vicons/ionicons5'
import { NIcon } from 'naive-ui'
import { computed, h, ref } from 'vue'

const accordion = ref(false)

function renderIcon(icon: Component) {
  return () => h(NIcon, null, { default: () => h(icon) })
}

const options = computed(() => [
  {
    label: 'parent 1',
    icon: renderIcon(PersonIcon),
    key: '1',
    children: [
      {
        label: '1.1',
        key: '2'
      }
    ]
  },
  {
    label: 'parent 2',
    key: '3',
    icon: renderIcon(BookIcon),
    show: !accordion.value,
    children: [
      {
        label: '2.1',
        key: '4'
      },
      {
        label: '2.2',
        key: '5'
      },
      {
        label: '2.3',
        key: '6'
      }
    ]
  }
])
</script>

<template>
  <n-space vertical>
    <n-switch v-model:value="accordion">
      <template #checked>
        Hide menu
      </template>
      <template #unchecked>
        Show menu
      </template>
    </n-switch>
    <n-menu :options="options" />
  </n-space>
</template>
