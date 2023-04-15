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
#### Installation

```shell
$ npm install axios
```