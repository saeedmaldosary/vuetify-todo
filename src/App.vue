<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="title">
            Vuetify Todo
          </v-list-item-title>
          <v-list-item-subtitle>
            Best Todo ever
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :to="item.to" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="#fcb69f"
      dark
      src="https://picsum.photos/1920/1080?random"
      prominent
    >
      <template v-slot:img="{ props }">
        <v-img
          v-bind="props"
          gradient="to top right, rgba(19,84,122,.5), rgba(128,208,199,.8)"
        ></v-img>
      </template>

      <v-col>
        <v-row>
          <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
          <v-spacer></v-spacer>

          <v-text-field
            @focus="searchClosed = false"
            @blur="searchClosed = true"
            class="searchInput mt-1"
            v-model="search"
            :class="{ 'closed-d': searchClosed }"
            placeholder="Search"
            prepend-inner-icon="mdi-magnify"
            filled
            dense
          ></v-text-field>
        </v-row>

        <v-row>
          <v-app-bar-title>Vuetify Todo</v-app-bar-title>
        </v-row>
      </v-col>
    </v-app-bar>

    <v-main>
      <router-view :search="search"></router-view>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    drawer: false,
    searchClosed: true,
    search: "",
    items: [
      { title: "Todo", icon: "mdi-format-list-checks", to: "/" },
      { title: "About", icon: "mdi-image", to: "/About" },
    ],
  }),
};
</script>

<style lang="sass">
.searchInput
  transition: max-width 1s
  .v-input__slot
    cursor: pointer !important
    &:before, &:after
      border-color: transparent !important
  &.closed-d
    max-width: 45px
    .v-input__slot
      background: transparent !important
</style>
