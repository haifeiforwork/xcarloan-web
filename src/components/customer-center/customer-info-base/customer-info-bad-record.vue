<!--黑白灰名单-->
<template>
  <section class="component customer-info-bad-record">
    <data-box :columns="columns" :data="dataSet" :height="440" ref="databox"></data-box>
  </section>
</template>

<script lang="ts">
import Vue from 'vue';
import Component from 'vue-class-component'
import { Prop } from "vue-property-decorator";
import { Dependencies } from "~/core/decorator";
import { BasicCustomerCenterService } from "~/services/manage-service/basic-customer-center.service";

@Component({})
export default class CustomerInfoBadRecord extends Vue {
  @Dependencies(BasicCustomerCenterService) private basicCustomerCenterService: BasicCustomerCenterService;

  @Prop() id: Number
  private dataSet: Array<any> = [];
  private columns: Array<any> = []

  created() {
    this.columns = [
      {
        align: "center",
        title: "客户名",
        key: "customerName",
        minWidth: this.$common.getColumnWidth(4)
      },
      {
        align: "center",
        title: "性别",
        key: "customerSex",
        minWidth: this.$common.getColumnWidth(4),
        render: (h, { row }) => h('p', {}, this.$filter.dictConvert(row.customerSex))
      },
      {
        align: "center",
        title: "手机号",
        key: "customerPhone",
        minWidth: this.$common.getColumnWidth(4)
      },
      {
        align: "center",
        title: "记录类型",
        key: "blacklistType",
        minWidth: this.$common.getColumnWidth(4),
        render: (h, { row }) => h('p', {}, this.$filter.dictConvert(row.blacklistType))
      },
      {
        align: "center",
        title: '开始时间',
        key: 'beginTime',
        minWidth: this.$common.getColumnWidth(6),
        render: (h, { row }) => h('p', {}, this.$filter.dateFormat(row.blacklistType))
      },
      {
        align: "center",
        title: '结束时间',
        key: 'endTime',
        minWidth: this.$common.getColumnWidth(6),
        render: (h, { row }) => h('p', {}, this.$filter.dateFormat(row.blacklistType))
      },
      {
        align: "center",
        title: '备注',
        key: 'remark',
        minWidth: this.$common.getColumnWidth(5)
      }
    ]
  }

  mounted() {
    this.basicCustomerCenterService.findCustomerBlackListRecord(this.id).subscribe(
      data => this.dataSet = data,
      err => this.$Message.error(err.msg)
    )
  }

}
</script>
<style lang="less" scoped>
</style>
