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
        <h1 class="text-center text-4xl mb-6">Enter DigitalRealms</h1>
        <form @submit.prevent="register">
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
          <button
            type="submit"
            class="w-full font-semibold mt-4 bg-blue-500 text-white px-3 py-2 rounded"
          >
            Log in!
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axiosInstance from "@/axiosConfig";
import { toast } from "vue3-toastify";

export default {
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    async register() {
      try {
        await axiosInstance
          .post("/login", {
            email: this.email,
            password: this.password,
          })
          .then(() => {
            toast.success("Login successful!", {
              autoClose: 5000,
              position: toast.POSITION.TOP_RIGHT,
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
          toast.error("Wrong email or password.", {
            autoClose: 5000,
            position: toast.POSITION.TOP_RIGHT,
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
