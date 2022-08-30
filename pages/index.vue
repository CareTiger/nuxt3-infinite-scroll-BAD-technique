<template>
    <ul role="list" class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 container mx-auto">
        <li class="col-span-1 flex flex-col divide-y divide-gray-200 rounded-lg bg-gray-100 text-center shadow hover:cursor-pointer" v-for="photo in data" :key="photo.id">
            <!-- <nuxt-link :to="`/photos/:${photo.url}`" @click.native="gotoDetails(photo.url)"> -->
            
            <nuxt-link to="" @click.native="gotoDetails(photo.url)">
                <div class="flex flex-1 flex-col p-8">
                <img class="mx-auto h-32 w-32 flex-shrink-0 rounded-full" :src="photo.thumbnailUrl" alt="">
                <h3 class="mt-6 text-sm font-medium text-gray-900">{{photo.title}}</h3>
                </div>
            </nuxt-link>
        </li>
    </ul>  
</template>

<script setup>
const router = useRouter();

const data = await $fetch('https://jsonplaceholder.typicode.com/photos', {
                    method: 'GET',
                })
console.log("Nos of photos " + data.length)  

async function gotoDetails(url) {
    console.log(url)
    try {
        await navigateTo({
        path: '/photos',
        query: { id: url },
        })   
        
    } catch (error) {
        console.log(error)
    }
    return
}              
</script>