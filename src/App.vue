<template>

  <div id="app">
    <!-- Process -->
    <div id="names" class="container flex flex-col justify-center items-center h-screen py-96" v-if="showForm">
      <div class="grid md:grid-cols-2 gap-2 md:gap-6">
        <!-- Images -->
        <div class="w-full">
          <img src="/img/vector.svg" alt="Images">
        </div>
        <div class="flex flex-col">
          <!-- Date -->
          <div>
            <Date />
          </div>
          <!-- Titles -->
          <div class="flex flex-col gap-4 mt-3 lg:mt-5">
            <h1 class="text-3xl md:text-4xl xl:text-6xl font-bold uppercase text-primary">
              Randomize Arisan
            </h1>
            <p class="text-md md:text-lg xl:text-xl font-semibold text-secondary">
              Kocok arisan kamu dan lihat siapa yang beruntung. Ayo coba aplikasi ini sekarang!
            </p>
          </div>
          <!-- Input -->
          <div class="flex text-xl w-full justify-center mt-3 lg:mt-5">
            <!-- Input Name -->
            <input type="text" v-model.trim="inputName" class="border-2 w-full border-gray-300 p-1 lg:p-2 focus:outline-none rounded-l-lg" placeholder="Masukkan Nama ...">
            <!-- Add Button -->
            <button @click="addNameToList" class="bg-primary text-white rounded-r-lg w-auto px-2 lg:px-4 hover:opacity-50 transition duration-300 ease-in-out">Tambah</button>
          </div>
          <!-- Check Button -->
          <div class="mt-8 md:mt-4 xl:mt-auto">
            <div
            v-if="isReady"
            class="w-full flex justify-center"
            >
              <button class="w-full bg-primary text-white font-semibold py-1 md:py-2 px-px text-xl rounded-t-lg md:rounded-lg hover:opacity-85 transition duration-300 ease-in-out" @click="showResults">
                  Kocok Arisan
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Name List -->
      <div class="w-full h-60 bg-slate-200 rounded-b-lg md:rounded-xl md:mt-14">
        <h6 class="text-lg sm:text-xl lg:text-2xl font-bold text-secondary py-1 px-2 md:py-2 md:px-4">Nama Peserta Arisan</h6>
        <div class="flex flex-wrap">
          <div
              v-for="(name,index) in names"
              :key="name"
              @click="removeName(index)"
              class="cursor-pointer m-2 md:m-4 bg-secondary text-white inline-block py-1 px-2 md:py-2 md:px-4 rounded-lg text-md hover:opacity-85 h-max"
          >
              {{ name }}
          </div>
        </div>
      </div>
    </div>

    <!-- Loading Spinner -->
    <div v-if="loading" class="bg-slate-100 flex flex-col justify-center items-center h-screen">
      <svg class="animate-spin w-20 mr-3" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path opacity="0.2" fill-rule="evenodd" clip-rule="evenodd" d="M12 19C15.866 19 19 15.866 19 12C19 8.13401 15.866 5 12 5C8.13401 5 5 8.13401 5 12C5 15.866 8.13401 19 12 19ZM12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" fill="#000000"/>
      <path d="M2 12C2 6.47715 6.47715 2 12 2V5C8.13401 5 5 8.13401 5 12H2Z" fill="#525CEB"/>
      </svg>
      <p class="text-md sm:text-lg lg:text-xl font-semibold text-secondary">Sedang Proses...</p>
    </div>

    <!-- Result -->
    <div v-if="showResult" id="result" class="container flex flex-col justify-center items-center h-screen">
      <div>
        <div class="flex flex-col gap-8 mb-10">
          <h1 class="text-2xl md:text-6xl lg:text-8xl font-bold text-secondary uppercase text-center">✨ Selamat Kepada ✨
          </h1>
          <h1 class="text-3xl md:text-6xl lg:text-8xl font-semibold text-primary text-center animate-bounce">
              {{ result }}
          </h1>
          <p class="text-xl md:text-2xl lg:text-4xl font-semibold text-primary text-center">Yang mendapatkan arisan kali ini. Jangan lupa taktirannya ya hehe...</p>
        </div>

        <div class="flex flex-col md:flex-row gap-4">
          <button class="md:w-1/2 bg-primary text-white py-2 px-20 text-xl rounded-lg hover:opacity-85 transition duration-300 ease-in-out"
            @click="resetApp"
          >
              Reset Halaman
          </button>
          <button class="md:w-1/2 bg-secondary text-white py-2 px-20 text-xl rounded-lg hover:opacity-85 transition duration-300 ease-in-out"
              @click="getNewResult"
          >
              Kocok Lagi Cuy
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Date from '@/components/Date.vue'

  export default {
    name: 'App',
    data(){
      return {
        inputName:'',
        names:['Sasha','Marsha','Maxim','Rudy', 'Alvin'],
        error:'',
        showError:false,
        result:'',
        showForm: true,
        showResult: false,
        loading: false,
      }
    },
    computed:{
      isReady(){
        return this.names.length > 1;
      }
    },
    methods:{
      addNameToList(){
        const userName = this.inputName;
        if(this.validate(userName)){
            this.names.push(userName);
            this.inputName = '';
            this.showError = false;
        } else {
            this.showError = true;
        }
      },
      validate(value){
        this.error = '';
        if(value === '' ){
          this.$swal({
            title: 'Ketik dulu namanya cuy',
            icon: 'warning',
            confirmButtonColor: "#525CEB",
            confirmButtonText: "Oke cuy"
          });
          return false;
        }
        if(this.names.includes(value)){
          this.$swal({
            title: 'Namanya udah ada cuy',
            icon: 'warning',
            confirmButtonColor: "#525CEB",
            confirmButtonText: "Oke cuy"
          });
          return false;
        }
        return true
      },
      removeName(index){
        this.names.splice(index,1)
      },
      getRandomName(){
        return this.names[Math.floor(Math.random() * this.names.length)]
      },
      generateResult(){
        let rand = this.getRandomName();

        if(this.result !== ''){
          while (rand === this.result){
              rand = this.getRandomName();
          }
        }
        this.result = rand;
      },
      showResults(){
        this.loading = true; 
        this.showForm = false;

        setTimeout(() => {
          this.generateResult();
          this.loading = false;
          this.showResult = true;
        }, 2000);
        this.generateResult();
      },
      resetApp(){
        this.showForm = true;
        this.inputName ='';
        this.names = [];
        this.error = '';
        this.showError = false;
        this.result = '';
        this.showResult = false;
      },
      getNewResult(){
        this.generateResult();
      }
    },
    components:{
      Date
    }
  }
  
</script>