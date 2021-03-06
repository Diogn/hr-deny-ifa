<template>
    <div class="main-content flex-1 bg-gray-700 pt-16 md:mt-2 pb-24 md:pb-5">
      <div class="max-w-5xl mx-auto"> 
        <div class="flex justify-between">
            <ul class="flex justify-start">
                <li class="-mb-px mr-1 cursor-pointer" v-for="(item, idx) in menu" :key="idx" :class="{ 'tab-active' : activeIdx === idx }" @click="activeIdx=idx; filterName=''">
                    <a class="bg-gray-300 inline-block py-4 px-4 text-gray-500 rounded-t hover:text-blue-600 font-semibold capitalize">{{ item.name }}</a>
                </li>
            </ul>
            <div class="flex justify-end items-center">
                <div class="bg-transparent hover:underline cursor-pointer text-white text-sm p-1 mx-1 rounded" @click="exportCsv()">Export CSV</div>
                <div class="bg-transparent hover:underline cursor-pointer text-white text-sm p-1 mx-1 rounded" @click="exportAllCSV()">Export CSV (All)</div>
                <router-link :to="{name: 'add_employees'}" tag="div" class="bg-blue-600 hover:bg-blue-700 text-white cursor-pointer text-sm p-1 px-2 mx-1 rounded"><a>Add New Employee</a></router-link>
            </div>

        </div>
        <div class="w-full bg-white overflow-y-auto max-h-75">
            <div class="flex flex-1 justify-center md:justify-start text-white p-4 sticky top-0 bg-white z-10">
                <span class="relative w-full border-b border-b-2 bg-gray-200">
                    <input type="search" placeholder="Search by name" class="w-full text-lg transition border border-transparent focus:outline-none rounded py-4 px-2 pl-10 appearance-none leading-normal text-gray-600 bg-transparent" v-model="filterName">
                    <div class="absolute search-icon" style="top: 50%; left: .8rem; transform:translateY(-50%)">
                        <svg class="pointer-events-none text-white w-4 h-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" style="fill: #3182ce">
                            <path d="M12.9 14.32a8 8 0 1 1 1.41-1.41l5.35 5.33-1.42 1.42-5.33-5.34zM8 14A6 6 0 1 0 8 2a6 6 0 0 0 0 12z"></path>
                        </svg>
                    </div>
                </span>
            </div>
            <div class="relative">
                <table class="min-w-full">
                    <tbody class="bg-white">
                        <tr>
                            <th class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-left">Name</th>
                            <th class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-left">Position and Diviton</th>
                            <th class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-left">Employee Status</th>
                            <th class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-left"></th>
                        </tr>
                        <tr class="hover:bg-gray-100" v-for="(item, idx) in getEmployee" :key="idx">
                            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200">
                                <div class="flex items-center">
                                    <div class="flex-shrink-0 h-10 w-10">
                                        <img :src="checkingImg(item)" alt="" class="rounded-full">
                                    </div>
                                    <div class="ml-4">
                                        <div class="text-sm leading-5 font-medium text-gray-900 capitalize">{{ item.name }}</div>
                                        <div class="text-sm leading-5 text-gray-500">{{ item.email }}</div>
                                        <div class="text-sm leading-5 text-gray-500">{{ item.phone }}</div>
                                    </div>
                                </div>
                            </td>
                            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200">
                                <div class="text-sm leading-5 text-gray-900 w-full block capitalize">{{ getRoleJobPosition(item) }}</div>
                                <div class="text-sm leading-5 text-gray-500 w-full block capitalize">{{ getRoleJobDivition(item) }}</div>
                            </td>
                            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200">
                                <div class="text-sm leading-5 text-gray-900 w-full block capitalize">{{ getEmployeeStatus(item) }}</div>
                            </td>
                            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-sm leading-5 font-medium text-center">
                                <router-link :to="{name: 'edit_employees', params: { id: item.id }}" tag="a"><i class="fas fa-cog cursor-pointer"></i></router-link>
                            </td>
                        </tr>
                        <tr v-if="getEmployee == ''">
                            <td class="px-6 py-4 whitespace-no-wrap border-b border-gray-200 text-center">
                                <div class="text-sm leading-5 text-gray-500 w-full block capitalize">No Data Entry</div>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            
        </div>
      </div>
    </div>
