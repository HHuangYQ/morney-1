<template>
  <div>
    <layout class-prefix="layout">
      <NumberPad :value.sync="record.amount" @submit="saveRecord" />
      <Tabs :data-source="recordTypeList"
      :value.sync="record.type"/>
      <div class="notes">
        <FormItem
          :value.sync="record.notes"
          field-name="备注"
          placeholder="在这里输入备注"
        />
      </div>
      <div class="createdAt">
        <FormItem
          :value.sync="record.createdAt"
          field-name="日期"
          placeholder="在这里输入日期"
          type="date"
        />
      </div>
      
      
      <Tags @update:value="record.tags = $event" />
    </layout>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import NumberPad from "@/components/Money/NumberPad.vue";
import FormItem from "@/components/Money/FormItem.vue";
import Tags from "@/components/Money/Tags.vue";
import { Component } from "vue-property-decorator";
import Tabs from '@/components/Tabs.vue';
import recordTypeList from '@/constants/recordTypeList';

@Component({
  components: {Tabs, Tags, FormItem,  NumberPad },
   
})
export default class Money extends Vue {

   get recordList() {
      return this.$store.state.recordList;
    }
    recordTypeList = recordTypeList;
  record: RecordItem = {
    tags: [],
    notes: "",
    type: "-",
    amount: 0,
    createdAt:new Date().toISOString(),
  };
  created() {
    this.$store.commit('fetchRecords')
  }

  onUpdateNotes(value: string) {
    this.record.notes = value;
  }

  saveRecord() {
    if(!this.record.tags || this.record.tags.length===0){
      return window.alert('请最少选择一个标签')
    }
    this.$store.commit("createRecord", this.record);
    if(this.$store.state.createRecordError === null){
          window.alert('已保存');
          this.record.notes = '';
    }
  }
}
</script>
<style lang="scss" scoped>
  ::v-deep .layout-content{
    display: flex;
    flex-direction: column-reverse;
}
.notes {
  padding: 12px 0;
}
</style>
