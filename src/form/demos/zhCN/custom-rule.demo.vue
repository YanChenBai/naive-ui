<markdown>
# 自定义规则

有时候内置的 trigger 无法满足验证的需要。你可以通过设定自定的 trigger 然后手动触发它来控制验证。
</markdown>

<script lang="ts" setup>
import type { FormInst, FormItemInst, FormItemRule, FormRules } from 'naive-ui'
import { useMessage } from 'naive-ui'
import { ref } from 'vue'

interface ModelType {
  age: string | null
  password: string | null
  reenteredPassword: string | null
}

const formRef = ref<FormInst | null>(null)
const rPasswordFormItemRef = ref<FormItemInst | null>(null)
const message = useMessage()
const modelRef = ref<ModelType>({
  age: null,
  password: null,
  reenteredPassword: null
})

function validatePasswordStartWith(rule: FormItemRule, value: string): boolean {
  return (
    !!modelRef.value.password
    && modelRef.value.password.startsWith(value)
    && modelRef.value.password.length >= value.length
  )
}

function validatePasswordSame(rule: FormItemRule, value: string): boolean {
  return value === modelRef.value.password
}

const rules: FormRules = {
  age: [
    {
      required: true,
      validator(rule: FormItemRule, value: string) {
        if (!value) {
          return new Error('需要年龄')
        }
        else if (!/^\d*$/.test(value)) {
          return new Error('年龄应该为整数')
        }
        else if (Number(value) < 18) {
          return new Error('年龄应该超过十八岁')
        }
        return true
      },
      trigger: ['input', 'blur']
    }
  ],
  password: [
    {
      required: true,
      message: '请输入密码'
    }
  ],
  reenteredPassword: [
    {
      required: true,
      message: '请再次输入密码',
      trigger: ['input', 'blur']
    },
    {
      validator: validatePasswordStartWith,
      message: '两次密码输入不一致',
      trigger: 'input'
    },
    {
      validator: validatePasswordSame,
      message: '两次密码输入不一致',
      trigger: ['blur', 'password-input']
    }
  ]
}

function handlePasswordInput() {
  if (modelRef.value.reenteredPassword) {
    rPasswordFormItemRef.value?.validate({ trigger: 'password-input' })
  }
}

function handleValidateButtonClick(e: MouseEvent) {
  e.preventDefault()
  formRef.value?.validate((errors) => {
    if (!errors) {
      message.success('验证成功')
    }
    else {
      console.log(errors)
      message.error('验证失败')
    }
  })
}
</script>

<template>
  <n-form ref="formRef" :model="modelRef" :rules="rules">
    <n-form-item path="age" label="年龄">
      <n-input v-model:value="modelRef.age" @keydown.enter.prevent />
    </n-form-item>
    <n-form-item path="password" label="密码">
      <n-input
        v-model:value="modelRef.password"
        type="password"
        @input="handlePasswordInput"
        @keydown.enter.prevent
      />
    </n-form-item>
    <n-form-item
      ref="rPasswordFormItemRef"
      first
      path="reenteredPassword"
      label="重复密码"
    >
      <n-input
        v-model:value="modelRef.reenteredPassword"
        :disabled="!modelRef.password"
        type="password"
        @keydown.enter.prevent
      />
    </n-form-item>
    <n-row :gutter="[0, 24]">
      <n-col :span="24">
        <div style="display: flex; justify-content: flex-end">
          <n-button
            :disabled="modelRef.age === null"
            round
            type="primary"
            @click="handleValidateButtonClick"
          >
            验证
          </n-button>
        </div>
      </n-col>
    </n-row>
  </n-form>

  <pre>{{ JSON.stringify(modelRef, null, 2) }}</pre>
</template>
