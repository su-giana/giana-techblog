---
title: Vue.js for frontend
---

## Installation
your computer must have node.js before you try to install vue.js

```shell
$ sudo npm install -g @vue.cli
```

## Project
Create a project with below command
```shell
$ vue create {project name}
> default {select option}
```
> MAKE IT WITH VUE 2
> if there was an error about permission of your computer, run "sudo chown -R 501:20 "npm path" 

Run Vue server with below command
```shell
$ cd {project name}
$ sudo npm run serve
```

## Vuetify
### UI Componenet Framework
 : Framework that provide commonly used UI unit of component such as button, form, dialog

```shell
 $ vue add vuetify
> Default option
```

### make vue
Let's make Header.vue for web page header, Content.vue for web page's main content, Footer.vue for web page footer
> If you use VS code, I recommand Vetur extension

```vue
// Header.vue
<template>
	<header>
		<h1>User List</h1>
	</header>
</template>

<script>
	export default {
		name : 'HeaderVue'
	};
</script>

<style></style>

```

```vue
// content.vue
<template>
    <div>
        <v-container fluid>
            <v-layout column>
                <v-flex xs12>
                    <h3 class="subject">User CURD</h3>
                </v-flex>
                <v-col cols="4" md="4">
                    <v-text-field v-model="username" :counter="15" label="Username" required></v-text-field>
                </v-col>
                <v-col cols="4" md="4">
                    <v-text-field v-model="age" label="Age" required></v-text-field>
                </v-col>
                 <v-col cols="4" md="4">
                    <v-text-field v-model="city" :counter="15" label="City" required></v-text-field>
                </v-col>
                <v-flex column>
                    <v-form ref="form" v-model="model" lazy-validation>
                        <v-text-field v-model="title" :counter="64" label="Title" required></v-text-field>
                        <v-text-field v-model="content" :counter="255" label="Content" required></v-text-field>

                        <v-btn @click="create" style="background: green">create</v-btn>
                        <v-btn @click="clear" style="background: red">clear</v-btn>
                    </v-form>
                </v-flex>

                <v-flex class="userList" column>
                    <v-card max-width="600" tile>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title>Title</v-list-item-title>
                                <v-list-item-subtitle>content</v-list-item-subtitle>
                            </v-list-item-content>
                        </v-list-item>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-container>
    </div>
</template>

<script>
export default {}
</script>

<style>
    .subject {
        color:blue;
        font-style:oblique;
        padding:30px;
        text-align: center;
    }

    .userList{
        margin: 30px 0px 30px 0px
    }
</style>
</style>
```

```vue
<template>
    <footer>leffept @ copyright</footer>
</template>

<script>
export default {
    
}
</script>

<style>

</style>
```

### axios
> a promise-based HTTP library that lets developers make requests to either their own or a third-party server to fetch data
#### Installation

```shell
$ npm install axios
```

#### Configuration
In vue project directory/main.js, import axios and specify server address of django. 

```js
import axios from "axios";

let url = "http://localhost:8000/";

axios.get(url).then(function(response){
    console.log(response);   // test connection
})
.catch(function(response){
    console.log(respones);
})
```

### Cors
a promise-based HTTP library that lets developers make requests to either their own or a third-party server to fetch data
#### Django Cors Header

```shell
pip install django-cors-headers
```

#### Change settings.py

```python
INSTALLED_APPS = [
    'corsheaders',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
]
CORS_ORIGIN_ALLOW_ALL = False
CORS_ORIGIN_WHITELIST = (
    'http://localhost:8080',
    'http://127.0.0.1:8080',
)
```

<hr>

## CRUD with Django + View
We will get data with "mounted" when we load a page. We will get userList form DRF server after DOM object is generated.

```vue
App.vue

<template>
  <div>
    <HeaderVue />
    <ContentVue />
    <FooterVue />
  </div>
</template>

<script>
import axios from "axios"
import HeaderVue from "./components/HeaderVue.vue";
import ContentVue from "./components/ContentVue.vue";
import FooterVue from "./components/FooterVue.vue";

export default {
    data: () => {
        return {
            userList: []
        };
    },

    components: {
    HeaderVue,
    ContentVue,
    FooterVue,
  },

  mounted(){
    axios({
        method: "GET",
        url: url
    })
        .then(respones =>{
            this.userList = response.data;
        })
        .catch(response => {
            console.log("Failed", response);
        });
  },

    methods: {
        getUserList: function() {},
        updateUserList: function() {},
        deleteUserList: function() {}
    }
};
</script>
```


### Component data transmission
- Upper component -> Sub component (ex) app -> footer, header, content) : props attribute
- Sub component -> Upper component (ex) footer, head, content -> app) : event emit

