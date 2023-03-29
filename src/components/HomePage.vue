<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <div class="bg-white p-8 rounded-lg shadow-md mx-4">
      <h1 class="text-4xl font-medium mb-4">Welcome to DigitalRealms</h1>
      <div>
        <button
          class="w-full font-semibold mt-4 bg-white border-solid border-2 border-blue-500 text-blue-500 px-3 py-2 rounded"
          @click="navigateToLogin"
        >
          Login
        </button>
        <button
          class="w-full font-semibold mt-4 bg-blue-500 text-white px-3 py-2 rounded"
          @click="navigateToRegister"
        >
          Register
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    navigateToRegister() {
      this.$router.push({ name: "RegisterUser" });
    },
    navigateToLogin() {
      this.$router.push({ name: "LoginUser" });
    },
  },
};
</script>
