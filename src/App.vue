<template>
    <div class="min-h-screen bg-gray-200 pt-6">
        <div class="mx-auto max-w-6xl bg-white p-6 rounded-lg shadow-2xl">
            <h1 class="text-2xl text-gray-700 border-b border-gray-400 pb-2 mb-3">
                Je timers:
            </h1>
            <ul role="list" class="divide-y divide-gray-200">
                <li
                    v-for="timer in timers"
                    :key="timer.name"
                    :class="{
                        'bg-amber-200': timer.blink,
                        'bg-green-50': timer.running && ! timer.blink
                    }"
                    class="py-2 flex justify-between"
                >
                    <div>
                        <p class="text-sm font-medium text-gray-900">{{ timer.name }}</p>
                        <p class="text-xl text-gray-800 italic">{{ timeRemaining(timer) }}</p>
                    </div>
                    <div class="flex gap-2">
                        <button
                            @click="timer.running = ! timer.running"
                            :class="timer.running ? 'bg-red-200' : 'bg-green-200'"
                            class="rounded py-2 px-4 hover:underline"
                        >
                            <template v-if="timer.running">Stop timer</template>
                            <template v-if="! timer.running">Start timer</template>
                        </button>
                        <button
                            @click="resetTimer(timer)"
                            class="bg-orange-100 rounded py-2 px-4 hover:underline"
                        >
                            Reset timer
                        </button>
                        <button
                            @click="deleteTimer(timer)"
                        >
                            Timer verwijderen
                        </button>
                    </div>
                </li>
                <li class="mt-2 py-2 flex flex-col">
                    <div class="flex gap-2">
                        <input type="text" v-model="newTimer.name" class="border border-gray-300 rounded p-2" placeholder="Naam van de timer" />
                        <input type="number" v-model="newTimer.duration" class="border border-gray-300 rounded p-2" placeholder="Duur van de timer" />
                    </div>
                    <div>
                        <button class="bg-gray-100 hover:bg-gray-200 py-2 px-3 mt-2 rounded" @click="addTimer">
                            Timer toevoegen
                        </button>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>

import { ref } from "vue";

export default {
    methods: {
        tick() {
            this.timers.forEach((timer) => {
                if (timer.running && ! timer.completed) {
                    timer.remaining--;
                }

                if(timer.remaining === 0) {
                    timer.blink = !timer.blink
                    timer.completed = true
                }
            })
        },
        addTimer() {
            this.timers.push({
                name: this.newTimer.name,
                duration: this.newTimer.duration,
                remaining: this.newTimer.duration,
                running: false,
                blink: false,
                completed: false,
            })

            this.newTimer = {
                name: "",
                duration: 300
            }
        },
        deleteTimer(timer) {
            timer = null
        },
        resetTimer(timer) {
            timer.remaining = timer.duration
            timer.blink = false
            timer.running = false
            timer.completed = false
        },
        timeRemaining(timer) {
            const date = new Date(null)
            date.setSeconds(timer.remaining)

            return date
                .toISOString()
                .substring(11, 19)
        }
    },
    mounted() {
        setInterval(this.tick, 1000)
    },
    setup() {
        const timers = ref([
            {
                name: "Aardappels",
                duration: 360,
                remaining: 360,
                running: false,
                blink: false,
                completed:false,
            },
            {
                name: "Eieren",
                duration: 600,
                remaining: 600,
                running: false,
                blink: false,
                completed:false,
            }
        ])

        const newTimer = {
            name: "nieuwe timer",
            duration: 300,
        }

        return {
            timers,
            newTimer,
        }
    },
}
</script>