<script setup>
import { Head, Link } from '@inertiajs/vue3';
import Logo from "@/Components/Logo.vue";
import {reactive, ref, watch} from "vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

defineProps({
    canLogin: Boolean,
    canRegister: Boolean,
    laravelVersion: String,
    phpVersion: String,
});

const round = ref(1);
const lap = ref(1);
const gameState = ref(0);
const numberOfPlayers = ref("2");
const currentPlayer = ref(1);
const police = ref(false);

const addCircle = function () {
    if(gameState.value === 0 || gameState.value === 2) {
        gameState.value = 1;
        return;
    }
    if(currentPlayer.value < numberOfPlayers.value) {
        if(police.value) {
            currentPlayer.value++;
            police.value = false;
        } else {
            police.value = true;
        }
    } else {
        if(!police.value) {
            police.value = true;
            return;
        }
        police.value = false;
        currentPlayer.value = 1;
        if (lap.value === 5) {
            lap.value = 1;
            round.value++;
            gameState.value = 2;
        } else {
            lap.value++;
        }
    }
}

watch(numberOfPlayers, (el) => {
    if(!(el >= 2 && el <= 8)) {
        console.log(numberOfPlayers);
        numberOfPlayers.value = "2";
    }
});
</script>

<template>
    <Head title="Gangs in the city helper" />

    <div class="relative flex justify-between items-center min-h-screen min-h-[100svh] bg-gradient-to-br from-slate-800 to-slate-600 flex-col">
        <div class="max-w-7xl mx-auto p-6">
            <div class="text-center justify-center">
                <Logo class="h-24" />

            </div>
        </div>
        <div class="max-w-7xl mx-auto p-6">
            <div class="text-center justify-center">
                <div
                    v-if="gameState === 0"
                >
                    <div class="text-white">Number of players:</div>
                    <input
                        v-model="numberOfPlayers"
                        type="number"
                        min="2"
                        max="8"
                        class="border-gray-300 focus:border-gray-500 focus:ring-gray-500 rounded-md shadow-sm bg-transparent text-white mt-3 w-[100px]"
                    />
                </div>
                <div
                    v-if="gameState === 1"
                >
                    <div class="text-white text-2xl">Round: {{ round }}</div>
                    <div class="text-white text-2xl">Lap: {{ lap }}</div>
                    <div class="text-white text-2xl">Player: {{ police ? 'Police' : currentPlayer }}</div>
                </div>
                <div
                    v-if="gameState === 2"
                >
                    <div class="text-white text-2xl">Round: Mega Battle!</div>
                </div>
            </div>
        </div>
        <div class="max-w-7xl mx-auto p-6">
            <div class="text-center justify-center">
                <div>
                    <PrimaryButton
                        @click="addCircle"
                    >
                        Next
                    </PrimaryButton>
                </div>
            </div>
        </div>
    </div>
</template>


<style>
.slide-fade-enter-active {
    transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
    transition: all 0.3s ease-out;
}

.slide-fade-enter-from,
.slide-fade-leave-to {
    transform: translateX(20px);
    opacity: 0;
}

.fade-enter-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
