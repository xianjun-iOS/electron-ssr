<template>
  <div class="app-option-field">
    <fieldset>
      <legend>服务器(截图注意打码)</legend>
      <form-item label="服务器 IP">
        <input type="text" v-model="form.host">
      </form-item>
      <form-item label="服务器端口">
        <input type="number" v-model="form.port">
      </form-item>
      <form-item label="本机代理IP">
        <input type="text" v-model="form.localAddr">
      </form-item>
      <form-item label="本机代理端口">
        <input type="number" v-model="form.localPort">
      </form-item>
      <form-item>
        <span slot="label">
          <input type="checkbox" v-model="showPassword">密码
        </span>
        <input v-if="showPassword" type="text" v-model="form.password">
        <input v-else type="password" v-model="form.password">
      </form-item>
      <form-item label="加密">
        <select v-model="form.method">
          <option value="rc4-md5">rc4-md5</option>
          <option value="aes-128-cfb">aes-128-cfb</option>
          <option value="aes-192-cfb">aes-192-cfb</option>
          <option value="aes-256-cfb">aes-256-cfb</option>
          <option value="salsa20">salsa20</option>
          <option value="chacha20">chacha20</option>
          <option value="chacha20-ietf">chacha20-ietf</option>
        </select>
      </form-item>
      <form-item label="协议">
        <select v-model="form.protocol">
          <option value="origin">origin</option>
          <option value="verify_simple">verify_simple</option>
          <option value="verify_deflate">verify_deflate</option>
          <option value="auth_simple">auth_simple</option>
          <option value="auth_sha1">auth_sha1</option>
          <option value="auth_sha1_v2">auth_sha1_v2</option>
        </select>
      </form-item>
      <form-item label="混淆协议">
        <select v-model="form.obfs" @change="form.obfsparam=''">
          <option value="plain">plain</option>
          <option value="http_simple">http_simple</option>
          <option value="tls_simple">tls_simple</option>
          <option value="random_head">random_head</option>
          <option value="tls1.0_session_auth">tls1.0_session_auth</option>
        </select>
      </form-item>
      <form-item label="混淆插件参数">
        <input type="text" v-model="form.obfsparam" :disabled="form.obfs!=='http_simple'">
      </form-item>
      <form-item label="备注">
        <input type="text" v-model="form.remark">
      </form-item>
      <form-item label="链接">
        <input type="text" v-model="ssrLink">
      </form-item>
      <!-- <form-item label="高级选项">
        <span>以下选项不是所有服务端都支持</span>
      </form-item>
      <form-item label="TCP over UDP">
        <span><input type="checkbox" v-model="form.udpport" :true-value="1" :false-value="0">不打勾使用原生 TCP</span>
      </form-item>
      <form-item label="UDP over TCP">
        <span><input type="checkbox" v-model="form.uot" :true-value="1" :false-value="0">不打勾使用原生 UDP</span>
      </form-item> -->
    </fieldset>
  </div>
</template>
<script>
import Config from '../Config'
import FormItem from './FormItem'
import { clone } from '../util'
export default {
  data () {
    return {
      form: new Config(),
      bak: undefined,
      showPassword: false
    }
  },
  computed: {
    ssrLink: {
      get () {
        const link = this.form.getSSRLink()
        this.$emit('config-change', this.form, link)
        return link
      },
      set (val) {
        this.form.setSSRLink(val)
      }
    }
  },
  components: {
    FormItem
  },
  methods: {
    reset () {
      Object.assign(this.form, this.bak)
    },
    setConfig (config) {
      Object.assign(this.form, config)
    }
  },
  created () {
    this.bak = clone(this.form)
  }
}
</script>
<style lang="stylus">
.app-option-field
  fieldset
    padding .5rem
    text-align left
    border 1px solid #aaa
</style>
