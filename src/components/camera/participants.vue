<template>
    <div @click="cameraStore.preview_participants">
        <div v-if="participants.length < 5" class="flex flex-col -space-y-4 rtl:space-x-reverse">
            <div v-if="participants.length > 0" v-for="(participant, index) in participants" :key="index" class="rounded-full border-2 border-white dark:border-gray-800 overflow-hidden h-14 w-14">
                <img  class="w-full h-full object-cover" :src="'assets/'+participant.name+'.webp'" :alt="`${participant.name}`">
            </div>
            <div v-else class="rounded-full border-2 border-white dark:border-gray-800 overflow-hidden h-14 w-14">
                <img  class="w-full h-full object-cover" :src="'/assets/face1.webp'" :alt="`participant`">
            </div>
        </div>
        <div v-else class="flex flex-col -space-y-4 rtl:space-x-reverse">
            <div v-for="(participant, index) in participants.slice(0, 5)" :key="index" class="rounded-full border-2 border-white dark:border-gray-800 overflow-hidden h-14 w-14">
                <img v-if="index < 4" class="w-full h-full object-cover" :src="'/assets/'+participant.name+'.webp'" :alt="`${participant.name}`">
                <span v-else @click="cameraStore.preview_participants" class="flex cursor-pointer items-center justify-center w-fbg-gray-700 ull h-full text-xs font-medium text-white bg-gray-700 " href="#">+{{ participants.length-4 }}</span>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
    import { useCameraStore, type Participant } from '@/stores/cameraStore'
    import { computed, onMounted, ref, type Ref } from 'vue';

    let cameraStore = useCameraStore()
    let loadedSpace = computed(() => cameraStore.loaded_space)
    let spaces = computed(() => cameraStore.spaces)
    let participants: Ref<Participant[]> = ref([]);

    onMounted(() => {
        let loadedSpaceValue = spaces.value.find(space => space.id == loadedSpace.value.id)

        if(loadedSpaceValue)
            participants.value = loadedSpaceValue.participants
    })
</script>