<script setup>
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user.js";
import { ref } from "vue";
import ShowErrors from "../components/ShowErrors.vue";

const router = useRouter();
const userStore = useUserStore();

const form = ref({
    name: "",
    password: "",
});
const errors = ref({
    // name: ["a", "b", "c"],
});

const login = async () => {
    const { ok, errors: loginErrors } = await userStore.login(
        form.value.name,
        form.value.password,
    );

    if (!ok) {
        errors.value = loginErrors;
        return;
    }

    await router.push("/");
};
</script>

<template>
    <div>
        <form
            @submit.prevent="login"
            class="mx-auto mt-[100px] space-y-4 rounded-md border p-8 shadow-xl sm:max-w-[500px]"
        >
            <h1 class="text-2xl font-bold">Login</h1>
            <div class="space-y-2">
                <label class="block" for="name">User Name</label>
                <input
                    class="w-full rounded"
                    id="name"
                    type="text"
                    placeholder="User Name"
                    v-model="form.name"
                    required
                    :class="{ 'border-red-500': errors.name }"
                />
                <ShowErrors :errors="errors.name" />
            </div>
            <div class="space-y-2">
                <label class="block" for="password">Password</label>
                <input
                    class="w-full rounded"
                    id="password"
                    type="password"
                    placeholder="Password"
                    v-model="form.password"
                    required
                    :class="{ 'border-red-500': errors.password }"
                />
                <ShowErrors :errors="errors.password" />
            </div>
            <div>
                <button class="w-full rounded-md bg-blue-500 py-2 text-white">
                    Login
                </button>
            </div>
            <div>
                <router-link to="/auth/register" class="text-blue-500">
                    Register
                </router-link>
            </div>
        </form>
    </div>
</template>
