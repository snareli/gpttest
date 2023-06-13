<script setup lang='ts'>
import { computed, ref } from 'vue'
import { NModal, NButton } from 'naive-ui'


interface DataProps {
  renderKey: string
  renderValue: string
  key: string
  value: string
}

interface Props {
  visible: boolean
}

interface Emit {
  (e: 'update:visible', visible: boolean): void
}

const props = defineProps<Props>()

const emit = defineEmits<Emit>()


const show = computed({
  get: () => props.visible,
  set: (visible: boolean) => emit('update:visible', visible),
})

const showModal = ref(false)

</script>

<template>
  <NModal v-model:show="show" style="width: 100%; max-width: 900px;" preset="card">
    <div class="space-y-4">
      <p class="text-xl to-blue-800">此网站为个人测试使用，不保证稳定性！</p>
      <h2 class="text-xl font-bold">初次使用</h2>
      <p class="font-bold">如果你认识作者</p>
      <p>使用QQ邮箱注册后，微信QQ私聊我，等待我审核，通过后即可登录</p>
      <p class="font-bold">如果你不认识作者</p>
      <p>加入QQ群813665832，注明你的<strong>注册邮箱和了解渠道（例如朋友推荐）</strong>,等待审核</p>
      <h2 class="font-bold">作者联系方式：</h2>
      <p>QQ：1320616682</p>
      <NButton type="primary" @click="showModal = true">打赏作者</NButton>
    </div>
  </NModal>

  <n-modal v-model:show="showModal" preset="card" style="width: 100%; max-width: 600px;" title="打赏作者">
    <template #header-extra>
      关闭
    </template>
    <div class="text-center">
      <h2 class="text-sm">至今平原客，感激慕清风</h2>
      <div>
        <img src="@/../public/zanshang.jpg" alt="" width="300" height="300">
      </div>

    </div>

  </n-modal>
</template>
