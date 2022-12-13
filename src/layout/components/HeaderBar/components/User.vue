<template>
  <div class="user-container">
    <el-dropdown trigger="click" style="height: 100%; line-height: 0">
      <div class="avatar-wrapper">
        <el-image :src="user.avatar" class="user-avatar" alt="头像">
          <div slot="error">
            <el-image :src="require('@/assets/default.png')" class="user-avatar" alt="头像" />
          </div>
        </el-image>
        <span class="username">{{ user.userName }}</span>
        <i class="el-icon-caret-bottom" />
      </div>
      <el-dropdown-menu slot="dropdown" class="user-dropdown">
        <el-dropdown-item @click.prevent.native="openSettingsDrawer" icon="el-icon-setting" v-if="showSettings">
          <span>My Settings</span>
        </el-dropdown-item>
        <el-dropdown-item divided @click.prevent.native="logout" icon="el-icon-back">
          <span>Log Out</span>
        </el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <global-settings ref="globalSettings" v-if="showSettings" />
  </div>
</template>

<script lang="ts">
import { SettingsModule } from "@/store/modules/settings";
import { UserModule } from "@/store/modules/user";
import { Component, Vue } from "vue-property-decorator";
import GlobalSettings from "./GlobalSettings.vue";

@Component({
  components: { GlobalSettings },
})
export default class User extends Vue {
  get user() {
    return UserModule.userInfo;
  }

  get showSettings() {
    return SettingsModule.showSettings;
  }

  public openSettingsDrawer() {
    (this.$refs.globalSettings as any).openSettingsDrawer();
  }

  public async logout() {
    await UserModule.LogOut();
    // this.$router.push(`/login?redirect=${this.$route.fullPath}`).catch(err => {
    //   console.warn(err)
    // })
  }
}
</script>

<style lang="scss" scoped>
.user-container {
  line-height: 0;
  .avatar-wrapper {
    position: relative;
    height: 100%;
    display: flex;
    align-items: center;
    .user-avatar {
      cursor: pointer;
      width: 35px;
      height: 35px;
      border-radius: 50%;
    }

    .username {
      line-height: 0px;
      display: inline-block;
      margin: 0 7px 0 9px;
      font-size: 14px;
      cursor: pointer;
      user-select: none;
    }

    .el-icon-caret-bottom {
      cursor: pointer;
      font-size: 12px;
    }
  }
}
</style>
<style lang="scss">
.user-dropdown {
  .message-badge {
    .is-dot {
      top: 7px;
      right: 17px !important;
    }
  }
}
</style>
