<template>
    <div>
        <div
            class="relative max-w-md mx-auto md:max-w-2xl mt-6 min-w-0 break-words bg-white w-full mb-6 rounded-xl mt-16 bg-neutral-200	">
            <div class="px-6">
                <div class="flex flex-wrap justify-center">
                    <div class="w-full flex justify-center">
                        <div class="relative">
                            <img src="https://upload.wikimedia.org/wikipedia/commons/7/7a/Alarm_Clock_GIF_Animation_High_Res.gif"
                                class="rounded-full align-middle border-none absolute -m-16 -ml-20 lg:-ml-16 max-w-[150px]" />
                        </div>
                    </div>

                    <div class="w-full text-center mt-12">
                        <div class="flex justify-center lg:pt-4 pt-8 pb-0">
                            <div class="p-3 text-center">
                                <span class="text-xl font-bold block uppercase tracking-wide text-slate-700">
                                    {{ data.displayDays }}
                                </span>
                                <span class="text-sm text-slate-400">Days</span>
                            </div>
                            <div class="p-3 text-center">
                                <span class="text-xl font-bold block uppercase tracking-wide text-slate-700">
                                    {{ data.displayHours }}
                                </span>
                                <span class="text-sm text-slate-400">Hours</span>
                            </div>
                            <div class="p-3 text-center">
                                <span class="text-xl font-bold block uppercase tracking-wide text-slate-700">
                                    {{ data.displayMinutes }}
                                </span>
                                <span class="text-sm text-slate-400">Minute</span>
                            </div>

                            <div class="p-3 text-center">
                                <span class="text-xl font-bold block uppercase tracking-wide text-slate-700">
                                    {{ data.displaySeconds }}
                                </span>
                                <span class="text-sm text-slate-400">Seconds</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="text-center mt-2">
                    <h3 class="text-2xl text-slate-700 font-bold leading-normal">
                        {{ title }}
                    </h3>
                    <div class="text-xs mt-0 mb-2 text-slate-400 font-bold uppercase">
                        <i class="fas fa-map-marker-alt mr-2 text-slate-400 opacity-75"></i>created by Arun Yadav 😍
                    </div>
                </div>
                <div class="mt-6 py-6 border-t border-slate-200 text-center"></div>
            </div>
        </div>
    </div>
</template>
<script setup>

import { onMounted, reactive, computed } from "vue";
const props = defineProps({
    endDate: {
        type: Object,
        default: false
    },
    title: {
        type: String,
        default: false
    }
});
const { year: endyear, month: endmonth, day: endday, hours: endhours, minutes: endminutes, seconds: endseconds, milliseconds: endmilliseconds } = props.endDate;

const data = reactive({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0,
});
const seconds = computed(() => {
    return 1000
});
const minutes = computed(() => {
    return seconds.value * 60
});
const hours = computed(() => {
    return minutes.value * 60
})
const days = computed(() => {
    return hours.value * 24
});
onMounted(() => {
    showRemaining();
})
const showRemaining = () => {

    const timer = setInterval(() => {
        const now = new Date();
        const end = new Date(endyear, endmonth, endday, endhours, endminutes, endseconds, endmilliseconds)
        const distance = end.getTime() - now.getTime()

        if (distance < 0) {
            clearInterval(timer)
            return;
        }

        const totalDays = Math.floor(distance / days.value);
        const totalHours = Math.floor((distance % days.value) / hours.value);
        const totalMinutes = Math.floor((distance % hours.value) / minutes.value);
        const totalSeconds = Math.floor((distance % minutes.value) / seconds.value);

        data.displayDays = totalDays
        data.displaySeconds = totalSeconds
        data.displayMinutes = totalMinutes
        data.displayHours = totalHours
    }, 1000)
}

</script>
