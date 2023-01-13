<script setup>
import { tableData } from "./mock.js";
import { computed, ref } from "vue";
import { ElConfigProvider } from "element-plus";
import zhCn from "element-plus/lib/locale/lang/zh-cn";

//选项
let options = ref([
  {
    value: true,
    label: "old_channel",
  },
  {
    value: false,
    label: "new_channel",
  },
]);
let old = ref("");

//分类
let finalBili = function (condition) {
  return tableData.filter((item) => item.tag_type === condition);
};
let totalChannel = ref(tableData);
let oldChannel = ref(finalBili("old_channel"));
let newChannel = ref(finalBili("new_channel"));
let curChannel = computed(() => {
  if (old.value === "") {
    return totalChannel.value;
  } else if (old.value) {
    return oldChannel.value;
  } else {
    return newChannel.value;
  }
});

//分页
let thisPage = computed(() =>
  curChannel.value.slice(
    (curPage.value - 1) * pageSize.value,
    curPage.value * pageSize.value
  )
);

let size = computed(() => curChannel.value.length);
let pageSize = ref(10);
let curPage = ref(1);

let handleSizeChange = function (size) {
  pageSize.value = size;
};

let handleCurrentChange = function (cur) {
  curPage.value = cur;
};

//分页记忆
let getRowKeys = (row) => {
  return row.tag_id;
};
</script>

<template>
  <div>
    <!-- 汉化 -->
    <div>
      <el-config-provider :locale="zhCn">
        <router-view />
      </el-config-provider>
    </div>

    <!-- 分类 -->
    <el-select v-model="old" clearable placeholder="请选择">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
      </el-option>
    </el-select>

    <!-- 表格 -->
    <el-table
      ref="multipleTable"
      :data="thisPage"
      border
      stripe
      tooltip-effect="dark"
      :row-key="getRowKeys"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" :reserve-selection="true">
      </el-table-column>
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

    <!-- 分页 -->
    <div class="block">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :page-sizes="[10, 15, 20, 25, 30]"
        layout="total,sizes, prev, pager, next"
        :total="size"
      >
      </el-pagination>
    </div>
  </div>
</template>

<style scoped></style>
