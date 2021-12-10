<template>
    <div>
        <header class="fixed z-10 top-0 left-0 h-18 w-full backdrop-filter backdrop-blur bg-white bg-opacity-40">
            <div class="flex h-18 items-center">
                <div class="mx-4 cursor-pointer text-2xl font-mono font-bold text-green-500" @click="goBack">&lt;</div>
                <div class="mx-4 text-xl font-mono font-medium">OneDrive/Blog</div>
                <div class="mx-4 text-xl font-mono">{{ headMessage }}</div>
                <div class="flex justify-end w-full">
                    <div class="mx-4 text-xl font-mono font-medium text-green-600">{{ config.WEBSITE_NAME }}</div>
                </div>
            </div>
        </header>
        <div class="h-18 w-full"></div>
        <div v-if="isLoading || errorMessage !== ''">
            <div class="fixed h-full w-full z-20 flex justify-center items-center backdrop-filter backdrop-blur">
                <div class="mb-48">
                    <img src="~/assets/onedrive.png" alt="logo" class="filter -hue-rotate-45 animate-pulse">
                </div>
            </div>
        </div>
        <div class="container mx-auto my-4 z-0">
            <slot />
        </div>
    </div>
</template>

<script setup>
const config = useRuntimeConfig()
const isLoading = useLoadStatus()
const headMessage = useHeadMessage()
const errorMessage = useErrorMessage()
</script>

<script>
export default {
    name: 'home',
    data () {
        return {
            backBtnClass: ''
        }
    },
    methods: {
        goBack () {
            this.$router.back()
        }
    }
}
</script>

<style scoped>

</style>
