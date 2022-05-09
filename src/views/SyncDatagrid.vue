<template>
<div class="col-lg-12 control-section">
    <div class="m-4">
        <ejs-grid :dataSource="data" :allowPaging='true' :pageSettings='pageSettings' :editSettings='editSettings' :toolbar='toolbar'>
            <e-columns>
            <e-column field='lastName' headerText='Last Name' width='120'></e-column>
            <e-column field='firstName' headerText='First Name' width='120'></e-column>
            <e-column field='id' headerText='Id' width='120' :isPrimaryKey='true'></e-column>
            <e-column field='idBook' headerText='Book Id' width='130' editType='numeric'></e-column>                        
        </e-columns> 
        </ejs-grid>
    </div>
</div>
</template>
<script lang="ts">
import Vue from "vue";
import { GridPlugin, Edit, Toolbar, Page } from "@syncfusion/ej2-vue-grids";

import axios from "axios"

Vue.use(GridPlugin);

export default Vue.extend({
  data: () => {
    return {    
      isBusy: true,
      data: [],
      editSettings: { allowEditing: true, allowAdding: true, allowDeleting: true, mode: 'Dialog' },
      toolbar: ['Add', 'Edit', 'Delete'],
      editparams: { params: { popupHeight: '300px' }},
      pageSettings: { pageCount: 5},
      totalRows: 1,
    };
  },
    mounted() {
    axios
      .get("https://fakerestapi.azurewebsites.net/api/v1/Authors")
      .then((res) => {
        var before = Date.now();
        var elapsed = Date.now() - before;
        setTimeout(() => {
          this.data = res.data;          
          this.isBusy = !this.isBusy;
          this.totalRows = this.orderDetails.length;
        }, elapsed);
      })
      .catch((error) => {
        console.error(error);
      });
  },
  provide: {
      grid: [Edit, Toolbar, Page]
  }
});
</script>