<script>
import Table from './components/Table.vue';

export default {
    components: { Table },
    data() {
        return {
            // Header Data
            tableTitle: "مبالغ نوسازی محاسبه شده",
            headerIcons: [
                "/src/assets/sort.svg",
                "/src/assets/show.svg",
                "/src/assets/report.svg",
                "/src/assets/close.svg",
            ],
            serachIcon :[
              "/src/assets/search.svg",
            ],
            actions: [
                { type: 'print', icon: "/src/assets/print.svg" },
                { type: 'excel', icon: "/src/assets/excel.svg" }
            ],
            paginationIcon: [
                { icon: "/src/assets/prev-key.svg" },
                { icon: "/src/assets/next-key.svg" }
            ],
            
            // Table Data
            headers: [
                { key: "year", label: "سال" },
                { key: "arz", label: "عرصه" },
                { key: "ayyan", label: "اعیان" },
                { key: "services", label: "بهای خدمات پسمانده" },
                { key: "goodAccount", label: "خوش حسابی" },
                { key: "education", label: "آموزش و پرورش" },
                { key: "darakd", label: "دیرکرد" },
                { key: "discount", label: "تخفیف" },
                { key: "total", label: "جمع سال" }
            ],
            items: [
                { id: 1, year: 1400, arz: "4523456", ayyan: "4523456", services: 1, goodAccount: "سال 1400", education: "سال 1400", darakd: "سال 1400", discount: "1%", total: "300,656,170" },
                { id: 2, year: 1400, arz: "4523456", ayyan: "4523456", services: 2, goodAccount: "سال 1400", education: "سال 1400", darakd: "سال 1400", discount: "10%", total: "300,656,170" }
            ],

            // Pagination
            searchQuery: "",
            currentPage: 1,
            itemsPerPage: 5,
            pageSizeOptions: [5, 10, 15, 20, 25, 50], // Dynamic options
        };
    },
    computed: {
        filteredItems() {
            if (!this.searchQuery) return this.items;
            return this.items.filter(item => 
                Object.values(item).some(value => 
                    String(value).toLowerCase().includes(this.searchQuery.toLowerCase())
                )
            );
        },
        totalPages() {
            return Math.ceil(this.filteredItems.length / this.itemsPerPage);
        }
    },
    methods: {
        updateSearch(query) {
            this.searchQuery = query;
        },
        changePage(page) {
            if (page >= 1 && page <= this.totalPages) {
                this.currentPage = page;
            }
        }
    }
};
</script>

