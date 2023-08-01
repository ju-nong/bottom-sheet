<template>
    <button
        type="button"
        class="open-btn text-white text-[2rem] bg-[#44bd87] px-[30px] py-[6px] rounded-[4px]"
        @click="show = true"
    >
        Open
    </button>
    <BottomSheet :show="show" @close="show = false">
        <div class="flex pt-3 pb-9 items-center justify-center text-center">
            아ㅋㅋ 겁나 잘 된다~

            <br />
            <br />

            라이브러리 왜 씀?
        </div>
    </BottomSheet>
</template>

<script setup>
onMounted(() => {
    const $div = document.createElement("div");
    $div.classList.add("full-modal");
    window.parent.document.body.appendChild($div);

    setTimeout(() => {
        console.log("콘솔");
    }, 1000);
    setTimeout(() => {
        alert("알림");
    }, 2000);
});
</script>

<style lang="scss"></style>
