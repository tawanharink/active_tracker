<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- Error Handling -->
    <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-light-grey shadow-lg">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Information -->
    <form
      @submit.prevent="createUser"
      class="p-8 flex flex-col bg-light-grey rounded-md shadow-lg"
    >
      <h1 class="text-3xl text-at-light-green mb-4">Information</h1>

      <div class="flex flex-col mb-2">
        <label for="email" class="mb-1 text-sm text-at-light-green">Email</label>
        <input
          type="text"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="email"
          v-model="email"
        />
      </div>

      <div class="flex flex-col mb-2">
        <label for="fullName" class="mb-1 text-sm text-at-light-green">Full Name</label>
        <input
          type="text"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="fullName"
          v-model="fullName"
        />
      </div>

      <div class="flex flex-col mb-2">
        <label for="licensePlate" class="mb-1 text-sm text-at-light-green">License plate</label>
        <input
          type="text"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="licensePlate"
          v-model="licensePlate"
        />
      </div>

      <button
        type="submit"
        class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-light-green duration-200 border-solid
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green"
      >
        Submit
      </button>

    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";
//import { uid } from "uid";

export default {
  name: "information",
  setup() {
    // Create data / vars
    const router = useRouter();
    const email = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const fullName = ref("");
    const licensePlate = ref("");
    const errorMsg = ref(null);
    const statusMsg = ref(null);

    // const test = () => {
    //   console.log("test")
    // }

    //insert data to users
    const createUser = async () => {
      try {
        console.log(fullName.value);
        console.log(licensePlate.value);
        const { error } = await supabase.from("user_information").insert([
          {
            // id: uid(),
            // created_at: new Date(),
            email: email.value,
            fullName: fullName.value,
            licensePlate: licensePlate.value,
          },
        ]);
        if (error) throw error;
        console.log("Succes: User Added!");
        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error createUser: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    // Register function
    // const register = async () => {
    //   if (password.value === confirmPassword.value) {
    //     try {
    //       const { error } = await supabase.auth.signUp({
    //         email: email.value,
    //         password: password.value,
    //       });
    //       createUser();
    //       if (error) throw error;
    //       router.push({ name: "Login" });
    //     } catch (error) {
    //       errorMsg.value = error.message;
    //       setTimeout(() => {
    //         errorMsg.value = null;
    //       }, 5000);
    //     }
    //     return;
    //   }
    //   errorMsg.value = "Error: Passwords do not match";
    //   setTimeout(() => {
    //     errorMsg.value = null;
    //   }, 5000);
    // };

    return { email, password, confirmPassword, errorMsg, fullName, licensePlate, createUser };
  },
};
</script>
