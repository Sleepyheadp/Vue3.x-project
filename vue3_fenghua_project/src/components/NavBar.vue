<template>
    <nav class="navbar">
        <router-link to="/"><img src="../assets/logo.svg" alt=""></router-link>
        <div class="searchInput">
            <input type="text" @change='searchPosts'>
            <TheIcon icon="search" />
        </div>
        <div class="navItems">
            <router-link to="/">
                <TheIcon icon="home" />
            </router-link>
            <button>
                <TheIcon icon="publish" @click="publishPost()" />
            </button>
            <!-- dropdown -->
            <div class="profileDropDown">
                <TheAvatar 
                    width="42" 
                    height="42" 
                    style="cursor:pointer" 
                    @click="showDropDown = !showDropDown"
                    :src="user.avatar"
                />
                <div class="dropdownMenu" v-show="showDropDown">
                    <ul class="profileMenu" @click="showDropDown = false">
                        <li><router-link to="/profile">个人主页</router-link></li>
                        <li @click="logout">退出登录</li>
                    </ul>
                </div>
            </div>
        </div>
    </nav>
</template>

<script setup>
import TheIcon from "./TheIcon.vue";
import TheAvatar from "./TheAvatar.vue";
import { ref, computed } from 'vue'
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'

const store = useStore()
const router = useRouter()

const user = computed(() => store.state.user.user)
console.log('user', user);
// 下拉菜单显隐
const showDropDown = ref(false)

function publishPost() {
    store.commit('changeShowPostUpload', true)
}

// 搜索查询
async function searchPosts(e) {
    await store.dispatch("searchPosts", e.target.value);
    router.push({
        name: "search_result",
        params: {
            term: e.target.value,
        },
    });
}
// 退出登录方法
async function logout() {
    await store.dispatch('logoutUser')
    router.push('/login')
}
</script>
<style scoped>
.navbar {
    width: 80vw;
    height: 80px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    align-items: center;
}

.navbar svg {
    width: 38px;
    height: 38px;
}

.searchInput {
    position: relative;
}

.searchInput input {
    width: 100%;
    padding: 12px;
    padding-left: 36px;

    background: #f1f1f1;
    border-radius: 14px;
    border: none;
}

.searchInput>svg {
    position: absolute;
    left: 0;
    top: 11px;
    left: 12px;
}

.navItems {
    justify-self: end;
    display: flex;
    gap: 24px;
    align-items: center;
}

.navItems>button {
    border: none;
    background: none;
}

.profileDropDown {
    position: relative;
}

.profileMenu {
    position: absolute;
    width: max-content;
    padding: 24px 26px;
    list-style: none;
    background: white;
    box-shadow: 0px 0px 24px rgba(0, 0, 0, 0.08);
    border-radius: 4px;
    right: 0;
    display: grid;
    row-gap: 18px;
    transform: translateY(18px);
}

.profileMenu::before {
    content: "";
    display: block;
    position: absolute;
    width: 0;
    height: 0;
    top: -12px;
    right: 10px;
    border-bottom: 12px solid white;
    border-left: 12px solid transparent;
    border-right: 12px solid transparent;
}

.profileMenu a,
.profileMenu li {
    text-decoration: none;
    cursor: pointer;
}

.profileMenu a:visited {
    color: initial;
}
</style>