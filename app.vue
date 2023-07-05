<template>
    <button
        type="button"
        class="open-btn text-white text-[2rem] bg-[#44bd87] px-[30px] py-[6px] rounded-[4px]"
        @click="show = true"
    >
        Open
    </button>
    <BottomSheet :show="show" @close="show = false">
        <div class="flex pt-3 pb-9 justify-center gap-x-[104px]">
            <div class="flex flex-col items-center cursor-pointer">
                <button type="button">
                    <img src="/images/icon-camera.svg" alt="" />
                </button>
                <span class="text-sm text-gray-500 mt-2">카메라</span>
            </div>
            <div class="flex flex-col items-center cursor-pointer">
                <button type="button">
                    <img src="/images/icon-gallery.svg" alt="" />
                </button>
                <span class="text-sm text-gray-500 mt-2">갤러리</span>
            </div>
        </div>
    </BottomSheet>
</template>

<script setup>
const show = ref(false);
</script>

<style lang="scss">
.open-btn {
    border-bottom: 2px solid #059669;
    text-shadow: 1px 1px 1px #059669;

    &:hover {
        background-color: #059669;
    }

    &:active {
        border-bottom: 0;
        border-top: 2px solid #059669;
    }
}
</style>