<template>
  <div class="container mx-auto bg-white p-4 rounded-lg">
    
    <!-- Header (Desktop: Everything in the correct order) -->
    <div class="flex flex-col md:flex-row md:items-center md:justify-between mb-4 ">
      
      <!-- Title (Right-aligned) -->
      <h2 class="text-lg font-bold text-[#364153] text-right w-full md:w-auto">
        {{ tableTitle }}
      </h2>

      <!-- Desktop: Icons (4 icons) after Title -->
      <div class="hidden md:flex md:gap-2 md:items-center">
        <span v-for="icon in headerIcons" :key="icon">
          <img :src="icon" class="w-[36px] h-[48px]">
        </span>
      </div>

      <!-- Search Input (Comes after Icons) -->
      <div class="relative w-[352px] md:w-[295px] h-[48px] mt-2 md:mt-0">
        <input 
          type="text" 
          placeholder="جستجو کنید..." 
          class="border p-2 pl-10 pr-10 rounded-[12px] w-full h-[48px] focus:outline-none text-[13px] text-right"
          v-model="searchQuery"
        >
        <span class="absolute right-3 top-1/2 transform -translate-y-1/2">
          <img :src="serachIcon[0]" class="w-[20px] h-[20px]">
        </span>
      </div>

      <!-- Desktop: Print & Export Buttons at the End -->
      <div class="hidden md:flex md:gap-1 pb-5">
        <button v-for="action in actions" 
          :key="action.icon" 
          class="w-[48px] h-[48px] flex items-center justify-center rounded-lg shadow-md"
          :class="action.type === 'excel' 
            ? 'bg-gradient-to-b from-[#02A357] to-[#0D7A40]' 
            : 'bg-gradient-to-b from-[#3FA2ED] to-[#1859BD]'">
          <img :src="action.icon" class="w-[22px] h-[22px]">
        </button>
      </div>
    </div>

      <!-- Mobile: Icons and Search aligned properly -->
      <div class="md:hidden flex items-center gap-2 w-full pb-5">
        <div class="flex gap-2">
          <span v-for="icon in headerIcons" :key="icon">
            <img :src="icon" class="w-[28px] h-[28px]">
          </span>
        </div>
 

      <!-- Print & Export Buttons -->
      <button v-for="action in actions" 
        :key="action.icon" 
        class="w-[42px] h-[42px] flex items-center justify-center rounded-md shadow"
        :class="action.type === 'excel' 
          ? 'bg-gradient-to-b from-[#02A357] to-[#0D7A40]' 
          : 'bg-gradient-to-b from-[#3FA2ED] to-[#1859BD]'">
        <img :src="action.icon" class="w-[20px] h-[20px]">
      </button>
    </div>

    <!-- Table with Horizontal Scrolling -->
    <!-- Table with Horizontal Scrolling on Mobile -->
    <div class="table-wrapper overflow-x-auto w-full">
      <div class="table-container min-w-[600px]">
        <Table 
          :headers="headers"
          :items="filteredItems"
          :currentPage="currentPage"
          :itemsPerPage="itemsPerPage"
        />
      </div>
    </div>

     <!-- Bottom Section (Pagination) -->
     <div class="flex flex-col md:flex-row justify-between items-center mt-10">
      <!-- Items per page -->
      <div class="flex items-center gap-2 text-[14px] w-full md:w-[167px] mb-4 md:mb-0">
        <span class="text-[#898D92]">نمایش در هر صفحه</span>
        <div class="relative">
          <select v-model="itemsPerPage" 
            class="border rounded-[17px] text-[13px] w-[48px] h-[31px] px-[8px] py-[8px] shadow-sm focus:outline-none focus:ring focus:border-blue-300 appearance-none">
            <option v-for="option in pageSizeOptions" :key="option" :value="option">
              {{ option }}
            </option>
          </select>
          <span class="absolute left-2 top-1/2 transform -translate-y-1/2 pointer-events-none">
            <img src="/src/assets/Vector.svg" class="w-3 h-3">
          </span>
        </div>
      </div>

      <!-- Pagination -->
      <div class="flex justify-center gap-1 text-[14px] mb-4 md:mb-0">
        <!-- Previous Button -->
        <button @click="changePage(currentPage - 1)" 
          :disabled="currentPage === 1" 
          class="px-1 py-1 border rounded-[8px] flex items-center justify-center w-[32px] h-[32px]"
          :class="{'border-[#148976]': currentPage !== 1, 'border-gray-300 text-[#898D92]': currentPage === 1}">
          <img :src="paginationIcon[1].icon" class="w-5 h-5">
        </button>

        <!-- Page Numbers -->
        <button v-for="page in totalPages" :key="page" @click="changePage(page)" 
          class="px-3 py-1 border rounded-[8px] w-8 h-8 flex items-center justify-center text-[14px]"
          :class="{
            'bg-[#B8DADA] border-[#B8DADA] text-[#148976]': page === currentPage, 
            'border-gray-300 text-gray-500': page !== currentPage
          }">
          {{ page }}
        </button>

        <!-- Next Button -->
        <button @click="changePage(currentPage + 1)" 
          :disabled="currentPage === totalPages" 
          class="px-1 py-1 border rounded-[8px] flex items-center justify-center w-[32px] h-[32px]"
          :class="{'border-[#148976]': currentPage !== totalPages, 'border-[#148976] text-gray-300': currentPage === totalPages}">
          <img :src="paginationIcon[0].icon" class="w-5 h-5">
        </button>
      </div>

      <!-- Pagination Summary -->
      <div class="text-[#898D92] text-[13px]">
        {{ (currentPage - 1) * itemsPerPage + 1 }} - 
        {{ Math.min(currentPage * itemsPerPage, filteredItems.length) }} از {{ filteredItems.length }} مورد
      </div>
    </div>
  </div>
</template>


