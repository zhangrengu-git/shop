<template>
  <div class="container">
    <el-form :inline="true" size="small">
      <el-form-item v-if="tabPane !== 'delete'">
        <el-button icon="el-icon-plus" @click="handleCreate">新增商品</el-button>
      </el-form-item>

      <!-- <el-form-item v-if="tabPane === 'stock'">
        <el-button icon="el-icon-top" @click="handleStatus(null, 1)">上架</el-button>
      </el-form-item>

      <el-form-item v-if="tabPane === 'sale'">
        <el-button icon="el-icon-bottom" @click="handleStatus(null, 0)">下架</el-button>
      </el-form-item>

      <el-form-item v-if="tabPane !== 'delete'">
        <el-dropdown placement="bottom" :show-timeout="50">
          <el-button>
            <i class="el-icon-medal" />
            <span>推荐</span>
            <i class="el-icon-arrow-down cs-pl-5" />
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item @click.native="handleRecommend(null, 1)">设为推荐</el-dropdown-item>
            <el-dropdown-item @click.native="handleRecommend(null, 0)">取消推荐</el-dropdown-item>
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
            <el-dropdown-item @click.native="handleNew(null, 1)">设为新品</el-dropdown-item>
            <el-dropdown-item @click.native="handleNew(null, 0)">取消新品</el-dropdown-item>
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
            <el-dropdown-item @click.native="handleHot(null, 1)">设为热卖</el-dropdown-item>
            <el-dropdown-item @click.native="handleHot(null, 0)">取消热卖</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-form-item>

      <el-form-item>
        <el-button-group>
          <el-button
            v-if="auth.del"
            icon="el-icon-delete"
          
            @click="handleDelete(null, true)"
          >
            <span>{{ tabPane === 'delete' ? '彻底删除' : '删除' }}</span>
          </el-button>

          <el-button
            v-if="tabPane === 'delete'"
            icon="el-icon-refresh-left"
          
            @click="handleDelete(null, false)"
          >恢复</el-button>
        </el-button-group>
      </el-form-item>-->
    </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loading: false,
      tabPane: 'sale',
      tabList: { sale: '出售中', stock: '已下架', delete: '回收站' },
    }
  },
  created () {

  },
  methods: {
    // 新增商品
    handleCreate () {
      console.log('3333');
    },
    // 处理上架下架
    handleStatus () {
      console.log('333');
    },
    //设置为推荐
    handleRecommend () {

    },
    //设置为新品
    handleNew () {

    },
    //设置为热卖
    handleHot () {

    },
    // 删除
    handleDelete () {

    }
  }
}
</script>

<style>
</style>