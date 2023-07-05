<template>
    <button
        type="button"
        class="open-btn text-white text-[2rem] bg-[#44bd87] px-[30px] py-[6px] rounded-[4px]"
        @click="show = true"
    >
        Open
    </button>
    <BottomSheet :show="show" @close="show = false" />
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
