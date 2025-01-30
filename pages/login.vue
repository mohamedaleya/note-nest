<template>
  <div class="flex h-screen bg-zinc-900">
    <!-- sidebar -->
    <div class="flex w-[516px] flex-col justify-center bg-black p-12">
      <Logo />
      <h1 class="mt-8 text-lg font-bold text-white">Log in to your account</h1>
      <p class="mt-0.5 text-sm text-zinc-300">
        Don't have an account?
        <nuxt-link to="/register" class="font-bold text-[#FFAC00] underline"
          >Sign up</nuxt-link
        >
        for one.
      </p>
      <form @submit.prevent="submit">
        <div class="mt-8">
          <label for="" class="mb-1 block text-sm font-normal text-zinc-300"
            >Email Address</label
          >
          <input
            v-model="email"
            type="email"
            required
            placeholder="you@example.com"
            class="block w-full rounded border border-[#3F3F46] bg-[#27272A] px-3 py-2 text-sm text-white placeholder:text-zinc-500"
          />
        </div>
        <div class="mt-6">
          <label for="" class="mb-1 block text-sm font-normal text-zinc-300"
            >Password</label
          >
          <input
            required
            v-model="password"
            type="password"
            placeholder="**************"
            class="block w-full rounded border border-[#3F3F46] bg-[#27272A] px-3 py-2 text-sm text-white placeholder:text-zinc-500"
          />
        </div>

        <!-- sign up button -->

        <div>
          <button
            class="mt-8 flex w-full items-center justify-center space-x-2 rounded-full bg-[#FFAC00] px-4 py-2 text-sm font-semibold"
          >
            <span>Log in</span>
            <ArrowRight />
          </button>
        </div>

        <!-- /sign up button -->
      </form>
    </div>
    <!-- /sidebar -->

    <!-- note introduction section  -->
    <div></div>
    <!-- /note introduction section  -->
  </div>
</template>

<script setup>
import Swal from "sweetalert2";

const email = ref("");
const password = ref("");

async function submit() {
  try {
    const response = await $fetch("/api/login", {
      method: "POST",
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      }),
    });

    const { isConfirmed } = await Swal.fire({
      icon: "success",
      title: "Success",
      text: "Logged in successfully.",
      confirmButtonText: "Close",
    });

    if (isConfirmed) {
      navigateTo("/");
    }
  } catch (error) {
    console.log("[LOGIN_ERROR]", error.response?._data?.message);
    Swal.fire({
      icon: "error",
      title: "Login Error",
      text: error.response?._data?.message,
      confirmButtonText: "Close",
    });
  }
}
</script>
