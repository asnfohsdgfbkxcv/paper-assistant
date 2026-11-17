<script setup lang="ts">
import {useStore} from "../../../../../../store/useStore";
import ConfigSwitchButton from "./components/ConfigSwitchButton.vue";
import {onBeforeUnmount, onMounted, watch} from "vue";

const store = useStore();

const THRESHOLD_WIDTH = 600;
const hasNotEnoughWidth = document.body.clientWidth < THRESHOLD_WIDTH;

let lastCopied = "";

function writeByExecCommand(text: string): boolean {
  const active = document.activeElement as HTMLElement | null;
  const ta = document.createElement("textarea");
  ta.value = text;
  ta.setAttribute("readonly", "");
  ta.style.position = "fixed";
  ta.style.top = "0";
  ta.style.left = "0";
  ta.style.width = "1px";
  ta.style.height = "1px";
  ta.style.opacity = "0";
  ta.style.pointerEvents = "none";
  document.body.appendChild(ta);
  ta.focus({preventScroll: true});
  ta.select();
  ta.setSelectionRange(0, ta.value.length);
  let ok = false;
  try {
    ok = document.execCommand("copy");
  } catch {
    ok = false;
  }
  document.body.removeChild(ta);
  if (active && typeof active.focus === "function") {
    try {
      active.focus({preventScroll: true});
    } catch {
      // ignore
    }
  }
  return ok;
}

function copyOutputText(force = false) {
  if (!store.storage.copy.copyOutputWhenLeave) return;
  const text = store.copy.outputText;
  if (!text || (!force && text === lastCopied)) return;

  // 先同步写入，尽量赶在页面失焦或用户手势失效之前
  if (writeByExecCommand(text)) {
    lastCopied = text;
    return;
  }
  if (navigator.clipboard && window.isSecureContext) {
    navigator.clipboard.writeText(text)
      .then(() => {
        lastCopied = text;
      })
      .catch(() => {
        // 页面失焦或浏览器拦截时静默失败
      });
  }
}

function onLeavePage() {
  copyOutputText(true);
  if (store.storage.copy.clearInputWhenLeave) store.copy.inputText = "";
}

function onVisibilityChange() {
  if (document.visibilityState === "hidden") copyOutputText(true);
}

function onMouseLeavePage() {
  copyOutputText(true);
}

onMounted(() => {
  window.addEventListener("blur", onLeavePage);
  document.addEventListener("visibilitychange", onVisibilityChange);
  document.documentElement.addEventListener("mouseleave", onMouseLeavePage);

  if (hasNotEnoughWidth) {
    store.storage.copy.horizontalLayout = false;
  }
});

onBeforeUnmount(() => {
  window.removeEventListener("blur", onLeavePage);
  document.removeEventListener("visibilitychange", onVisibilityChange);
  document.documentElement.removeEventListener("mouseleave", onMouseLeavePage);
});

watch(
  () => [store.copy.outputText, store.storage.copy.copyOutputWhenLeave] as const,
  () => {
    copyOutputText();
  },
  {flush: "sync", immediate: true},
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
