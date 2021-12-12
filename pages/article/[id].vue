<template>
    <div class="border-3 border-green-500 border-dashed rounded-lg p-2">
        <div v-if="password">
            <div class="text-3xl text-green-800 dark:text-green-100">请输入密码：</div>
            <div class="flex-col my-4">
                <div class="flex justify-center">
                    <div class="border-3 border-dashed rounded-md w-md" :class="borderColor">
                        <div class="filter blur-2">
                            <input v-model="myPassword" class="w-full text-center dark:bg-dark dark:text-green-100" style="outline: none;" @keyup.enter="loadArticle" />
                        </div>
                    </div>
                    <div class="cursor-pointer flex justify-center bg-green-100 dark:bg-green-900 w-8 h-8 border-3 border-dashed rounded-md ml-4" :class="borderColor" @click="loadArticle">
                        <div class="w-auto text-xl align-middle text-green-500 dark:text-green-100 font-mono font-bold text-green-800">&gt;</div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else>
            <div v-if="contentType === 'html'">
                <iframe class="w-full" style="height: calc(100vh - 8.5rem)" :srcdoc="content" />
            </div>
            <div v-else-if="contentType === 'md'">
                <div class="dark:text-green-100" style="white-space: pre-wrap;" v-text="content" />
            </div>
            <div v-else-if="contentType === 'txt'">
                <div class="dark:text-green-100" style="white-space: pre-wrap;" v-text="content" />
            </div>
        </div>
    </div>
</template>

<script setup>
const isLoading = useLoadStatus()
const headMessage = useHeadMessage()
</script>

<script>
import axios from 'axios';

export default {
    name: '[id]',
    layout: 'home',
    data () {
        return {
            content: '',
            contentType: 'txt',
            settings: {},
            password: false,
            statusCode: 200,
            myPassword: '',
            borderColor: 'border-green-500'
        }
    },
    beforeMount() {
        this.loadArticle('')
    },
    methods: {
        loadArticle () {
            this.isLoading = true
            this.headMessage = ''

            const articleId = this.$route.params.id
            const password = this.myPassword

            const getContent = (c) => this.content = c
            const getSettings = (s) => this.settings = s
            const needPassword = () => {
                this.password = true
                if (this.myPassword !== '') {
                    this.borderColor = 'border-red-500'
                }
            }
            const noNeedPassword = () => this.password = false
            const getContentType = (t) => this.contentType = t

            const statusMonitor = [false, false]
            const stopLoading = (i) => {
                statusMonitor[i] = true
                if (statusMonitor[0] && statusMonitor[1]) {
                    this.isLoading = false
                }
            }

            axios.get('/api/article', {
                params: {
                    id: articleId,
                    password: password
                }
            }).then((response) => {
                if (!response.data.password) {
                    noNeedPassword()
                    getSettings(response.data.settings)
                    if (response.data.contentUrl === '') {
                        getContent('文件缺失，请等待同步')
                        stopLoading(1)
                        return
                    }
                    axios.get(response.data.contentUrl)
                    .then((contentResponse) => {
                        getContent(contentResponse.data)
                        getContentType(response.data.contentType)
                    })
                    .catch((contentError) => {
                        getContent('')
                    })
                    .finally(() => {
                        stopLoading(1)
                    })
                } else {
                    stopLoading(1)
                    needPassword()
                }
            }).catch((error) => {
                console.error(error.response.status)
            }).finally(() => {
                stopLoading(0)
            })
        }
    }
}
</script>
