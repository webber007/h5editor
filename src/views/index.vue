<template>
  <div class="g-pg">
    <ly-header class="g-hd s-bg-main">
      <div class="m-lst comp-list" slot="middle">
        <div
          class="itm"
          v-for="(comp, idx) in compLists"
          @click="$store.dispatch('addNewComp', comp.compName)"
          :key="idx">
          {{ comp.name }}
        </div>
        <div class="itm" @click="openMusicDialog">音乐</div>
      </div>

      <div slot="action-btns">
        <el-button type="primary" @click="$store.dispatch('saveUserData')">保存</el-button>
        <el-button type="success" @click="$router.push('/preview')">预览</el-button>
      </div>
    </ly-header>
    <ly-main class="g-mn">

      <prop-manage slot="left" />

      <ly-mobile slot="main">
        <the-container />
      </ly-mobile>

      <page-manage slot="right" />

    </ly-main>
    <!-- 图片选取 -->
    <pick-image />
    <!-- 背景乐选取 -->
    <pick-music />
  </div>
</template>
<script>
export default {
  name: 'index',
  data () {
    return {
      compLists: [
        { name: '图片', compName: 'Image' },
        { name: '文字', compName: 'Text' }
      ]
    }
  },
  methods: {
    openMusicDialog () {
      this.$store.commit('SET_PICK_BGM', {
        status: true,
        callback: (bgm) => {
          this.$store.dispatch('setH5', {
            bgm: bgm.src
          })
        }
      })
    }
  },
  components: {
    lyHeader: () => import('@/layouts/ly-header'),
    lyMain: () => import('@/layouts/ly-main'),
    lyMobile: () => import('@/layouts/ly-mobile'),
    PageManage: () => import('@/containers/page-manage'),
    TheContainer: () => import('@/views/modules/the-container'),
    PropManage: () => import('@/containers/prop-manage'),
    PickImage: () => import('@/containers/pick-image/index'),
    PickMusic: () => import('@/containers/pick-music/index')
  }
}
</script>

<style lang="scss">
  .g-pg {
    display: flex;
    flex-direction: column;
    flex: 1;
    height: 100%;
    > .g-hd {
      padding: 10px 20px;
    }
    > .g-mn {
      flex: 1;
      flex-basis: auto;
    }
  }
  .comp-list {
    text-align: center;
    .itm {
      display: inline-block;
      padding: 10px 30px;
      margin: 0 5px;
      border: 1px solid #dedede;
      color: #fff;
      text-align: center;
      cursor: pointer;
    }
  }
</style>
