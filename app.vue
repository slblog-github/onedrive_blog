<template>
  <div>
    <NuxtPage />
  </div>
</template>

<script>
export default {
    setup () {
        useMeta({
            meta: [
                { name: 'viewport', content: 'width=device-width, initial-scale=1, maximum-scale=1' }
            ],
        })
    }
}
</script>
