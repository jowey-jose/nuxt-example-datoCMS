<template>
    <div>
        <!-- makes sure the page does not try and load unless the post exsists, and if it does, it shows the title and also do the same with the body. -->
        <h1 v-if="post"> {{ post.title }} </h1>
        <h1 v-if="post"> {{ post.body }} </h1>
    </div>
</template>

<script>

import gql from 'graphql-tag'

export default {
    // Start Writing query.
    apollo: {
        post: {
            //Creating the graphQl query, where we are going to do the gql query.
            //Creating new variable called slug, defined in the variables section, its a string,and inside it we are creating a post filter.
            //What we then do inside the post filter, we write a query that reurn the slug route, and return the title. 
            query:gql`query Post($slug: String!){
                post(filter: { slug: {
                    matches: { pattern: $slug }

                }}) {
                    title
                    body
                }
            }`,
            //Makes sure everytime before the route loads we can grab the slug as well. 
            prefetch({route}) {
                return {
                    slug: route.params.slug
                }
            },

            variables() {
                return {
                    // Creating a new variable called Slug.
                    // its gonna retrieve the value from the path and stored it as a variable into the slug variable.
                    slug: this.$route.params.slug
                }
            }
        }
    }
    
}
</script>