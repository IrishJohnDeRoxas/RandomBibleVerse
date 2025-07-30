<script setup lang="ts">
import { onMounted, ref } from 'vue';
import {
    Card,
    CardContent,
    CardDescription,
    CardHeader,
    CardTitle,
} from '@/components/ui/card'

import { Skeleton } from '@/components/ui/skeleton'
import { Button } from './ui/button';
import axios from 'axios';

interface RandomVerseResponse {
    random_verse: {
        text: string;
        book: string;
        chapter: number;
        verse: number;
    };
    translation: {
        name: string;
    };
}

const apiResponse = ref<RandomVerseResponse | null>(null)
const verse = ref<string>('')
const verseReference = ref<string>('')
const verseTranslation = ref<string>('')
const fetching = ref(false)

const API_URL = 'https://bible-api.com/data/web/random'

async function fetchRandomVerse() {
    fetching.value = true
    apiResponse.value = null;
    verse.value = '';
    verseReference.value = '';
    verseTranslation.value = '';

    try {
        const response = await axios.get<RandomVerseResponse>(API_URL);
        const data = response.data;

        apiResponse.value = data

        verse.value = data.random_verse.text
        verseReference.value = `${data.random_verse.book} ${data.random_verse.chapter}:${data.random_verse.verse}`
        verseTranslation.value = data.translation.name

    } catch (error) {
        if (axios.isAxiosError(error)) {
            console.error("Error fetching data:", error.message);
            if (error.response) {
                console.error("Status:", error.response.status);
                console.error("Data:", error.response.data);
                console.error("Headers:", error.response.headers);
            } else if (error.request) {
                console.error("No response received:", error.request);
            } else {
                console.error("Error setting up request:", error.message);
            }
        } else {
            console.error("An unexpected error occurred:", error);
        }
    } finally {
        fetching.value = false
    }
}

onMounted(() => {
    fetchRandomVerse()
})

</script>

<template>
    <div class="min-h-[190px]">
        <Transition name="fade-scale" mode="out-in">
            <Card v-if="apiResponse" :key="apiResponse.random_verse.text">
                <CardHeader class="max-h-[30px]">
                    <CardTitle>
                        {{ verseReference }}
                    </CardTitle>
                    <CardDescription>
                        {{ verseTranslation }}
                    </CardDescription>
                </CardHeader>
                <CardContent class="min-h-[80px] max-h-[80px] overflow-y-auto">
                    {{ verse }}
                </CardContent>
            </Card>

            <Card v-else>
                <CardHeader class="max-h-[30px]">
                    <Skeleton class="h-4 w-3/4 mb-2" />
                    <Skeleton class="h-2 w-1/2" />
                </CardHeader>
                <CardContent class="min-h-[80px] max-h-[80px] overflow-y-auto">
                    <Skeleton class="h-4 w-full mb-2" />
                    <Skeleton class="h-4 w-11/12 mb-2" />
                    <Skeleton class="h-4 w-5/6 mb-2" />
                    <Skeleton class="h-4 w-full mb-2" />
                    <Skeleton class="h-4 w-1/2" />
                </CardContent>
            </Card>
        </Transition>
    </div>
    <Button variant="link" @click="fetchRandomVerse()" class="cursor-pointer w-fit" :disabled="fetching">
        Random verse
    </Button>
</template>

<style>
.fade-scale-enter-active,
.fade-scale-leave-active {
    transition: opacity 0.5s ease, transform 0.2s ease;
}

.fade-scale-enter-from,
.fade-scale-leave-to {
    opacity: 0;
    transform: scale(0.95);
}
</style>