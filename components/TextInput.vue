<template>
    <div>
        <ClientOnly>
            <input :placeholder="placeholder" :maxlength="max"
                class="w-full bg-white text-gray-800 border text-sm border-[#EFF0EB] rounded-lg p-3 placeholder-gray-500 focus:outline-none"
                @focues="isFocused = true" @blur="isFocused = false"
                :class="[isFocused && 'border-gray-900', error && 'border-red-500']" :type="inputType"
                v-model="inputComputed" autocomplete="off" />
        </ClientOnly>
        <span v-if="error" class="text-red-500 text-[14px] font-semibold">
            {{ error }}
        </span>
    </div>
</template>

<script setup>
const emit = defineEmits(["update:input"]);

const props = defineProps(["input", "placeholder", "max", "inputType", "error"]);
const { input, placeholder, max, inputType, error } = toRefs(props);

const isFocused = ref(false);

const inputComputed = computed({
    get: () => input.value,
    set: (value) => emit("update:input", value)
});
</script>