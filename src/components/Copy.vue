<script setup>
import { Copy, Loading, CheckOne } from "@icon-park/vue-next";
import { Theme } from "@icon-park/vue-next/lib/runtime";
import { ref } from "vue";

const porps = defineProps({ content: String});
const btnConfig = {
  size: 14,

  theme: "outline",
};
const btnTips = {
  copy: "复制全文",
  loading: "",
  success: "已复制到剪贴板！",
  error: "复制失败！",
};
const btnStatus = ref("copy");

const copyToClipboard = (content = porps.content) => {
  btnStatus.value = "loading";
  navigator.clipboard
    .writeText(content)
    .then(() => setTimeout(() => (btnStatus.value = "success"), 150))
    .catch(() => (btnStatus.value = "error"))
    .finally(() => setTimeout(() => (btnStatus.value = "copy"), 1500));
};
</script>

<template>
  <div class="flex items-center cursor-pointer" @click="copyToClipboard()">
    <copy
      v-show="btnStatus === 'copy'"
      :theme="btnConfig.theme"
      :size="btnConfig.size"
      :fill="btnConfig.fill"
    />
    <loading
      class="rotate"
      v-show="btnStatus === 'loading'"
      :theme="btnConfig.theme"
      :size="btnConfig.size"
      :fill="btnConfig.fill"
    />
    <check-one
      v-show="btnStatus === 'success'"
      :theme="btnConfig.theme"
      :size="btnConfig.size"
      :fill="btnConfig.fill"
    />
    <close-one
      v-show="btnStatus === 'error'"
      :theme="btnConfig.theme"
      :size="btnConfig.size"
      :fill="btnConfig.fill"
    />
    <span class="text-xs ml-0.5 leading-none">{{
      btnTips[btnStatus]
    }}</span>
  </div>
</template>

<style scoped>
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.rotate {
  animation: spin 2s linear infinite;
}
</style>
