<template>
  <el-row>
    <el-col :span="8" v-for="(o, index) in jobs" :key="o" :offset="index > 0 ? 2 : 0">
      <el-card class="job-card">
        <img :src="image" class="job-image" />
        <div style="padding: 14px">
          <span>{{ o.workContent }}</span>
          <div class="bottom">
            <!-- <time class="time">月结-观澜京东</time> -->
            <el-button class="button like" type="success" @click="apply(o)">应聘</el-button>
          </div>
        </div>
      </el-card>
    </el-col>
  </el-row>
</template>

<script lang="ts">
import image from '@/assets/image/job.jpg';
import { defineComponent, computed, ref, reactive, onMounted } from 'vue';
import { likedPositionApi, applyPositionApi } from '../../api/app';
import { ElMessage } from 'element-plus';

export default defineComponent({
  name: 'Login',
  setup() {
    const jobs = ref([]);

    function loadData() {
      likedPositionApi().then((res) => {
        jobs.value = res.data;
      });
    }

    onMounted(() => {
      loadData();
    });

    function apply(row: any) {
      applyPositionApi(row.id).then((res) => {
        ElMessage({
          message: '应聘申请成功',
          duration: 3000,
          type: 'success',
        });
      });
    }

    return {
      image,
      jobs,
      apply,
    };
  },
});
</script>

<style scoped>
.like {
  float: right;
}
.job-image {
  height: 100%;
  width: 100%;
}
.job-card {
  height: 300px;
}
.bottom {
  padding-bottom: 25px;
}
</style>