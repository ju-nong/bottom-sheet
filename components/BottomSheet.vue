<template>
    <Teleport to="body">
        <div
            class="bottom-sheet w-full h-full fixed z-[100] overflow-hidden flex items-end justify-center left-0 top-0 invisible"
            :class="props.show ? 'show' : ''"
            :style="`--bottom-sheet-transition: ${
                props.show ? 'visibility 0s' : 'visibility 0s 0.3s'
            };`"
            ref="$container"
        >
            <div
                class="bottom-sheet-back z-[200] bg-black/50 w-full h-full absolute left-0 top-0"
                @click="handleClose"
            ></div>
            <div
                class="bottom-sheet-main z-[300] bg-white absolute w-full"
                :style="`--bottom-sheet-height: -${
                    $sheet?.offsetHeight ?? 0
                }px; --bottom-sheet-interval: ${movePoint * -1}px;`"
                ref="$sheet"
            >
                <div
                    class="bottom-sheet-main-header h-[40px]"
                    ref="$header"
                ></div>
                <div class="h-[30vh]"></div>
                <div class="min-h-[100vh]"></div>
            </div>
        </div>
    </Teleport>
</template>

<script setup>
import { useEventListener } from "@vueuse/core";
const props = defineProps({
    show: {
        type: Boolean,
        default: false,
    },
});

const emits = defineEmits(["close"]);

const $container = ref();
const $sheet = ref();

const $header = ref();

const startPoint = ref(0);
const movePoint = ref(0);
const isStart = ref(false);

function handleStart(event) {
    event = event.touches ? event.touches[0] : event;

    startPoint.value = event.clientY;
    isStart.value = true;
}

function handleMove(event) {
    if (!isStart.value) {
        return;
    }

    event = event.touches ? event.touches[0] : event;

    movePoint.value = event.clientY - startPoint.value;
    console.log(movePoint.value);
}

function handleEnd(event) {
    isStart.value = false;
    startPoint.value = 0;
    movePoint.value = 0;
}

useEventListener($header, "touchstart", handleStart);
useEventListener($header, "touchmove", handleMove);
useEventListener($header, "touchend", handleEnd);
useEventListener($header, "mousedown", handleStart);
useEventListener($container, "mousemove", handleMove);
useEventListener($container, "mouseup", handleEnd);

function handleClose() {
    isStart.value = false;
    emits("close");
}
</script>

<style lang="scss">
.bottom-sheet {
    transition: var(--bottom-sheet-transition);

    &.show {
        visibility: visible;

        .bottom-sheet-back {
            opacity: 1;
        }

        .bottom-sheet-main {
            margin-bottom: -100vh;
        }
    }

    &-back {
        transition: all 0.3s;
        opacity: 0;
    }

    &-main {
        transition: margin 0.3s;
        border-top-left-radius: 20px;
        border-top-right-radius: 20px;
        box-shadow: 0 -6px 6px -4px rgba(0, 0, 0, 0.259);
        margin-bottom: var(--bottom-sheet-height);
        bottom: var(--bottom-sheet-interval);
    }
}
</style>
