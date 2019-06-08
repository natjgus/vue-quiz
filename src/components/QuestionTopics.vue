<template>
    <div>
        <h3>Choose a quiz topic below!</h3>
        <b-dropdown id="dropdown-1" text="Quiz Topics" class="m-md-2">
            <b-dropdown-item
            v-for="topic in topicList"
            :key="topic.id"
            v-on:click.prevent="setTopic(topic.id)"
            >
                {{ topic.name }}
            </b-dropdown-item>
        </b-dropdown>
    </div>
</template>


<script>

export default {
    name: 'questionTopics',
    data() {
        return{
            topicList: [],
            selectedId: null,
        }
    },
    mounted : function(){
    fetch('https://opentdb.com/api_category.php', {
      method: 'get'
    })
    .then( response => {
        return response.json().then( data => 
        this.topicList = data.trivia_categories)
    })
    },
    methods: {
        setTopic(id){
            this.selectedId = id;
            this.$emit('setTopic', this.selectedId)
        }
    }   
}
</script>
