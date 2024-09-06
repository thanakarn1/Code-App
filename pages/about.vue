<template>
  <v-container style="margin: 50px; margin-left: -20px; padding: 150px">
    <!-- User Profile Section -->
    <v-row>
      <v-col cols="12" md="4">
        <v-card
          style="
            backdrop-filter: blur(10px);
            width: 100%;
            margin: 0;
            text-align: start;
            padding: 0;
            color: white;
            background: linear-gradient(#141e30, #243b52);
            background: transparent;
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 30px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
          "
        >
          <v-img
            src="https://w0.peakpx.com/wallpaper/816/942/HD-wallpaper-alan-walker-back-view-superstars-stage-dj-alan-walker-djs.jpg"
          ></v-img>
          <v-card-title>{{ user.name }}</v-card-title>
          <v-card-subtitle>{{ user.email }}</v-card-subtitle>
        </v-card>
      </v-col>

      <!-- User Activities Section -->
      <v-col
        cols="12"
        md="8"
        style="
          backdrop-filter: blur(10px);
          width: 100%;
          margin: 0;
          text-align: start;
          padding: 0;
          color: white;
          background: linear-gradient(#141e30, #243b52);
          background: transparent;
          border: 2px solid rgba(255, 255, 255, 0.2);
          border-radius: 30px;
          box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        "
      >
        <v-card
          style="
            backdrop-filter: blur(10px);
            width: 100%;
            margin: 0;
            text-align: start;
            padding: 0;
            color: white;
            background: linear-gradient(#141e30, #243b52);
            background: transparent;
            border-radius: 30px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
          "
        >
          <v-card-title>Recent Activities</v-card-title>
          <v-list
            style="
              backdrop-filter: blur(10px);
              width: 100%;
              margin: 0;
              text-align: start;
              padding: 0;
              color: white;
              background: linear-gradient(#141e30, #243b52);
              background: transparent;
            "
          >
            <v-list-item v-for="(activity, index) in activities" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{ activity.title }}</v-list-item-title>
                <v-list-item-subtitle>{{ activity.date }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
          <v-card-title></v-card-title>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      user: {
        name: "Thanakarn Nittayachit",
        email: "6121010025@ccollege.ac.th",
        username: "Thanakarn Nittayachit",
        phone: "0621045594",
      },
      activities: [
        { title: "Logged in", date: "2024-07-30" },
        { title: "Updated profile", date: "2024-07-29" },
        { title: "Posted a comment", date: "2024-07-28" },
      ],
    };
  },
};
</script>
