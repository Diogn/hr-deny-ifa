<template>
    <div class="main-content flex-1 bg-gray-700 pt-16 md:mt-2 pb-24 md:pb-5">
      <div class="max-w-5xl mx-auto"> 
        <div class="flex justify-end py-4">
            <div class="flex justify-end items-center">
                <div class="bg-green-600 hover:bg-green-700 text-white cursor-pointer text-base p-1 px-2 mx-1 rounded" @click="add()">Add</div>
                <router-link :to="{name: 'employees'}" tag="div" class="bg-transparent hover:underline cursor-pointer text-white text-base p-1 mx-1 rounded"><a>Cancel</a></router-link>
            </div>
        </div>
        <div class="w-full bg-white overflow-y-auto max-h-75">
            <div class="shadow relative">
              <h1 class="py-4 px-10 border-b font-bold text-gray-700 rounded-t bg-white">Add New Employee</h1>
              <div class="min-w-full rounded-b bg-gray-100 px-10 pt-5 pb-8 border-t border-white">
                  <div class="flex">
                      <div class="w-2/4">
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Name</p>
                              <input type="text" placeholder="" class="block w-full bg-white text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="name">
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Email</p>
                              <input type="email" placeholder="" required="required" class="block w-full bg-white text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="email">
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Phone</p>
                              <input type="tel" placeholder="" required="required" class="block w-full bg-white text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="phone">
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Gender</p>
                              <label class="mr-2 inline-flex items-baseline">
                                  <input type="radio" name="gender" value="male" v-model="gender" checked><span class="ml-1">Male</span></label>
                              <label class="inline-flex items-baseline">
                                  <input type="radio" name="gender" value="female" v-model="gender"><span class="ml-1">Female</span></label>
                          </div>
                          <div class="w-full mb-6">
                            <p class="mb-2 text-gray-600">Upload Photo</p>
                            <div class="flex">
                                <input class="w-2/5 block" type="file" accept="image/*" @change="uploadImage($event)" id="file-input">
                                <div class="bg-cover bg-center" style="height: 120px; width: 120px" :style="{ 'background-image': 'url(' + photo + ')' }" v-if="photo"></div>
                            </div>
                          </div>
                      </div>
                      <div class="w-2/4 ml-10">
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Birth Date</p>
                              <input type="date" placeholder="" class="block w-full bg-white text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="birth_date">
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Birth Place</p>
                              <input type="text" placeholder="" class="block w-full bg-white text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500" v-model="birth_place">
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Position</p>
                              <div class="relative">
                                <select class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow-inner leading-tight focus:outline-none focus:shadow-outline capitalize" v-model="role_jobs">
                                  <option v-for="(item, idx) in roleJob" :key="idx" :value="item.id" class="capitalize"> {{ item.divition }} - {{ item.position }} </option>
                                </select>
                                <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" class="fill-current h-4 w-4"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"></path></svg></div>
                              </div>
                          </div>
                          <div class="w-full mb-6">
                              <p class="mb-2 text-gray-600">Employee Status</p>
                              <div class="relative">
                                <select class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow-inner leading-tight focus:outline-none focus:shadow-outline capitalize" v-model="status_employee">
                                  <option v-for="(item, idx) in employeeStatus" :key="idx" :value="item.id" class="capitalize"> {{ item.status }} </option>
                                </select>
                                <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" class="fill-current h-4 w-4"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"></path></svg></div>
                              </div>
                          </div>
                      </div>
                  </div>
              </div>
              <transition name="fade">
                <div class="custom-modal-succes capitalize absolute flex justify-center items-center text-white bg-green-400 rounded" v-if="addSuccess">Success add new Employee</div>
              </transition>
              <transition name="fade">
                <div class="custom-modal-succes capitalize absolute flex justify-center items-center text-white bg-red-400 rounded" v-if="addError">Error: some input are empty</div>
              </transition>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
import { mapActions, mapGetters } from 'vuex'
export default {
    beforeRouteEnter (to, from, next) {
        $cookies.get('local_login') ? next() : next({ name: 'login', query: { redirect: 'home' } })
    },
    data(){
        return{
            addSuccess: false,
            addError: false,
            name: '',
            email: '',
            phone: '',
            gender: 'male',
            birth_date: '1993-06-20',
            birth_place: '',
            role_jobs: 0,
            status_employee: 0,
            photo: '',
        }
    },
    watch: {
        addSuccess(){
            if(this.addSuccess){
                let self = this;
                setTimeout(function(){
                    self.addSuccess = false;
                    self.$router.push({ name: 'employees' });
                },1500);
            }
        },
        addError(){
            if(this.addError){
                let self = this;
                setTimeout(function(){
                    self.addError = false;
                },1500);
            }
        },
    },
    computed:{
        ...mapGetters({
            employee: 'employee/employee',
            roleJob: 'employee/roleJob',
            employeeStatus: 'employee/employeeStatus'
        })
    },
    methods:{
        ...mapActions({
            addEmployee: 'employee/addEmployee',
            fetchEmployees : 'employee/fetchEmployees',
            fetchRoleJob : 'employee/fetchRoleJob',
            fetchEmployeeStatus : 'employee/fetchEmployeeStatus'
        }),
        add(){
            let last_id = this.employee.length>0 ? (this.employee[this.employee.length-1].id) : 0;
            let new_user = {
                "id" : parseInt(last_id)+1,
                "role_job" : parseInt(this.role_jobs),
                "status_employee" : parseInt(this.status_employee),
                "name" : this.name,
                "email" : this.email,
                "phone" : this.phone,
                "gender" : this.gender,
                "birth_date" : this.birth_date,
                "birth_place" : this.birth_place,
                "photo" : this.photo,
                "password" : "kompas2020"
            }
            if(this.name != '' && this.email != '' && this.phone != '' && this.birth_place != '' && this.photo != ''){
                this.addSuccess = true;
                this.addEmployee(new_user);
            }else{
                this.addError = true;
            }
        },
        uploadImage(event) {
            let filesSelected = event.target.files;
            let fileToLoad = filesSelected[0];
            let fileReader = new FileReader();
            let sef = this;
            fileReader.onload = function(fileLoadedEvent) 
            {
                sef.photo = fileLoadedEvent.target.result;
            };
            fileReader.readAsDataURL(fileToLoad);
        }
    },
    async created(){
        await this.fetchEmployees();
        await this.fetchRoleJob();
        await this.fetchEmployeeStatus();
    }
}
</script>