```js
//app.js
<template>
    <div>
        <HeaderVue></HeaderVue>
        <ContentVue v-bind:propsdata="userList"></ContentVue>
        <FooterVue></FooterVue>
    </div>
</template>
```

```vue
// componenets/Content.vue

<v-list-item v-for="(data, index) in propsdata" v-bind:key="index">
    <v-list-item-content>
        <v-list-item-title>Name : {{data.username}}</v-list-item-title>
            <v-list-item-subtitle>Age : {{data.age}}, Location: {{data.city}}
        </v-list-item-subtitle>
    </v-list-item-content>
</v-list-item>
```

## Delete function
Add below content to Content.vue

```js
deleteUser: function(id) {
            axios({
                method: "DELETE",
                url: url + id + "/",
            })
            .then((response) => {
                this.$emit("deleted");  // send deleted event to super componenet
                console.log("Success", response);
            })
            .catch((error) => {
                console.log("Failed to get userList", error.response);
            })
        },
```

## Update function
Revise App.vue and Content.vue like this

```js
// App.vue
mounted(){
    axios({     // get userList from DRF server and store it
        method: "GET",
        url: url
    })
    .then(response => {
        for(var index in response.data){
            response.data[index].is_hidden = false; // add is_hidden = false to data from backend
        }
        this.userList=response.data;
        console.log("Success", response);
    })
    .catch(error => {
        console.log("Failed to get userList", error.response);
    });

    axios({
        method: "GET",
        url: url
    })
    .then(response =>{
        this.userList = response.data;
    })
    .catch(response => {
        console.log("Failed", response);
    });
  },

    methods: {
        getUserList: function() {
            axios({
                method: "GET",
                url: url,
            })
            .then((response) => {
                for(var index in response.data){
                    response.data[index].is_hidden = false; // add is_hidden = false to data from backend
                }
                this.userList = response.data;
                console.log("Success", response.data);
            })
            .catch((error) => {
                console.log("Failed to get userList", error.response)
            });
        },
        updateUserList: function() {},
        deleteUserList: function() {}
    }
```

```vue
<!-- content.vue-->
                <v-flex class="userList" column>
                    <v-card max-width="600" tile>
                        <v-list-item v-for="(data, index) in propsdata" v-bind:key="index">
                            <v-list-item-content>
                                <v-list-item-title>Name : {{data.username}}</v-list-item-title>
                                <v-list-item-subtitle>Age : {{data.age}}, Location: {{data.city}}</v-list-item-subtitle>
                            </v-list-item-content>
                             <!-- form which update data if is_hidden = true -->
                             <v-form v-show="data.is_hidden">
                                <v-container>
                                    <v-row>
                                        <v-col col="12" md="4">
                                            <v-text-field v-model="data.username" :counter="15" label="Username" required>
                                            </v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-text-field v-model="data.age" label="Age" disabled></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-text-field v-model="data.city" label="City" required></v-text-field>
                                        </v-col>
                                        <v-col cols="12" md="4">
                                            <v-btn class="ma-2" @click="
                                            data.is_hidden = !data.is_hidden;
                                            updateUser(data);
                                            "
                                            v-show="data.is_hidden"
                                            color="#4CAF50"
                                            >Save</v-btn>
                                            <v-btn class="ma-2" @click="
                                            deleteUser(data);
                                            "
                                            color="#F443326"
                                            >Delete</v-btn>
                                        </v-col>
                                    </v-row>
                                </v-container>
                             </v-form>
                        </v-list-item>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-container>
    </div>
</template>

<script>
import axios from "axios";

let url = "http://localhost:8000/";


export default {
    data: () => {
        return {
            data: {
                username: "",
                age: "",
                city: "",
                xsrfCookieName: 'csrftoken', 
                xrfHeaderName: 'X-CSRFToken',
                withCredentials: true,
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
                // execute get if suceed
                this.$emit('saved');
            })
            .catch((error) => {
                console.log("Failed to get userList", error.reponse);
            });
        },
         
        clearForm: function() {
            (this.data.username=""), (this.data.age=""), (this.data.city= "");
        },

        deleteUser: function(id) {
            axios({
                method: "DELETE",
                url: url + id + "/",
            })
            .then((response) => {
                this.$emit("deleted");  // send deleted event to super componenet
                console.log("Success", response);
            })
            .catch((error) => {
                console.log("Failed to get userList", error.response);
            })
        },

        updateUser: function(data) {
            axios({
                method:"PATCH",
                url: url+data.id+"/",
                data: data,
            })
            .then((response) => {
                this.$emit("patched");
                console.log("Success", response);
            })
            .catch((error) => {
                console.log("Failed to patched userList", error.response);
            })
        },
        onlyUserListCard: function(data, propsdata) {
            for(var index in propsdata) {
                data.id != propsdata[index].id ? (propsdata[index].is_hiddent = false): "";
            }
        },
    },
};
</script>
```