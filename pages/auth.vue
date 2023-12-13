<template>
    <div id="authPage" class="w-full h-[100vh] bg-white">
        <div class="w-full flex items-center justify-center p-5 border-b border-b-gray-300">
            <NuxtLink to="/" class="min-w-[170px]">
                <NuxtImg src="/AliExpress-logo.png" width="170" />
            </NuxtLink>
        </div>
        <div class="max-w-[400px] mx-auto px-2">
            <div class="text-center my-6">
                Login / Register
            </div>
            <button @click="login('google')"
                class="flex items-center justify-center gap-3 p-1.5 w-full border hover:bg-gray-100 rounded-full text-lg font-semibold">
                <NuxtImg class="w-full max-w-[30px]" src="/google-logo.png" />
                <div>Google</div>
            </button>
            <button @click="login('github')"
                class="mt-4 flex items-center justify-center gap-3 p-1.5 w-full border hover:bg-gray-100 rounded-full text-lg font-semibold">
                <NuxtImg class="w-full max-w-[30px]" src="/github-logo.png" />
                <div>GitHub</div>
            </button>
        </div>
    </div>
</template>

<script setup>
/* const client = useSupabaseClient();
const user = useSupabaseUser(); */

/* watchEffect(() => {
    if (user.value) {
        navigateTo("/");

        return;
    }
});  */

const login = async (provider) => {
    const { data, error } = await client.auth.signInWithOAuth({
        provider
    });
};
</script> 