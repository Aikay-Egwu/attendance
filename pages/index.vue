<template>
  <div class="container mx-auto">
    <p class="text-xl font-bold">Sunday School Attendance</p>
    <p>Attending Sunday School requires you to be in Church. This application requies you to grant access to your location </p>
    <div class="my-1">
      <input
      type="text" 
      v-model="firstname"
      placeholder="Firstname"
      name="firstname"
      class="px-1 border border-solid rounded-sm border-black">

    <div class="my-1">
    </div>
    <input
      type="text"
      placeholder="Lastname"
      name="lastname"
      v-model="lastname" 
      class="px-1 border border-solid rounded-sm border-black">

    </div>
    <button 
      @click="submit"
      class="border-2 px-3 py-1 rounded-md"
    >Present</button>
    {{ message }}
  </div>
</template>

<script setup>
  const message = ref('')
  const firstname = ref('')
  const lastname = ref('')
  const position = reactive({lat: '', long: ''})
  let postcodes = ref([])
  const postcode = 'SR4 6RQ'

  onMounted(() => {
    console.log("let us check the location")
  })


  async function getPosition (cords) {
    
    position.lat = cords.coords.latitude
    position.long = cords.coords.longitude
    
    const { data: posts} =  await useFetch(`https://api.postcodes.io/postcodes?lon=${cords.coords.longitude}&lat=${cords.coords.latitude}`)
    
    //find the postcode in the list
    const found = posts.value.result.find((post, i) => post.postcode == postcode)
    if (found) {
      message.value = "You are in the right location, thank you for attending sunday school today"
    }

  } 
  function submit() {
    if (window.navigator.geolocation) {
      window.navigator.geolocation.getCurrentPosition(getPosition)
    }
    console.log(firstname.value)
  }
</script>

<style lang="scss" scoped>

</style>