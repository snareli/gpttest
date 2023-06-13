<script setup lang='ts'>
import { computed, onMounted, ref } from 'vue'
import { NAvatar, NButton, NModal } from 'naive-ui'
import { useRoute } from 'vue-router'
import { useAuthStore, useUserStore } from '@/store'
import defaultAvatar from '@/assets/avatar.jpg'
import { isString } from '@/utils/is'
import Permission from '@/views/chat/layout/Permission.vue'
import { useBasicLayout } from '@/hooks/useBasicLayout'

const route = useRoute()
const userStore = useUserStore()
const authStore = useAuthStore()
const { isMobile } = useBasicLayout()
const showPermission = ref(false)

const needPermission = computed(() => !!authStore.session?.auth && !authStore.token && (isMobile.value || showPermission.value))

const userInfo = computed(() => userStore.userInfo)

const showPopUp = ref(false)

onMounted(async () => {
  const sign = route.query.verifyresetpassword as string
  if (sign)
    showPermission.value = true

  if(!!authStore.session?.auth && !authStore.token){
    showPopUp.value = true
  }
})
</script>

<template>
  <div class="flex items-center overflow-hidden">
    <div class="w-10 h-10 overflow-hidden rounded-full shrink-0">
      <template v-if="isString(userInfo.avatar) && userInfo.avatar.length > 0">
        <NAvatar size="large" round :src="userInfo.avatar" :fallback-src="defaultAvatar" />
      </template>
      <template v-else>
        <NAvatar size="large" round :src="defaultAvatar" />
      </template>
    </div>
    <div class="flex-1 min-w-0 ml-2">
      <h2 v-if="userInfo.name" class="overflow-hidden font-bold text-md text-ellipsis whitespace-nowrap">
        {{ userInfo.name }}
      </h2>
      <NButton v-else tag="a" text @click="showPermission = true">
        <span v-if="!!authStore.session?.auth && !authStore.token" class="text-xl text-[#ff69b4] dark:text-white">
          {{ $t('common.notLoggedIn') }}
        </span>
        <span v-else class="text-xl text-[#ff69b4] dark:text-white">
          {{ authStore.session?.title }}
        </span>
      </NButton>
    </div>
    <Permission :visible="needPermission" />
  </div>

  <!-- 弹窗 -->
  <NModal v-model:show="showPopUp" style="width: 100%; max-width: 600px;" preset="card">
    <div class="space-y-4">
      <p class="text-xl to-blue-800">此网站为个人测试使用，不保证稳定性！</p>
      <h2 class="text-xl font-bold">初次使用</h2>
      <p class="font-bold">如果你认识作者</p>
      <p>使用QQ邮箱注册后，微信QQ私聊我，等待我审核，通过后即可登录</p>
      <p class="font-bold">如果你不认识作者</p>
      <p>加入QQ群813665832，注明你的<strong>注册邮箱和了解渠道（例如朋友推荐）</strong>,等待审核</p>
      <h2 class="font-bold">作者联系方式：</h2>
      <p>QQ：1320616682</p>
    </div>
  </NModal>
</template>