</template>
<script>
import { mapGetters, mapActions } from 'vuex'
export default {
    computed:{
        ...mapGetters({
            employee: 'employee/employee',
            roleJob: 'employee/roleJob',
            employeeStatus: 'employee/employeeStatus'
        }),
        getEmployee(){
            return this.filterName === '' ? this.employee.filter(ob=>ob.status_employee === this.activeIdx) : this.employee.filter(ob=>ob.status_employee === this.activeIdx && ob.name.toLowerCase().includes(this.filterName.toLowerCase()));
        }
    },
    methods:{
        ...mapActions({
            fetchEmployees : 'employee/fetchEmployees',
            fetchRoleJob : 'employee/fetchRoleJob',
            fetchEmployeeStatus : 'employee/fetchEmployeeStatus'
        }),
        getRoleJobPosition(d){
            return this.roleJob.find(ob=>ob.id === d.role_job) ? this.roleJob.find(ob=>ob.id === d.role_job).position : ''
        },
        getRoleJobDivition(d){
            return this.roleJob.find(ob=>ob.id === d.role_job) ? this.roleJob.find(ob=>ob.id === d.role_job).divition : ''
        },
        checkingImg(item){
            return item.photo ? item.photo : '/src/assets/avatar.jpg'
        },
        getEmployeeStatus(a){
            return this.employeeStatus.find(ob=>ob.id == a.status_employee) ? this.employeeStatus.find(ob=>ob.id == a.status_employee).status : ''
        },
        prepareCsvData(a){
            let fix = []
            for(let i=0;i<a.length;i++){
                let temp = {
                    no: i+1,
                    name: a[i].name,
                    email: a[i].email,
                    phone: a[i].phone,
                    gender: a[i].gender,
                    birth_date: a[i].birth_date,
                    birth_place: a[i].birth_place,
                    position: this.getRoleJobPosition(a[i]),
                    divition: this.getRoleJobDivition(a[i]),
                    status_employee: this.getEmployeeStatus(a[i]),
                    password: a[i].password,
                }
                fix.push(temp);
            }
            return fix;
        },
        exportCsv(){
            this.exportingProcess(this.prepareCsvData(this.getEmployee));
        },
        exportAllCSV(){
            this.exportingProcess(this.prepareCsvData(this.employee));
        },
        exportingProcess(items){
            const replacer = (key, value) => value === null ? '' : value // specify how you want to handle null values here
            const header = Object.keys(items[0])
            let csv = items.map(row => header.map(fieldName => JSON.stringify(row[fieldName], replacer)).join(','))
            csv.unshift(header.join(','))
            csv = csv.join('\r\n')
            let blob = new Blob(['\ufeff' + csv], { 
            type: 'text/csv;charset=utf-8;'
            }); 
            let dwldLink = document.createElement("a"); 
            let url = URL.createObjectURL(blob); 
            navigator.userAgent.indexOf('Chrome') == -1; 
            dwldLink.setAttribute("href", url); 
            dwldLink.setAttribute("download", "alltable.csv"); 
            dwldLink.style.visibility = "hidden"; 
            document.body.appendChild(dwldLink); 
            dwldLink.click(); 
            document.body.removeChild(dwldLink); 
        }
    },
    data(){
        return{
            activeIdx: 0,
            filterName: '',
            menu: [{
                key: 'permanent',
                name: 'Permanent'
            },{
                key: 'probation',
                name: 'Probation'
            },{
                key: 'contract',
                name: 'Contract'
            },]
        }
    },
    async created(){
        await this.fetchEmployees();
        await this.fetchRoleJob();
        await this.fetchEmployeeStatus();
    },
    beforeRouteEnter (to, from, next) {
        $cookies.get('local_login') ? next() : next({ name: 'login', query: { redirect: 'home' } })
    },
}
</script>