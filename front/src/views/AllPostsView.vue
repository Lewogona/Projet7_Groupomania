<template>
    <div>
        <div class="title">
            <h1>
                Tous les posts
            </h1>
            <h2>
                Du plus récent au plus vieux...
            </h2>
        </div>
        <PublishedPost 
            commentButton="Voir le fil"
            v-for="post in posts"
            :email="post.User && post.User.email"
            :firstName="post.User && post.User.firstName"
            :lastName="post.User && post.User.lastName"
            :date="post.date" 
            :title="post.title" 
            :content="post.content" 
            :likes="post.likes"
            :id="post.id"
            :key="post.id"
            :deletePost="deletePost"
            :postUserId="post.User && post.User.id"
            :imageUrl="post.imageUrl"
            :profileImageUrl="post.User && post.User.imageUrl"/>
    </div>
</template>

<script>
import axios from "../services/axios-service"
import {getReadableDate} from "../services/date-service"

import PublishedPost from "@/components/PublishedPost.vue"

export default {
    name: "AllPostsView",
    components: {
        PublishedPost
    },
    data() {
        return {
            posts: []
        }
    },
    created() {
        // Get all posts and format the date
        axios.get("posts")
            .then(response => {
                this.posts.push(...response.data.map(post => {
                    post.date = getReadableDate(post.date);
                    return post
                }));
            }).catch(e => {
                console.error(e);
            });
    },
    methods: {
        // Delete the post using its id
        async deletePost(id) {
            await axios.delete("posts/" + id);
            this.posts = this.posts.filter((post) => {
                return post.id !== id;
            })
        }
    }
}
</script>

<style scoped lang="scss">
h1 {
    font-size: 28px;
    margin-top: 15px;
    font-weight: bold;
    text-align: center;
}

h2 {
    font-size: 20px;
    text-align: center;
}

.title {
    width: 50%;
    margin: auto;
}
</style>

