<template>
    <Head>
        <Title>Bottom Sheet</Title>
        <Meta name="theme-color" content="#e4fff3" />
        <Meta
            name="description"
            content="이상한 botom-sheet 라이브러리 쓰다가, 그냥 내가 만드는게 낫겠다 싶어서 만든 프로젝트"
        />
        <Meta name="author" content="이준용" />
        <Meta
            name="keywords"
            content="이준용, bottom, sheet, component, vue, nuxt"
        />
        <Meta property="og:type" content="website" />
        <Meta property="og:title" content="Bottom Sheet" />
        <Meta
            property="og:description"
            content="이상한 botom-sheet 라이브러리 쓰다가, 그냥 내가 만드는게 낫겠다 싶어서 만든 프로젝트"
        />
        <Meta property="og:locale" content="ko_KR" />
        <Meta
            property="og:url"
            content="https://open-bottom-sheet.vercel.app/"
        />
        <Meta property="og:image" content="/thumbnail.png" />
        <Meta property="og:site_name" content="생일선물 뽑기" />

        <Meta name="twitter:card" content="summary_large_image" />
        <Meta name="twitter:title" content="Bottom Sheet" />
        <Meta
            name="twitter:description"
            content="이상한 botom-sheet 라이브러리 쓰다가, 그냥 내가 만드는게 낫겠다 싶어서 만든 프로젝트"
        />
        <Meta name="twitter:image" content="/thumbnail.png" />

        <Link rel="shortcut icon" href="/favicon.png" type="image/x-icon" />
    </Head>
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
