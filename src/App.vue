<script setup>
import { tableData } from "./mock.js";
import { ref } from "vue";

let bilbili = ref(tableData);
let options = ref([
  {
    value: 1,
    label: "old_channel",
  },
  {
    value: 2,
    label: "new_channel",
  },
]);
let oldOrNew = ref();
</script>

<template>
  <div>
    <el-select v-model="oldOrNew" placeholder="请选择">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
      </el-option>
    </el-select>

    <el-table
      ref="multipleTable"
      :data="bilbili"
      border="true"
      stripe="true"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55"> </el-table-column>
      <el-table-column prop="tag_name" label="名称" width="150">
      </el-table-column>
      <el-table-column prop="tag_id" label="TagID" width="150">
      </el-table-column>
      <el-table-column prop="tag_type" label="类型" width="150">
      </el-table-column>
      <el-table-column prop="subscribed_count" label="订阅数" width="100">
      </el-table-column>
      <el-table-column prop="archive_count" label="投稿数" width="100">
      </el-table-column>
      <el-table-column prop="featured_count" label="点赞数" width="180">
      </el-table-column>
      <el-table-column
        prop="short_content"
        label="简述"
        width="180"
        show-overflow-tooltip
      >
      </el-table-column>
      <el-table-column
        prop="content"
        label="详述"
        width="180"
        show-overflow-tooltip
      >
      </el-table-column>
    </el-table>

    <div class="block">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[10, 15, 20, 25, 30]"
        :page-size="10"
        layout="sizes, prev, pager, next"
        :total="100"
      >
      </el-pagination>
    </div>
  </div>
</template>

<style scoped></style>
