---
title: Vue foundation and Create model
---

## Vue layout
v-flex(sub) -> v-layout -> v-container (super)
- v-container : container which contains content
    -> applied to centeral page
- v-layout : used for seperating each section.
    -> define layout including v-flex which is single content
- v-flex : tool for representing layout

## Bi-directional data binding (v-model)
v-model attribute operates with combination fo v-bind and v-on
v-bind : binding HTML components and data attribute of view instance
v-on : binding HTML components with logic of instance (method included)
For instance.

```
<input v-bind:value="inputText" v-on:input="updateInput">
new Vue({
    data: {
        inputText: ''
    },
    methods: {
        updateInput: function(event){
            var updatedText = event.target.value;
            this.inputText = updateText;
        }
    }
})
```
```
<input v-model='inputText'>
new Vue({
    data: {
        inputText: ''
    }
})
```

```vue
//content.vue
<script>
import axios from "axios";
let url = "http://localhost:8000";

export default {
    data: () => {
        return {
            data: {
                username: "",
                age: "",
                city: "",
            },
        };
    },

    props: ["propsdata"],

    methods: {
        sendForm: function(){
            axios({
                method: "POST",
                url: url,
                data: this.data,
            })
            .then((response) => {
                this.userList = response.data;
            })
            .catch((error) => {
                console.log("Failed to get userList", error.reponse);
            });
        },
        clearForm: function() {
            (this.data.username=""), (this.data.age=""), (this.data.city= "");
        },
    },
};
</script>
```

## Change mounted() setting

```vue
mounted(){
    axios({     // get userList from DRF server and store it
        method: "GET",
        url: url
    })
    .then(response => {
        this.userList=response.data;
        console.log("Success", response);
    })
    .catch(error => {
        console.log("Failed to get userList", error.response);
    });
},
```
