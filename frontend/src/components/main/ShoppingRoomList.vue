<template>
  <div class="room-container">
    <h2><i class="bi bi-record2-fill me-1" ></i>LIVE</h2>
    <div v-if="shoppingRoomList.length" class="row">
      <div
        id="room"
        class="room col-6"
        :class="index % 2 ? 'room-right' : 'room-left'"
        :style="{
          'background-image': `url(${require(`@/assets/shopping_${
            (index % 5) + 1
          }.png`)})`,
        }"
        v-for="(room, index) in shoppingRoomList"
        :key="index"
        @click="selectShoppingRoom(room)"
      >
        <div class="participant">
          <i class="bi bi-eye me-1"></i> {{ room.participantCount }} /
          {{ room.maxParticipantCount }}
        </div>
        <i v-if="room.isPrivate" class="fas fa-lock"></i>
        <div class="room-info">
          <p class="mall-name">{{ room.shoppingMallName }}</p>
          <div>
            <span class="host-name">[ {{ room.hostName }} ]</span
            ><span>님의 쇼핑룸</span>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="info-message">
      <i class="fas fa-store"></i>
      <h4>라이브 중인 <span>쇼핑룸</span>이 없습니다.</h4>
      <h5>친구를 추가하거나 쇼핑룸을 생성해주세요.</h5>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";
import { useStore } from "vuex";

export default {
  name: "ShoppingRoomList",
  setup() {
    const store = useStore();

    const shoppingRoomList = computed(() => {
      return store.state.room.shoppingRoomList;
    });

    const loadShoppingRoomList = () => {
      store.dispatch("room/loadShoppingRoomList");
    };

    const selectShoppingRoom = (room) => {
      store.dispatch("room/selectedRoom", room);
    };

    // created
    loadShoppingRoomList();

    return {
      shoppingRoomList,
      selectShoppingRoom,
    };
  },
};
</script>

<style scoped>
.room-container {
  height: 775px;
  width: 543px;
  background-color: #f5e6c0;
  border-radius: 16px;
  padding: 0;
  margin-bottom: 20px;
  position: relative;
  box-shadow: 2px 8px 18px rgb(207 206 206);
  border: 3px solid #f7d682;
}

h2 {
  font-size: 40px;
  font-weight: bold;
  padding: 30px 43px;
  margin: 0;
  color: red;
}

.row {
  width: 543px;
  height: 691px;
  overflow-y: scroll;
  padding: 0 43px 0 43px;
  margin: 0;
}

/* 스크롤 */
.row::-webkit-scrollbar {
  width: 7px;
}
.row::-webkit-scrollbar-track {
  background-color: transparent;
}
.row::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #2f3542;
}

.room {
  position: relative;
  padding: 0;
  margin-bottom: 45px;
  width: 205px;
  height: 297px;
  border-radius: 10px;
  border: 3px solid #d3e2e7;
  cursor: pointer;
  background-color: #ffffff;

  background-repeat: no-repeat;
  background-size: contain;
  background-position: 0 20%;
}

.room:hover {
  box-shadow: 1px 1px 15px rgb(207, 206, 206);
}

.room-left {
  margin-right: 20px;
}
.room-right {
  margin-left: 20px;
}

.participant {
  position: absolute;
  top: 18px;
  left: 18px;
  width: 65px;
  background-color: rgba(224, 222, 222, 0.884);
  border-radius: 5px;
  padding: 0 5px 0;
  text-align: center;
  font-size: 14px;
}

.fa-lock {
  position: absolute;
  top: 17px;
  right: 18px;
  font-size: 20px;
  color: #ad1d4e;
}

.room-info {
  position: absolute;
  bottom: 0;
}

.room-info p {
  font-size: 16px;
  font-weight: bold;
}

.room-info div {
  margin-bottom: 18px;
}

.room-info span {
  font-size: 16px;
  font-weight: bold;
}

.mall-name {
  margin: 0 23px 11px;
}

.host-name {
  margin: 0 0 18px 23px;
}

.info-message {
  margin-top: 240px;
}

.info-message i {
  position: absolute;
  left: 50%;
  top: 28%;
  transform: translate(-50%, 0);
  font-size: 80px;
  color: #1b4d50;
}

h4,
h5 {
  text-align: center;
  font-weight: bold;
}

h4 > span {
  color: #f2af46;
}
</style>
