<markdown>
# Debug
</markdown>

<script lang="ts" setup>
import { ArchiveOutline as ArchiveIcon } from '@vicons/ionicons5'
import { ref } from 'vue'

function handleUpdateList(...args: unknown[]) {
  console.log(...args)
}

const multiple = ref(false)
const directory = ref(false)
const directoryDnd = ref(false)
const directoryDndUndefined = ref(true)
</script>

<template>
  Retry
  <n-upload action="http://fake-api">
    <n-button>Upload</n-button>
  </n-upload>
  Multiple <n-switch v-model:value="multiple" /><br>
  Directory <n-switch v-model:value="directory" /><br>
  DirectoryDnd <n-switch v-model:value="directoryDnd" /><br>
  DirectoryDnd undefined <n-switch v-model:value="directoryDndUndefined" />
  <n-upload
    action="__HTTP__://www.mocky.io/v2/5e4bafc63100007100d8b70f"
    :multiple="multiple"
    :directory="directory"
    :directory-dnd="directoryDndUndefined ? undefined : directoryDnd"
    @update:file-list="handleUpdateList"
  >
    <n-button>Upload</n-button>
  </n-upload>
  <n-upload
    action="__HTTP__://www.mocky.io/v2/5e4bafc63100007100d8b70f"
    :multiple="multiple"
    :directory="directory"
    :directory-dnd="directoryDndUndefined ? undefined : directoryDnd"
    @update:file-list="handleUpdateList"
  >
    <n-upload-dragger>
      <div style="margin-bottom: 12px">
        <n-icon size="48" :depth="3">
          <ArchiveIcon />
        </n-icon>
      </div>
      <n-text style="font-size: 16px">
        点击或者拖动文件到该区域来上传
      </n-text>
      <n-p depth="3" style="margin: 8px 0 0 0">
        请不要上传敏感数据，比如你的银行卡号和密码，信用卡号有效期和安全码
      </n-p>
    </n-upload-dragger>
  </n-upload>
</template>
