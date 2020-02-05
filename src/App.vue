<template>
  <v-app>
    <v-content class="pa-5">
      <MessageBox v-for="(log, index) of logs" 
        :key="index" 
        :logs="logs" 
        :log="log" 
        :hasDivider="index != logs.length - 1" 
        :index="index" 
        :startingMinutes="startingMinutes" />
    </v-content>
  </v-app>
</template>

<script>
import MessageBox from './components/MessageBox';

// import session1_1_1 from '@/assets/sessions/session_1-1.json'
import userlist from '@/assets/users/session_1-2.json'
// import session from '@/assets/sessions/session_1-2.json'
import session from '@/assets/sessions/session_1-1.json'

export default {
  name: 'App',
  components: {
    MessageBox,
  },
  data() {
    return {
      users: userlist.users,
      logs: session.logs,
      startingMinutes: this.timeToMinutes(session.startingTime),
    }
  },
  computed: {
  },
  methods: {
    timeToMinutes(time) {
      let minutes = 0;
      let m = time.split(" ")[1];
      let timeArr = time.split(" ")[0].split(":");
      if (m == "PM") minutes += (12 * 60);
      minutes += parseInt(timeArr[1]);
      minutes += (parseInt(timeArr[0] * 60));

      return minutes;
    }
  }
};
</script>

<style>
.v-application {
  background-color: #36393f !important;
  color: #dcddde !important;
}
</style>