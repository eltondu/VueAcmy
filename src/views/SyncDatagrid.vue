<template>
<div class="col-lg-12 control-section">
    <div>
        <ejs-grid :dataSource="data" :allowPaging='true' :pageSettings='pageSettings' :editSettings='editSettings' :toolbar='toolbar'>
            <e-columns>
            <e-column field='OrderID' headerText='Order ID' width='120' textAlign='Right' :isPrimaryKey='true' :validationRules='orderidrules'></e-column>
            <e-column field='CustomerID' headerText='Customer ID' width='120' :validationRules='customeridrules'></e-column>
            <e-column field='Freight' headerText='Freight' width='120' format='C2' textAlign='Right' editType='numericedit' :validationRules='freightrules'></e-column>
            <e-column field='OrderDate' headerText='Order Date' width='130' editType='datepickeredit' format='yMd' textAlign='Right' type='date'></e-column>
            <e-column field='ShipCountry' headerText='Ship Country' width='150' editType='dropdownedit' :edit='editparams'></e-column>
            <e-column field='Verified' headerText='Verified' width='100' editType='booleanedit' displayAsCheckBox='true' type='boolean' textAlign='Center'></e-column>
        </e-columns> 
        </ejs-grid>
    </div>

</div>
</template>
<script lang="ts">
import Vue from "vue";
import { GridPlugin, Edit, Toolbar, Page } from "@syncfusion/ej2-vue-grids";
import  orderDetails  from "../data/sync.json";

Vue.use(GridPlugin);

export default Vue.extend({
  data: () => {
    return {
      data: orderDetails.slice(0),
      editSettings: { allowEditing: true, allowAdding: true, allowDeleting: true, mode: 'Dialog' },
      toolbar: ['Add', 'Edit', 'Delete'],
      orderidrules: { required: true, number: true },
      customeridrules: { required: true },
      freightrules:  { required: true },
      editparams: { params: { popupHeight: '300px' }},
      pageSettings: { pageCount: 5}
    };
  },
  provide: {
      grid: [Edit, Toolbar, Page]
  }
});
</script>