<template>
  <div class="footer">
    <el-pagination
      :current-page="pageNo || 1"
      :page-size="pageSize || 25"
      :total="total"
      :page-sizes="sizes"
      :disabled="loading"
      :layout="sizes.includes(pageSize) && isSize ? layout : simple"
      style="margin: -10px -5px"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    >
    </el-pagination>
  </div>
</template>

<script>
import { mapActions } from "vuex";

export default {
  name: "page-footer",
  props: {
    pageNo: {
      default: 0,
    },
    pageSize: {
      default: 0,
    },
    total: {
      default: 0,
    },
    loading: {
      default: false,
    },
    isSize: {
      default: true,
    },
  },
  data() {
    return {
      sizes: [10, 25, 50, 100, 250, 500],
      layout: "total, sizes, prev, pager, next, jumper",
      simple: "total, prev, pager, next, jumper",
    };
  },
  methods: {
    // ...mapActions("careyshop/db", ["databasePage"]),
    handleSizeChange(val) {
      if (!this.isSize) {
        return;
      }

      //   this.databasePage({ user: true }).then((res) => {
      //     res.set("size", val).write();
      //   });

      this.$emit("change", {
        page_no: this.pageNo,
        page_size: val,
      });
    },
    handleCurrentChange(val) {
      this.$emit("change", {
        page_no: val,
        page_size: this.pageSize,
      });
    },
  },
};
</script>
<style lang="scss" scoped>
.footer {
  position: fixed;
  left: 0;
  bottom: 0;
  height: 53px;
  width: 100%;
  padding-top: 20px;
  padding-left: 80px;
  background-color: #fff;
  border-top: 1px solid #ccc;
  z-index: 99;
}
</style>
