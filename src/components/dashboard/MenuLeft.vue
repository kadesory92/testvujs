<script setup lang="ts">
import { useRouter } from "vue-router";
import { apiLogout } from "../../plugins/api";
import {authUserStore} from '../../store/authUser'


const router = useRouter()
function logout() {
    apiLogout().then(function(response) {
        authUserStore().logout()
        router.push('/')
    })
}
</script>
<template>
<div>
  <nav class="navbar navbar-dark navbar-theme-primary px-4 col-12 d-lg-none">
    <a class="navbar-brand me-lg-5" href="../../index.html">
      <img class="navbar-brand-dark" src="/img/light.svg" alt="Volt logo" /> <img
        class="navbar-brand-light" src="/img/dark.svg" alt="Volt logo" />
    </a>
    <div class="d-flex align-items-center">
      <button class="navbar-toggler d-lg-none collapsed" type="button" data-bs-toggle="collapse"
        data-bs-target="#sidebarMenu" aria-controls="sidebarMenu" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
    </div>
  </nav>

  <nav id="sidebarMenu" class="sidebar d-lg-block bg-gray-800 text-white collapse" data-simplebar>
    <div class="sidebar-inner px-4 pt-3">
      <div class="user-card d-flex d-md-none align-items-center justify-content-end justify-content-md-end pb-4">
        <div class="collapse-close d-md-none">
          <a href="#sidebarMenu" data-bs-toggle="collapse" data-bs-target="#sidebarMenu" aria-controls="sidebarMenu"
            aria-expanded="true" aria-label="Toggle navigation">
            <svg class="icon icon-xs" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"></path>
            </svg>
          </a>
        </div>
      </div>
      <ul class="nav flex-column pt-3 pt-md-0">
        <li class="nav-item">
          <a href="#" class="nav-link d-flex align-items-center">
            <span class="sidebar-icon">
              <img src="/img/light.svg" height="20" width="20" alt="Volt Logo">
            </span>
            <span class="mt-1 ms-1 sidebar-text">Overview</span>
          </a>
        </li>
        <li class="nav-item active">
          <a href="/dashboard" class="nav-link">
            <span class="sidebar-icon">
              <font-awesome-icon icon="fa-solid fa-chart-line" class="icon icon-xs me-2"/>
            </span>
            <span class="sidebar-text">Dashboard</span>
          </a>
        </li>

        <li class="nav-item">
          <a href="/datatable" class="nav-link">
            <span class="sidebar-icon">
              <font-awesome-icon icon="fa-solid fa-table" class="icon icon-xs me-2"/>
            </span>
            <span class="sidebar-text">Datatable</span>
          </a>
        </li>
        
        
        <li class="nav-item">
          <span class="nav-link  collapsed  d-flex justify-content-between align-items-center" data-bs-toggle="collapse"
            data-bs-target="#submenu-app">
            <span>
              <span class="sidebar-icon">
                <svg class="icon icon-xs me-2" fill="currentColor" viewBox="0 0 20 20"
                  xmlns="http://www.w3.org/2000/svg">
                  <path fill-rule="evenodd"
                    d="M5 4a3 3 0 00-3 3v6a3 3 0 003 3h10a3 3 0 003-3V7a3 3 0 00-3-3H5zm-1 9v-1h5v2H5a1 1 0 01-1-1zm7 1h4a1 1 0 001-1v-1h-5v2zm0-4h5V8h-5v2zM9 8H4v2h5V8z"
                    clip-rule="evenodd"></path>
                </svg>
              </span>
              <span class="sidebar-text">Parent</span>
            </span>
            <span class="link-arrow">
              <svg class="icon icon-sm" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd"
                  d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                  clip-rule="evenodd"></path>
              </svg>
            </span>
          </span>
          <div class="multi-level collapse " role="list" id="submenu-app" aria-expanded="false">
            <ul class="flex-column nav">
              <li class="nav-item ">
                <a class="nav-link" href="#">
                  <span class="sidebar-text">Child Item</span>
                </a>
              </li>
            </ul>
          </div>
        </li>
      </ul>
    </div>
  </nav>
</div>
</template>
<style lang="css" scoped>
@import '../../css/volt/volt.css';
</style>