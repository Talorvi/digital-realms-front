<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 rounded-lg shadow-md mx-4">
      <div class="container mx-auto">
        <div class="flex">
          <button
            class="bg-blue-500 text-white px-6 py-2 rounded mb-4"
            @click="goBack"
          >
            Return
          </button>
        </div>
        <h1 class="text-center text-4xl mb-6">Join DigitalRealms</h1>
        <form @submit.prevent="register">
          <div class="mb-4">
            <label for="name" class="block text-left">Tamer name</label>
            <input
              type="text"
              id="name"
              v-model="name"
              class="w-full px-3 py-2 border rounded"
            />
          </div>
          <div class="mb-4">
            <label for="email" class="block text-left">Email</label>
            <input
              type="email"
              id="email"
              v-model="email"
              class="w-full px-3 py-2 border rounded"
            />
          </div>
          <div class="mb-4">
            <label for="password" class="block text-left">Password</label>
            <input
              type="password"
              id="password"
              v-model="password"
              class="w-full px-3 py-2 border rounded"
            />
          </div>
          <div class="mb-4">
            <label for="password" class="block text-left"
              >Confirm password</label
            >
            <input
              type="password"
              id="c_password"
              v-model="c_password"
              class="w-full px-3 py-2 border rounded"
            />
          </div>
          <button
            type="submit"
            class="w-full font-semibold mt-4 bg-blue-500 text-white px-3 py-2 rounded"
          >
            Register me!
          </button>

          <div
            v-if="Object.keys(errorMessages).length > 0"
            class="error-messages mt-4"
          >
            <ul>
              <li v-for="(messages, field) in errorMessages[0]" :key="field">
                <ul>
                  <li v-for="message in messages" :key="message">
                    <div class="text-red-500">{{ message }}</div>
                  </li>
                </ul>
              </li>
            </ul>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axiosInstance from "@/axiosConfig";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

export default {
  data() {
    return {
      name: "",
      email: "",
      password: "",
      c_password: "",
      errorMessages: [],
    };
  },
  methods: {
    async register() {
      const id = toast.loading("Pending...", {
        position: toast.POSITION.TOP_RIGHT,
      });

      try {
        await axiosInstance
          .post("/register", {
            name: this.name,
            email: this.email,
            password: this.password,
            c_password: this.c_password,
          })
          .then(() => {
            toast.update(id, {
              closeOnClick: true,
              closeButton: true,
              type: "success",
              isLoading: false,
              render: "Registration successful!",
              autoClose: 5000,
            });
          });

        this.$router.push({ path: "/home" });

        // Navigate to another page or show a success message
      } catch (error) {
        if (error.response && error.response.data && error.response.data.data) {
          this.errorMessages = [];
          const messages = error.response.data.data;

          for (const field in messages) {
            // eslint-disable-next-line no-prototype-builtins
            if (messages.hasOwnProperty(field)) {
              this.errorMessages = this.errorMessages.concat(messages[field]);
            }
          }
          toast.update(id, {
            autoClose: 5000,
            closeOnClick: true,
            closeButton: true,
            type: "error",
            isLoading: false,
            render: "Could not register. Look at the errors down below.",
          });
        }
      }
    },
    goBack() {
      this.$router.go(-1);
    },
  },
};
</script>
