<script setup lang="ts">
import { ref } from '@vue/reactivity';
import { apiCheckToken, apiCsrfCookie, apiForgotPassword, apiResetPassword } from '../plugins/api';
import { Notyf } from 'notyf';
import { onMounted } from '@vue/runtime-core';
import { useRoute } from 'vue-router'

let email = ref("")
let password = ref("")
let confirmPassword = ref("")
let isDisabled = ref(true)
let spinner = ref("spinner-border spinner-border-sm")
let recoveryButton = ref(" Espere, comprobando datos")

const route = useRoute();

const notyf = new Notyf({
    duration: 10000,
    position: {
        x: 'right',
        y: 'bottom'
    },
    dismissible: true
});

onMounted(() => {
    apiCheckToken(String(route.params.token)).then(function(response) {
        email.value = response.data.email
        isDisabled.value = false
        spinner.value = ""
        recoveryButton.value = "Recover password"
    }).catch(function(response) {
        if (response.response.status == 404) {
            notyf.error('Incorrect token');
        }
    })

    apiCsrfCookie()
})

function recover() {
    if (password.value.length < 8) {
        notyf.error('The password must contain at least 8 characters!');
        return false
    }

    if (password.value != confirmPassword.value) {
        notyf.error("The passwords don't match!");
        return false
    }

    apiResetPassword(email.value, password.value, String(route.params.token)).then(function(response) {
        console.log(response)
        if (response.data.errors) {
            response.data.errors.forEach((element: string) => {
                notyf.error(element);
            });
            return false
        }

        if (response.status == 200) {
            notyf.success('Updated password!');
            isDisabled.value = true
        }
    }).catch(function(response) {
        console.log(response)
        if (response.response.status == 404) {
            notyf.error('Incorrect data, this link is invalid');
            isDisabled.value = true
        }
    })
}
</script>
<template>
<div>
    <main>

        <!-- Section -->
        <section class="vh-lg-100 mt-5 mt-lg-0 bg-soft d-flex align-items-center">
            <div class="container">
                <div class="row justify-content-center form-bg-image">
                    <p class="text-center">
                        <router-link to="/" class="d-flex align-items-center justify-content-center">
                        <svg class="icon icon-xs me-2" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M7.707 14.707a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l2.293 2.293a1 1 0 010 1.414z" clip-rule="evenodd"></path></svg>
                            Back to login
                        </router-link>
                    </p>
                    <div class="col-12 d-flex align-items-center justify-content-center">
                        <div class="bg-white shadow border-0 rounded p-4 p-lg-5 w-100 fmxw-500">
                            <h1 class="h3 mb-4">Recover password</h1>
                            <div class="mb-4">
                                <label for="email">E-mail address</label>
                                <div class="input-group">
                                    <input type="email" class="form-control" id="email" v-model="email" required disabled>
                                </div>  
                            </div>
                            <!-- End of Form -->
                            <!-- Form -->
                            <div class="form-group mb-4">
                                <label for="password">Your password</label>
                                <div class="input-group">
                                    <span class="input-group-text" id="basic-addon2">
                                        <svg class="icon icon-xs text-gray-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z" clip-rule="evenodd"></path></svg>
                                    </span>
                                    <input type="password" placeholder="Password" class="form-control" id="password" v-model="password" required>
                                </div>  
                            </div>
                            <!-- End of Form -->
                            <!-- Form -->
                            <div class="form-group mb-4">
                                <label for="confirm_password">Confirm password</label>
                                <div class="input-group">
                                    <span class="input-group-text" id="basic-addon2">
                                        <svg class="icon icon-xs text-gray-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z" clip-rule="evenodd"></path></svg>
                                    </span>
                                    <input type="password" placeholder="Confirm password" class="form-control" id="confirm_password" v-model="confirmPassword" required>
                                </div>  
                            </div>
                            <!-- End of Form -->
                            <div class="d-grid">
                                <button type="button" :disabled="isDisabled" v-on:click="recover" class="btn btn-gray-800">
                                    <div :class="spinner" role="status"></div>
                                    {{recoveryButton}}
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
</div>    
</template>
<style lang="css" scoped>
@import '../css/volt/volt.css';
</style>