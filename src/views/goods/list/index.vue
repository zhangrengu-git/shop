<template>
  <div class="container">
    <!-- 头部分类 -->
    <Header></Header>

    <div class="fun">
      <el-form :inline="true" size="small">
        <el-form-item v-if="tabPane !== 'delete'">
          <el-button icon="el-icon-plus" @click="handleCreate"
            >新增商品</el-button
          >
        </el-form-item>

        <el-form-item v-if="tabPane === 'stock'">
          <el-button icon="el-icon-top" @click="handleStatus(null, 1)"
            >上架</el-button
          >
        </el-form-item>

        <el-form-item v-if="tabPane === 'sale'">
          <el-button icon="el-icon-bottom" @click="handleStatus(null, 0)"
            >下架</el-button
          >
        </el-form-item>

        <el-form-item v-if="tabPane !== 'delete'">
          <el-dropdown placement="bottom" :show-timeout="50">
            <el-button>
              <i class="el-icon-medal" />
              <span>推荐</span>
              <i class="el-icon-arrow-down cs-pl-5" />
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item @click.native="handleRecommend(null, 1)"
                >设为推荐</el-dropdown-item
              >
              <el-dropdown-item @click.native="handleRecommend(null, 0)"
                >取消推荐</el-dropdown-item
              >
            </el-dropdown-menu>
          </el-dropdown>
        </el-form-item>

        <el-form-item v-if="tabPane !== 'delete'">
          <el-dropdown placement="bottom" :show-timeout="50">
            <el-button>
              <i class="el-icon-star-off" />
              <span>新品</span>
              <i class="el-icon-arrow-down cs-pl-5" />
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item @click.native="handleNew(null, 1)"
                >设为新品</el-dropdown-item
              >
              <el-dropdown-item @click.native="handleNew(null, 0)"
                >取消新品</el-dropdown-item
              >
            </el-dropdown-menu>
          </el-dropdown>
        </el-form-item>

        <el-form-item v-if="tabPane !== 'delete'">
          <el-dropdown placement="bottom" :show-timeout="50">
            <el-button>
              <i class="el-icon-sunny" />
              <span>热卖</span>
              <i class="el-icon-arrow-down cs-pl-5" />
            </el-button>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item @click.native="handleHot(null, 1)"
                >设为热卖</el-dropdown-item
              >
              <el-dropdown-item @click.native="handleHot(null, 0)"
                >取消热卖</el-dropdown-item
              >
            </el-dropdown-menu>
          </el-dropdown>
        </el-form-item>

        <el-form-item>
          <el-button-group>
            <el-button icon="el-icon-delete" @click="handleDelete(null, true)">
              <span>{{ tabPane === "delete" ? "彻底删除" : "删除" }}</span>
            </el-button>

            <el-button
              v-if="tabPane === 'delete'"
              icon="el-icon-refresh-left"
              @click="handleDelete(null, false)"
              >恢复</el-button
            >
          </el-button-group>
        </el-form-item>
      </el-form>
    </div>

    <div class="goods_list">
      <el-tabs v-model="tabPane" @tab-click="handleTabsEdit">
        <el-tab-pane
          :key="index"
          v-for="(item, index) in tabList"
          :label="item"
          :name="index"
        >
          <el-table
            ref="multipleTable"
            :data="tableData"
            tooltip-effect="dark"
            style="width: 100%"
            @selection-change="handleSelectionChange"
          >
            <el-table-column type="selection" width="55"> </el-table-column>
            <el-table-column label="商品" sortable="custom" min-width="150">
              <template slot-scope="scope">
                <el-image
                  class="goods-image"
                  @click="handleView(scope.row.goods_id)"
                  :src="img"
                  fit="contain"
                  lazy
                />
                <div class="goods_info">
                  <div class="name">{{ scope.row.date }}</div>
                  <div class="des">这是物品的描述</div>
                  <div class="ind">物品介绍</div>
                </div>
              </template>
            </el-table-column>

            <el-table-column
              sortable="custom"
              prop="name"
              label="本店价"
              width="120"
            >
            </el-table-column>

            <el-table-column
              sortable="custom"
              prop="address"
              label="库存"
              show-overflow-tooltip
            >
            </el-table-column>

            <el-table-column
              sortable="custom"
              prop="address"
              label="总销量"
              show-overflow-tooltip
            >
            </el-table-column>

            <el-table-column
              sortable="custom"
              prop="address"
              label="排序值"
              align="center"
              show-overflow-tooltip
            >
              <template slot-scope="scope">
                <el-input-number
                  v-if="tabPane !== 'delete'"
                  v-model="scope.row.sort"
                  style="width: 88px"
                  size="mini"
                  controls-position="right"
                  :min="0"
                  :max="255"
                  @change="handleSort(scope.$index)"
                >
                </el-input-number>
                <span v-else>
                  {{ scope.row.sort }}
                </span>
              </template>
            </el-table-column>

            <el-table-column label="操作" align="center">
              <template slot-scope="scope">
                <div class="goods-text">
                  <p v-if="tabPane !== 'delete'">
                    <el-link
                      class="button"
                      type="primary"
                      @click="handleEdit(scope.row.goods_id)"
                      :underline="false"
                      >编辑商品</el-link
                    >
                  </p>

                  <p v-if="tabPane !== 'delete'">
                    <el-link
                      class="button"
                      type="primary"
                      @click="handleCopy(scope.row.goods_id)"
                      :underline="false"
                      >复制商品</el-link
                    >
                  </p>

                  <p v-if="tabPane !== 'delete'">
                    <el-link
                      class="button"
                      type="primary"
                      @click="
                        handleStatus(scope.$index, Number(!scope.row.status))
                      "
                      :underline="false"
                      >{{ scope.row.status ? "下架" : "上架" }}</el-link
                    >
                  </p>

                  <p>
                    <el-link
                      class="button"
                      type="primary"
                      @click="handleDelete(scope.$index, true)"
                      :underline="false"
                      >{{ tabPane === "delete" ? "彻底删除" : "删除" }}</el-link
                    >
                  </p>

                  <p v-if="tabPane === 'delete'">
                    <el-link
                      class="button"
                      type="primary"
                      @click="handleDelete(scope.$index, false)"
                      :underline="false"
                      >恢复</el-link
                    >
                  </p>
                </div>
              </template>
            </el-table-column>
          </el-table>
        </el-tab-pane>
      </el-tabs>
    </div>

    <!-- 翻页 -->
    <page-footer
      slot="footer"
      :loading="loading"
      :page-no="page.page_no"
      :page-size="page.page_size"
      :total="pageTotal"
      @change="handlePaginationChange"
    />
  </div>
