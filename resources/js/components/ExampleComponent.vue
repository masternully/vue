<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Comments</div>
                    <div class="card-body">
                        <ul class="list-group">
                            <li class="list-group-item" v-for="(item, index) in comments" :key="index">
                                <p>Author: {{item.author}}</p>
                                <p>Comment: {{item.body}}</p>
                                <button @click="deleteComment(item, index)">Delete</button>
                            </li>
                        </ul>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="add">
                            <input class="form-control mb-2" type="text" placeholder="Author" v-model="comment.author">
                            <textarea class="form-control mb-2" placeholder="Comment" v-model="comment.body" cols="55" rows="10"></textarea>
                            <button>Send</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                comments: [],
                comment: {author: '', body: ''}
            }
        },
        created(){
            axios.get('/vue/public/comments')
                .then(res => {
                    this.comments = res.data;
                })
        },
        methods: {
            add(){
                const params = {
                    author: this.comment.author,
                    body: this.comment.body
                }

                axios.post('/vue/public/comments', params)
                    .then(res => {
                        this.comments.push(res.data);
                    })
            },
            deleteComment(item, index){
                axios.delete(`/vue/public/comments/${item.id}`)
                    .then(()=>{
                        this.comments.splice(index, 1);
                    })
            }
        }
    }
</script>
