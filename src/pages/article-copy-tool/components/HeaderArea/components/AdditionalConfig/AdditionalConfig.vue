<script setup lang="ts">
import {useStore} from "../../../../../../store/useStore";
import ConfigSwitchButton from "./components/ConfigSwitchButton.vue";
import useClipboard from "vue-clipboard3";
import {onBeforeUnmount, onMounted, watch} from "vue";

const store = useStore();
const {toClipboard} = useClipboard();

const THRESHOLD_WIDTH = 600;
const hasNotEnoughWidth = document.body.clientWidth < THRESHOLD_WIDTH;

let copyTimer: ReturnType<typeof setTimeout> | undefined;
let lastCopied = "";

function copyOutputText() {
  const text = store.copy.outputText;
  if (!store.storage.copy.copyOutputWhenLeave || !text || text === lastCopied) return;
  if (typeof document !== "undefined" && !document.hasFocus()) return;

  clearTimeout(copyTimer);
  copyTimer = setTimeout(() => {
    const latest = store.copy.outputText;
    if (!store.storage.copy.copyOutputWhenLeave || !latest || latest === lastCopied) return;
    if (typeof document !== "undefined" && !document.hasFocus()) return;
    toClipboard(latest)
      .then(() => {
        lastCopied = latest;
      })
      .catch(() => {
        // 页面失焦或浏览器拦截时静默失败
      });
  }, 80);
}

function onLeavePage() {
  if (store.storage.copy.clearInputWhenLeave) store.copy.inputText = "";
}

onMounted(() => {
  window.addEventListener("blur", onLeavePage);

  if (hasNotEnoughWidth) {
    store.storage.copy.horizontalLayout = false;
  }
});

onBeforeUnmount(() => {
  window.removeEventListener("blur", onLeavePage);
  clearTimeout(copyTimer);
});

watch(
  () => [store.copy.outputText, store.storage.copy.copyOutputWhenLeave] as const,
  () => {
    copyOutputText();
  },
);
</script>

<template>
  <n-space justify="center" align="center" wrap="wrap">
    <n-space>
      <ConfigSwitchButton class="clear-input-when-leave-switch" label="自动清空输入" v-model:status="store.storage.copy.clearInputWhenLeave"/>
      <ConfigSwitchButton class="copy-output-when-leave-switch" label="自动复制输出" v-model:status="store.storage.copy.copyOutputWhenLeave"/>
    </n-space>

    <n-space>
      <ConfigSwitchButton class="auto-output-switch" label="实时输出" v-model:status="store.storage.copy.autoOutput"/>
      <ConfigSwitchButton class="horizontal-layout-switch" label="布局方式" v-model:status="store.storage.copy.horizontalLayout"
                          true-label="水平" false-label="竖直" :disabled="hasNotEnoughWidth"/>
    </n-space>
  </n-space>
</template>

<style scoped>

</style>