</template>

<script>
import Header from "./components/header.vue";
import PageFooter from "../../components/page-footer/index.vue";
export default {
  components: {
    Header,
    PageFooter,
  },
  data() {
    return {
      tabPane: "sale",
      tabList: { sale: "出售中", stock: "已下架", delete: "回收站" },
      editableTabsValue: "2",
      editableTabs: [
        {
          title: "Tab 1",
          name: "1",
          content: "Tab 1 content",
        },
        {
          title: "Tab 2",
          name: "2",
          content: "Tab 2 content",
        },
      ],
      tabIndex: 2,
      tableData: [
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-08",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-06",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-07",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄",
        },
      ],
      multipleSelection: [],
      img: "https://demo.careyshop.cn/api/v1/storage.html?method=get.storage.thumb&code=goods_image_x480&url=aliyun.oss.careyshop.cn%2Fuploads%2Ffiles%2F20191230%2F32dcbebd-ebad-4d41-96d4-196fd7fe9ac7.jpg%3Ftype%3Daliyun",
      page: {
        page_no: 1,
        page_size: 0,
      },
      pageTotal: 0,
    };
  },
  created() {},
  methods: {
    // 新增商品
    handleCreate() {
      console.log("3333");
    },
    // 处理上架下架
    handleStatus() {
      console.log("333");
    },
    //设置为推荐
    handleRecommend() {},
    //设置为新品
    handleNew() {},
    //设置为热卖
    handleHot() {},
    // 删除
    handleDelete() {},
    //tab点击事件
    handleTabsEdit(tab) {
      let config = { status: 1, is_delete: 0 };
      switch (tab.name) {
        case "stock":
          config.status = 0;
          break;

        case "delete":
          config.is_delete = 1;
          break;
      }
    },
    // 选中数据项
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    // 编辑商品
    handleEdit() {},
    //复制商品
    handleCopy() {},
    //上下架商品
    handleStatus() {},
    //删除or回复商品
    handleDelete() {},
    //排序
    handleSort() {},
    handlePaginationChange(val) {
      this.page = val;
      if ((val.page_no - 1) * val.page_size > this.pageTotal) {
        return;
      }

      this.$nextTick(() => {
        this.$refs.header.handleFormSubmit();
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  padding-bottom: 53px;
}
.fun {
  padding: 20px;
  padding-bottom: 0;
  border-bottom: 1px solid #ccc;
}
.goods_list {
  padding: 20px;
}
.goods-text {
  p {
    margin: 0;
  }

  .button {
    padding: 0;
    font-size: 13px;
  }
}
.goods-image {
  float: left;
  width: 150px;
  height: 150px;
}
.goods_info {
  float: left;
  margin-top: 30px;
  margin-left: 10px;
  .name {
    font-size: 18px;
  }
}
</style>