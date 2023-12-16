<template>
    <MainLayout>
        <div id="shoppingCartPage" class="mt-4 max-w-[1200px] mx-auto px-2">
            <div v-if="!userStore.cart.length" class="h-[500px] flex items-center justify-center">
                <div class="pt-20">
                    <NuxtImg class="mx-auto" width="250" src="/cart-empty.png" />
                    <div class="text-xl text-center mt-4">No items yet?</div>
                    <div v-if="!user" class="flex text-center">
                        <NuxtLink to="/auth"
                            class="bg-[#FD374F] w-full text-white text-[21px] font-semibold p-1.5 rounded-full mt-4">
                            Sign in
                        </NuxtLink>
                    </div>
                </div>
            </div>
            <div v-else class="md:flex gap-4 justify-center mx-auto w-full">
                <div class="md:w-[65%]">
                    <div class="bg-white rounded-lg p-4">
                        <div class="text-2xl font-bold mb-2">
                            Shopping Cart ({{ userStore.cart.length }})
                        </div>
                    </div>
                    <div class="bg-[#FEEEEF] ronunded-lg p-4 mt-4">
                        <div class="text-red-500 font-bold">
                            Welcome Deal applicable on 1 item only
                        </div>
                    </div>
                    <div id="items" class="bg-white rounded-lg p-4 mt-4">
                        <div v-for="product in userStore.cart" :key="product.id">
                            <CartItem :product="product" :selectedArray="selectedArray"
                                @selectedRadio="selectedRadioFunc" />
                        </div>
                    </div>
                </div>
                <div class="md:hidden block my-4" />
                <div class="md:w-[35%]">
                    <div id="summary" class="bg-white rounded-lg p-4">
                        <div class="text-2xl font-extrabold mb-2">
                            Summary
                        </div>
                        <div class="flex items-center justify-between my-4">
                            <div class="font-semibold">Total</div>
                            <div class="text-2xl font-semibold">
                                $ <span class="font-extrabold">{{ totalPriceComputed }}</span>
                            </div>
                        </div>
                        <button
                            class="flex items-center justify-center bg-[#FD374F] w-full text-white text-[21px] font-semibold p-1.5 rounded-full mt-4"
                            @click="goToCheckout()">
                            Checkout
                        </button>
                    </div>
                    <div id="paymentProtection" class="bg-white rounded-lg p-4 mt-4">
                        <div class="text-lg font-semibold mb-2">
                            Payment methods
                        </div>
                        <div class="flex items-center justify-start gap-8 my-4">
                            <div v-for="card in cards" :key="card.id">
                                <NuxtImg class="h-6" :src="card" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </MainLayout>
</template>

<script setup>
import MainLayout from "../layouts/MainLayout.vue";
import { useUserStore } from "../stores/user";

const userStore = useUserStore();
const user = useSupabaseUser();

const selectedArray = ref([]);

onMounted(() => {
    setTimeout(() => {
        userStore.isLoading = false;
    }, 200);
});

const cards = ref([
    "visa.png",
    "mastercard.png",
    "paypal.png",
    "applepay.png"
]);

const totalPriceComputed = computed(() => {
    let price = 0;

    userStore.cart.forEach((cardProduct) => {
        price += cardProduct.price;
    });

    return price / 100;
});

const selectedRadioFunc = (element) => {
        if (!selectedArray.value.length) {
            selectedArray.value.push(element);

            return
        }

        selectedArray.value.forEach((item, index) => {
            if (element.id !== item.id) {
                selectedArray.value.push(element);
            } else {
                selectedArray.value.splice(index, 1);
            }
        });
    };

const goToCheckout = () => {
    let ids = [];

    userStore.checkout = [];

    selectedArray.value.forEach(item => { ids.push(item.id) });

    let result = userStore.cart.filter(item => {
        return ids.indexOf(item.id) != -1;
    });

    result.forEach(item => {
        userStore.checkout.push(toRaw(item));
    });

    navigateTo("/checkout");

    return;
};
</script>