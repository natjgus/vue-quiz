<template>
    <div>
        <b-dropdown id="dropdown-1" text="Quiz Topics" class="m-md-2">
            <b-dropdown-item>{{topicList}}</b-dropdown-item>
        </b-dropdown>
    </div>
</template>


<script>
export default {
    props: {
        topicList: Object
    }   
}
</script>
