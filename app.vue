<template>
  <div>
    <NuxtPage />
  </div>
</template>

<script>
export default {
    setup () {
        const config = useRuntimeConfig()
        useMeta({
            meta: [
                { name: 'viewport', content: 'width=device-width, initial-scale=1, maximum-scale=1' }
            ],
            title: config.WEBSITE_NAME
        })
    }
}
</script>
