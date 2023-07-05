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
                class="bottom-sheet-main z-[300] bg-white absolute w-full max-w-[400px]"
                :style="`--bottom-sheet-height: -${
                    $sheet?.offsetHeight ?? 0
                }px; --bottom-sheet-interval: ${movePoint * -1}px;`"
                ref="$sheet"
            >
                <div
                    class="bottom-sheet-main-header h-[40px] flex items-center cursor-pointer"
                    ref="$header"
                >
                    <div
                        v-if="props.hasHeader"
                        class="bottom-sheet-main-header-icon mx-auto"
                    ></div>
                </div>
                <div>
                    <slot> </slot>
                </div>
                <div class="min-h-[100vh]"></div>
            </div>
        </div>
    </Teleport>
</template>

<script setup>
import { useScrollLock, useEventListener } from "@vueuse/core";
const props = defineProps({
    show: {
        type: Boolean,
        default: false,
    },
    hasHeader: {
        type: Boolean,
        default: false,
    },
});

const emits = defineEmits(["close"]);

const isLock = ref(null);

const $container = ref();
const $sheet = ref();

const $header = ref();

const startPoint = ref(0);
const movePoint = ref(0);
const isStart = ref(false);
const perFrame = ref(0);

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
}

function handleEnd(event) {
    isStart.value = false;

    startPoint.value = 0;

    if (movePoint.value > 99) {
        emits("close");
        setTimeout(() => {
            movePoint.value = 0;
        }, 300);
    } else {
        perFrame.value = movePoint.value / 16;
        animateElement();
    }
}

useEventListener($header, "touchstart", handleStart);
useEventListener($header, "touchmove", handleMove);
useEventListener($header, "touchend", handleEnd);
useEventListener($header, "mousedown", handleStart);
useEventListener($container, "mousemove", handleMove);
useEventListener($container, "mouseup", handleEnd);
useEventListener($container, "mouseleave", handleEnd);

function handleClose() {
    isStart.value = false;
    emits("close");
}

function animateElement() {
    const per = perFrame.value;
    const move = movePoint.value;

    if (move !== 0) {
        movePoint.value = move - per;

        requestAnimationFrame(animateElement);
    }
}

watch(props, (to, from) => {
    if (isLock.value === null) {
        return;
    }

    isLock.value.value = to.show;
});

onMounted(() => {
    isLock.value = useScrollLock(document.body);
});
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

        &-header {
            &-icon {
                width: 30%;
                height: 8px;
                background-color: rgb(88, 88, 88);
                border-radius: 10px;
            }
        }
    }
}
</style>
