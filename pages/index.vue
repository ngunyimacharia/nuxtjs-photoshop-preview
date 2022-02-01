<template>
<div class="min-h-full flex flex-col justify-center py-12 sm:px-6 lg:px-8">

  <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
    <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
      <form class="space-y-6" action="#" method="POST">
        <div>
          <label for="psd" class="block text-sm font-medium text-gray-700">
            Photoshop file
          </label>
          <div class="mt-1">
            <input id="psd" name="psd" type="file" required class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          </div>
        </div>

        <div>
          <button type="submit" class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Upload
          </button>
        </div>
      </form>

    </div>
  </div>

  <div class="w-4/5 mx-auto my-10 grid grid-cols-3 gap-4">
    <div class="h-full w-full flex m-5" v-for="image in images" :key="image" >
      <img :src="image" class="m-auto" />
    </div>
  </div>
</div>

</template>

<script>


export default {
  name: 'IndexPage',
  data(){
    return {
      cloudinaryFile:null,
      images:[]
    };
  },
  mounted(){
    this.cloudinaryFile = {
        public_id: "nuxtjs-photoshop-preview/Cld_Sample_PSD"
    };
  },
  watch:{
    cloudinaryFile(){
      this.getLayers();
    }
  },
  methods:{
    async getLayers(){
      let count = 0;
      let resp;
      do{
        count++;
        let url =  this.$cloudinary.image.url(
          `${this.cloudinaryFile.public_id}.jpg`,
          {page:count}
        );
        resp = await fetch(url);
        if(resp.ok){
          setTimeout(() => this.images.push(url), count * 0);
        }
      }while(resp.ok);
      console.log(this.images);
    }
  }
}
</script>
