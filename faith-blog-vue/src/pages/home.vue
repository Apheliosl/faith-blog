<template>
  <div class="home">
    <el-row
      id="artList"
      type="flex"
      justify="space-around"
    >
      <el-col :span="16">

        <el-row
          class="art-item"
          v-for="blog in blogs"
          :key="blog"
        >
          <el-card shadow="hover">
            <h5>
              <router-link
                :to="{
                  name: 'article',
                  params: {blogId:blog.id}
                }"
                tag="span"
                class="art-title"
              >{{ blog.title }}

              </router-link>
            </h5>
            <el-row class="art-info d-flex align-items-center justify-content-start">
              <div
                class="art-time"
                v-format="'YYYY年MM月DD日'"
              ><i class="el-icon-time"></i> {{ $t('home.time') }}{{blog.created | moment}}</div>
              <div class="art-time"><i class="fa fa-eye"></i> {{ blog.views }}{{ $t('home.views') }}</div>
              <div class="art-time"><i class="fa fa-tags"> </i> 
                 <el-tag size="mini"> swagger2</el-tag>
              </div>
            </el-row>
            <el-row class="art-body">
              <div class="side-img hidden-sm-and-down"><img
                  class="art-banner"
                  src="https://api.ixiaowai.cn/mcapi/mcapi.php"
                ></div>
              <div class="side-abstract">
                <div
                  style="text-indent:2em"
                  class="art-abstract"
                >
                  {{ blog.description }}
                </div>
                <div class="art-more">
                  <router-link
                    :to="{
                  name: 'article',
                  params: {blogId:blog.id}
                }"
                    tag="span"
                  >
                    <el-button plain>{{$t('home.readMore')}}</el-button>
                  </router-link>
                </div>
              </div>
            </el-row>
          </el-card>
          <img
            v-show="blog.top==1"
            class="star"
            src="../assets/images/top.png"
          />
        </el-row>

        <div class="block pagination">
          <el-pagination
            background
            layout="prev, pager, next"
            :current-page="currentPage"
            :page-size="pageSize"
            :total="total"
            @current-change=page
          >
          </el-pagination>
        </div>

      </el-col>
      <el-col
        :span="5"
        class="hidden-sm-and-down"
        id="side"
      >
        <div class="item">
          <aboutme></aboutme>
        </div>
        <div class="item">
          <tag></tag>
        </div>
        <div class="item">
          <statistics></statistics>
        </div>
        <div class="item">
          <friend></friend>
        </div>
      </el-col>
    </el-row>

  </div>
</template>

<script>
/**
 * 博客首页
 * 接口地址：
 *       - 文章分页 http://127.0.0.1:8080/blogs/{currentPage}
 */

import aboutme from "../components/aboutme";
import friend from "../components/friend";
import tag from "../components/tag";
import statistics from "../components/statistics";
export default {
  name: "home",
  components: {
    aboutme,
    friend,
    statistics,
    tag,
  },
  data() {
    return {
      blogs: {},
      currentPage: 1,
      total: 0,
      pageSize: 10,
    };
  },
  methods: {
    page(currentPage) {
      this.$axios
        .get("/blogs/" + currentPage)
        .then((res) => {
          this.blogs = res.data.data.records;
          this.currentPage = res.data.data.current;
          this.total = res.data.data.total;
          this.pageSize = res.data.data.size;
        })
        .catch((err) => {
          console.error(err);
        });
    },
  },
  created() {
    this.page(1);
  },
};
</script>

<style scoped>
.art-info {
  font-size: 14px;
}
#side .item {
  margin-bottom: 30px;
}

.art-item {
  margin-bottom: 30px;
  position: relative;
}

.art-item .star {
  width: 60px;
  height: 60px;
  position: absolute;
  top: 0;
  right: 0;
}

img.tag {
  width: 16px;
  height: 16px;
}

.art-title {
  border-left: 3px solid #f56c6c;
  padding-left: 5px;
  cursor: pointer;
}

.art-title:hover {
  padding-left: 10px;
  color: #409eff;
}

.art-time {
  margin-right: 20px;
}

.art-body {
  display: flex;
  padding: 10px 0;
}

.side-img {
  height: 150px;
  width: 270px;
  overflow: hidden;
  margin-right: 10px;
}

img.art-banner {
  width: 100%;
  height: 100%;
  transition: all 0.6s;
}

img.art-banner:hover {
  transform: scale(1.4);
}

.side-abstract {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.art-abstract {
  flex: 1;
  color: #aaa;
}

.art-more {
  height: 40px;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.art-more .view {
  color: #aaa;
}
h5 {
  font-size: 18px;
}
.pagination {
  background-color: #f9f9f9;
}
</style>