<template>
    <div class="create">
        <h1>Create Book</h1>
        <form action="#" method="POST" @submit.prevent="addBooks">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" name="title" id="title" v-model="title">
            </div>
            <div class="form-group">
                <label for="author">Author</label>
                <input type="text" name="author" id="author" v-model="author">
            </div>
            <div class="form-group">
                <label for="image">Image</label>
                <input type="text" name="image" id="image" v-model="image">
            </div>
            <div class="form-group">
                <label for="description">Description</label>
                <textarea name="description" id="description" cols="30" rows="10" v-model="description"></textarea>
            </div>
            <div class="form-group">
                <label for="link">Link</label>
                <input type="text" name="link" id="link" v-model="link">
            </div>
            <div class="form-group">
                <label for="featured">Featured</label>
                <input type="checkbox" name="featured" id="featured" v-model="featured">
            </div>
            <div class="form-group">
                <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
                    <template slot-scope="{ result: { data, loading }, isLoading }">
                        <div v-if="isLoading">Loading...</div>
                        <select v-else  v-model="category">
                            <option v-for="category of data.categories" :key="category.id" 
                            @click.prevent="selectCategory(category.id)" class="link-margin" :value="category.id">
                                {{ category.name }}
                            </option>
                        </select>
                    </template>
                </ApolloQuery>
            </div>

            
            <div class="fomr-group">
                <button type="submit">Add Book</button>
            </div>
        </form>
    </div>
</template>

<script>
import addBook from '@/graphql/mutations/AddBooks.gql'

export default {

    data(){
        return {
            title: '',
            author:'',
            image: '',
            description: '',
            link: '',
            featured: false,
            category: 1,
        }
    },
    methods: {
        addBooks(){
            this.$apollo.mutate({
                mutation: addBook,
                variables: {
                    title: this.title,
                    author: this.author,
                    image: this.image,
                    description: this.description,
                    link: this.link,
                    featured: this.featured,
                    category: this.category
                }
            }).then((data) => {
                console.log(data)
                this.$router.push('/')
            }).catch((error) => {
                console.error(error)

            })
        }
    }
}
</script>


<style scoped>
.form-group{
    margin-bottom: 32px;
}
input[type="text"]{
    padding: 10px 14px;
}
button{
    padding: 16px;
    background: #027BFF;
    color: white;
    border-radius: 5px;
    font-size: 16px;
}
</style>
