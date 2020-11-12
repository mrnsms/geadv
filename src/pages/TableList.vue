<template>
    <div class="row">
      <div class="col-12">
        <card :title="table1.title" :subTitle="table1.subTitle">
          <div slot="raw-content" class="table-responsive">
            <paper-table>
            </paper-table>
          </div>
        </card>
      </div>
    </div>
</template>
<script>
import { PaperTable } from "@/components";
const tableColumns = [];
const tableData = [];

export default {
  components: {
    PaperTable
  },
  data() {
    return {
      table1: {
        title: "",
        subTitle: "",
        columns: [],
        data: []
      },
    };
  }
};
</script>
<style>
</style>
