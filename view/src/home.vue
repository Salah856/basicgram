<template>
    <div class="h-full">
        <div v-if="isLoggedIn">
            <v-ons-page>
                <v-ons-toolbar>
                    <div class="center">{{ title }}</div>
                </v-ons-toolbar>

                <v-ons-tabbar 
                    swipeable
                    position="auto" 
                    :tabs="tabs" 
                    :visible="true" 
                    :index.sync="activeIndex"
                >
                </v-ons-tabbar>
            </v-ons-page>
        </div>
        <div class="h-full" v-else>
            <!-- add an event "@login-success" to login component -->
            <!-- this event will trigger #loginView methods ⬇️ -->
            <login @login-success="loginView"></login>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import homePage from './pages/homepage.vue'
import camera from './pages/camera.vue'
import profile from './pages/profile.vue'
import login from './pages/login.vue'

export default {
    components: {
        login
    },
    data() {
        return {
            isLoggedIn: false,
            userName: { type: String },
            userID: { type: Number },
            activeIndex: 0,
            tabs: [
                {
                    icon: 'fa-home',
                    label: 'Home',
                    page: homePage,
                    key: "homePage",
                    props: {
                        userID: {
                            type: Number // specify typing
                        }
                    }
                }, 
                {
                    icon: 'fa-camera',
                    label: 'Camera',
                    page: camera,
                    key: "camera",
                    props: {
                        userName: {
                            type: String // specify typing
                        },
                        userID: {
                            type: Number // specify typing
                        }
                    }
                }, 
                {
                    icon: 'fa-user',
                    label: 'Profile',
                    page: profile,
                    key: "profile",
                    props: {
                        userName: {
                            type: String // specify typing
                        },
                        userID: {
                            type: Number // specify typing
                        }
                    }
                }, 
            ]
        }
    },
    methods: {
        // this is the loginView method triggered by @login-success event ⬆️
        loginView(user) {
            // this.tabs[2] passes the userName to the `profile` tab child component
            // a child passes data to a parent by events ($emit)
            // a parent passes date to child by `props
            this.userName = this.tabs[2].props.userName = this.tabs[1].props.userName = user.name
            this.userID = this.tabs[2].props.userID = this.tabs[1].props.userID = this.tabs[0].props.userID = user.id
            this.isLoggedIn = true
        }
    },
    computed: {
        title() {
            return this.tabs[this.activeIndex].label === 'Home' ? 'Basicgram' : this.tabs[this.activeIndex].label
        }
    }
}
</script>
