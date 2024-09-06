<template>
    <v-layout style=" background-image: url(https://images.unsplash.com/photo-1568781269551-3e3baf5ec909?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D);">
      <v-app-bar style="background-image: url(https://images.unsplash.com/photo-1483707288101-bf92ee3183a6?q=80&w=1776&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D); background-size: cover; color: white;" title="LOGIN PAGE" class="text-center font-italic "></v-app-bar>
  
      <v-navigation-drawer style="background-image: url(https://images.unsplash.com/photo-1514707298111-6d136d5e6485?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D);background-size: cover;">
        <v-list>
          <v-list-item title=""></v-list-item>
        </v-list>
      </v-navigation-drawer >
  
      <v-navigation-drawer location="right" style="background-image: url(https://images.unsplash.com/photo-1514707298111-6d136d5e6485?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D);background-size: cover; transform: scalex(-1);">
        <v-list>
          <v-list-item title=""></v-list-item>
        </v-list>
      </v-navigation-drawer>
  
      <v-main class="" >
        <NuxtPage />
       
      </v-main>
    </v-layout>
  </template>