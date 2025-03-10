<template>
    <div class="overflow-x-auto drop-shadow-[0_4px_6px_rgba(0,0,0,0.25)] ">
      <table class="w-[952px] h-[32px] border-collapse border text-center">
        <thead>
          <tr>
            <th class="border w-[105px] h-[32px]">انتخاب</th>
            <th v-for="header in headers" 
                :key="header.key" 
                class=" border w-[105px] h-[32px]">
              {{ header.label }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in paginatedItems" 
              :key="item.id" 
              class="odd:bg-gray-100">
            <td class="border w-[105px] h-[32px]">
              <input type="checkbox" v-model="selectedRows" :value="item.id">
            </td>
            <td v-for="header in headers" 
                :key="header.key" 
                class="border w-[105px] h-[32px]">
              {{ convertToPersian(item[header.key]) }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>
  
  <script>
  export default {
    props: {
      headers: Array, // Table Headers
      items: Array,   // Table Data (Dynamic)
      currentPage: Number,
      itemsPerPage: Number
    },
    data() {
      return {
        selectedRows: [] // Stores selected row IDs
      };
    },
    computed: {
      paginatedItems() {
        const start = (this.currentPage - 1) * this.itemsPerPage;
        return this.items.slice(start, start + this.itemsPerPage);
      }
    },
    methods: {
      convertToPersian(value) {
        if (typeof value === "number" || typeof value === "string") {
          return value.toString().replace(/\d/g, (digit) => "۰۱۲۳۴۵۶۷۸۹"[digit]);
        }
        return value;
      },
      toggleSelectAll(event) {
        if (event.target.checked) {
          this.selectedRows = this.paginatedItems.map(item => item.id);
        } else {
          this.selectedRows = [];
        }
      }
    }
  };
  </script>
  