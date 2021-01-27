<template>
  <div class="user-profile">
    <div class="user-profile__sidebar">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">@{{ state.user.username }}</h1>
        <div class="user-profile__admin-badge" v-if="state.user.isAdmin">
          Admin
        </div>
        <div class="user-profile__follower-count">
          <strong>Followers: </strong> {{ state.followers }}
        </div>
      </div>
      <CreateTwootPanel @add-twoot="addTwoot"/>
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
          v-for="twoot in state.user.twoots"
          :key="twoot.id"
          :username="state.user.username"
          :twoot="twoot"
      />
    </div>
  </div>
</template>

<script>
import { reactive, computed } from 'vue';
import { useRoute } from 'vue-router';
import { users } from "../assets/users";
import TwootItem from "../components/TwootItem";
import CreateTwootPanel from "../components/CreateTwootPanel";
export default {
  name: "UserProfile",
  components: { CreateTwootPanel, TwootItem },
  setup() {
    const route = useRoute();
    const userId = computed(() => route.params.userId)
    const state = reactive({
      followers: 0,
      user: users[userId.value - 1] || users[0]
    })
    function addTwoot(twoot) {
      state.user.twoots.unshift({ id: state.user.twoots.length + 1, content: twoot });
    }
    return {
      state,
      addTwoot,
      userId
    }
  }
};
</script>

<style lang="scss" scoped>
.user-profile{
  display:grid;
  grid-template-columns:1fr 3fr;
  width:100%;
  padding:50px 5%;
  box-sizing:border-box;

  .user-profile__user-panel{
    display:flex;
    flex-direction:column;
    padding:20px;
    background-color:white;
    border-radius:5px;
    border:1px solid #dfe3e8;

    .user-profile__admin-badge{
      background: rebeccapurple;
      color:white;
      border-radius:5px;
      margin-right:auto;
      padding:0 10px;
      font-weight:bold;
      margin-bottom: 20px;
    }

    h1{
      margin:0;
    }
    .user-profile__create-twoot{
      display:flex;
      flex-direction:column;
      border-top:1px solid #dfe3e8;
      padding-top:20px;

      &.--exceeded{
        color:red;
        border-color:red;
        button{
          background-color: red;
          border:none;
          color:white;
        }
      }
    }
  }
  .user-profile__twoots-wrapper{
    padding:0 15px 15PX 15px;
  }
}


@media(max-width:767px){
  .user-profile{
    grid-template-columns:1fr;
    padding:20px 5%;
  }
  .user-profile__twoots-wrapper{
    padding:30px 0 0 0;
  }
}
</style>
