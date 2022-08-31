<template>
    <ul role="list" class="grid grid-cols-1 gap-6 container mx-auto" ref="scrollComponent">
        <li class="col-span-1 flex flex-col divide-y divide-gray-200 rounded-lg bg-gray-100 text-center shadow hover:cursor-pointer" v-for="photo in photos" :key="photo.id">
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
const scrollComponent = ref(null)

// Fetch the data 
const data = await $fetch('https://jsonplaceholder.typicode.com/photos', {
                    method: 'GET',
                })
console.log("total Nos of photos fetched " + data.length)  

const getPhotos = (number) => {
    let ret = []
    for (let index = 0; index < number; index++) {
        ret.push(data[index]);    
    }
    return ret
}

const photos = ref(getPhotos(10))

//  creating a method that loads in 10 posts at a time and appends them to our posts variable.
const loadMorePhotos = () => {
        let newPhotos = getPhotos(10)
        console.log(newPhotos)
        photos.value.push(...newPhotos)
    }

// trigger loadMorePosts method. by adding an event listener that listens to a scroll event and calls a method. We are going to add it when the component is mounted and remove it when the component is unmounted (destroyed).  
onMounted(() => {
    window.addEventListener("scroll", handleScroll)
})

onUnmounted(() => {
    window.removeEventListener("scroll", handleScroll)
})

const handleScroll = (e) => {
    let element = scrollComponent.value
    if (element.getBoundingClientRect().bottom < window.innerHeight) {
        loadMorePhotos()
    }
}



// dynamic routing to show the image detail
/* async function gotoDetails(url) {
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
}       */        
</script>