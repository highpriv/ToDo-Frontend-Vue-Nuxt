<template>
  <v-card elevation="2" width="100%" flat>
    <v-navigation-drawer permanent>
      <v-list class="d-flex justify-center grey lighten-3" elevation="10">
        <v-list-item
          class="ml-0"
          link
          @click="$router.push({ name: 'profile' })"
        >
          <v-list-item-avatar>
            <v-img src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title class="text-h6">
              @{{ loggedUser.username }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ loggedUser.email }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <v-divider></v-divider>
      <v-list nav class="mt-6">
        <v-list-item-group v-model="selectedItem" color="primary">
          <v-list-item v-for="(item, i) in items" :key="i">
            <v-list-item-icon>
              <v-icon v-text="item.icon"></v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title v-text="item.text"></v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
  </v-card>
</template>
