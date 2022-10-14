<template id="button-template">
    <main class="app">
        <section class="create-comment">
            <form id="new-comment-form" @submit.prevent="addComment">
                <textarea class="form-control" id="content" style='width: 400px; height: 150px; border-radius: 10px;'
                    v-model="input_content" placeholder="Your text here."></textarea>
                <br>
                <button style="background-color:#9B59B6" class="submit" @click="removeComment(comment)">Submit
                </button>
                <hr>
            </form>
        </section>
        <section class="comment-list">
            <h3>COMMENTS</h3>
            <div class="list" id="comment-list">
                <div v-for="comment in comments_asc.slice().reverse()"
                    :class="`comment-item ${comment.done && 'done'}`">
                    <hr>
                    <div class="comment-content">
                        <textarea type="text" style="width: 400px; height: 150px; border-radius: 10px;"
                            :disabled="disabled == 1" v-model="comment.content"> </textarea>
                    </div>
                    <div class="actions">
                        <div>
                            <div class="about">
                            </div>
                            <button @click="LikesUp">👍🏻 {{likes}} </button> ‏
                            <button @click="disabled = (disabled + 1) % 2 ">✍🏻</button>‏
                            <button @click="DislikesUp">👎🏻 {{dislikes}} </button> ‏
                            <button style="background-color:#9B59B6" class="delete"
                                @click="removeComment(comment)">🗑️</button>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const comments = ref([])
const input_content = ref('')
const comments_asc = computed(() => comments.value.sort((a, b) => {
    return a.createdAt - b.createdAt
}))

watch(comments, (newVal) => {
    localStorage.setItem('comments', JSON.stringify(newVal))
}, {
    deep: true
})

const addComment = () => {
    if (input_content.value.trim() === '') {
        return
    }

    comments.value.push({
        content: input_content.value,

    })
}

const removeComment = (comment) => {
    comments.value = comments.value.filter((t) => t !== comment)
}

const LikesUpdate =

    onMounted(() => {
        comments.value = JSON.parse(localStorage.getItem('comments')) || []
    })
</script>


<script>
el: '#app'
export default {
    data() {
        return {
            likes: 0,
            dislikes: 0,
            disabled: 1
        }
    },

    methods: {
        LikesUp() {
            this.likes += 1;
        },
        DislikesUp() {
            this.dislikes += 1;
        }
    }
}
</script>






