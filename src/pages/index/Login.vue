<template>
  <div>
    <form>
      <section class="container stu-login">
        <h2 class="title is-2" style="text-align: center">学生登录</h2>
        <div class="columns is-mobile">
          <div class="column is-half is-offset-one-quarter">
            <div class="field">
              <label class="label">用户名</label>
              <div class="control">
                <input class="input" type="number" placeholder="e.g 20151333" v-model="LoginForm.UserId">
              </div>
            </div>
          </div>
        </div>

        <div class="columns is-mobile">
          <div class="column is-half is-offset-one-quarter">
            <div class="field">
              <label class="label">密码</label>
              <div class="control">
                <input class="input" type="password" v-model="LoginForm.Password">
              </div>
            </div>
          </div>
        </div>

        <div class="columns is-mobile">
          <div class="column is-half is-offset-one-quarter">
            <div class="field is-grouped">
              <div class="control">
                <button class="button is-primary" @click="Login">登录</button>
              </div>
              <div class="control">
                <button class="button is-link">
                  <router-link to="/contact">忘记密码？</router-link>
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>
    </form>
  </div>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'

  export default {
    data () {
      return {
        LoginForm: {
          UserId: '',
          Password: ''
        }
      }
    },
    methods: {
      Login () {
        axios({
          method: 'POST',
          url: this.GetApi + 'auth/login',
          data: Qs.stringify({
            UserId: this.LoginForm.UserId,
            Password: this.LoginForm.Password,
            UserType: 'Student'
          }),
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        })
          .then(function (response) {
            if (response.data.message === 'success') {
              this.$cookie.set('UserId', this.LoginForm.UserId, 1)
              this.$cookie.set('UserName', response.data.UserName, 1)
              this.$cookie.set('IndexToken', response.data.Token, 1)
              this.$cookie.set('IndexAuthenticated', true, 1)
              this.$router.replace('/')
              location.reload(true)
              // 参数必须为true，否则会启用浏览器缓存刷新页面
            } else if (response.data.message === 'error') {
              this.$notify.error({
                title: '错误',
                message: '登陆失败'
              })
            }
          }.bind(this))
      }
    },
    computed: {
      GetUserId () {
        return this.$cookie.get('UserId')
      },
      GetIndexToken () {
        return this.$cookie.get('IndexToken')
      },
      GetUserName () {
        return this.$cookie.get('UserName')
      },
      GetIndexAuthenticated () {
        return this.$cookie.get('IndexAuthenticated')
      },
      /**
       * @return {string}
       */
      GetApi () {
        return this.$store.state.Api
      }
    },
    created: function () {
      if (this.$cookie.get('IndexAuthenticated') === 'true') {
        this.$router.push('/')
      }
    }
  }
</script>

<style scoped>
  .stu-login {
    padding-top: 3%;
    padding-bottom: 3%;
  }
</style>
