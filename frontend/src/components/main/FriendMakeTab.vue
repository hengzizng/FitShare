<template>
  <div>
    <!-- 친구 이름, 이메일 검색 input-box -->
    <div class="d-flex">
      <input
        class="input-box"
        type="text"
        placeholder="이메일 검색"
        @keyup.enter="SearchUserEmail"
        v-model="SearchUser"
      />
      <button class="search-btn" @click="SearchUserEmail">
        <i class="bi bi-search"></i>
      </button>
    </div>

    <!-- 이름, 이메일로 검색 시 나오는 users들 components -->
    <searched-users :friend="state.friend"></searched-users>
  </div>
</template>

<script>
import SearchedUsers from "./SearchedUsers.vue";
import { ref, reactive } from "vue";
import axios from "../../api/axios";

export default {
  name: "FriendMakeTab",
  components: {
    SearchedUsers,
  },
  setup() {
    const state = reactive({
      searchedUsers: [],
      friend: [],
    });

    // input에 입력하는 값
    const SearchUser = ref("");

    // input에 email 입력 시 server에 요청
    const SearchUserEmail = () => {
      if (SearchUser.value) {
        if (state.friend.length) {
          state.friend = [];
          axios({
            method: "GET",
            url: `members/${SearchUser.value}`,
          }).then((res) => {
            if (res.data.data) {
              state.friend.push(res.data.data);
              SearchUser.value = "";
            }
          });
        } else {
          axios({
            method: "GET",
            url: `members/${SearchUser.value}`,
          }).then((res) => {
            if (res.data.data) {
              state.friend.push(res.data.data);
              SearchUser.value = "";
            }
          });
        }
      } else {
        state.friend = [];
      }
    };

    return {
      state,
      SearchUser,
      SearchUserEmail,
    };
  },
};
</script>

<style>
.result {
  text-align: center;
  font-size: 12px;
  color: #565656;
}

img {
  width: 60px;
  height: 60px;
}

.input-box {
  width: 250px;
  border-radius: 20px;
  text-align: center;
}

input::placeholder {
  text-align: center;
}

.search-btn {
  border: none;
  background-color: #fdfaf3;
}
</style>
