<template>
  <div class="v-notifications">
    <nav-menu></nav-menu>
    <user-setting-menu></user-setting-menu>

    <div class="ui grid" v-show="notifications.length">
      <div class="four wide column"></div>

      <div class="eight wide column">
        <div class="ui segment">
          <div class="ui minimal comments">
            <h3 class="ui dividing header">通知</h3>
            <div class="comment" v-for="notification in notifications">
              <a class="avatar" v-link="{path: '/users/' + notification.from._id}">
                <img :src="notification.from.avatar">
              </a>

              <div class="content" v-if="notification.type === 'comment' && notification.poem">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">评论了你的诗<a v-link="{path:'/poems/' + notification.poem._id}">《{{notification.poem.title}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'comment' && notification.poetry">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">评论了你的诗集<a v-link="{path:'/poetries/' + notification.poetry._id}">《{{notification.poetry.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'comment' && notification.group">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">评论了你的诗社<a v-link="{path:'/groups/' + notification.group._id}">《{{notification.group.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'collect'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">收藏了你的诗<a v-link="{path:'/poems/' + notification.poem._id}">《{{notification.poem.title}}》</a>到诗集<a v-link="{path:'/poetries/' + notification.poetry._id}">《{{notification.poetry.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'follow'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">关注了你</div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'joinToGroup'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">申请加入诗社<a v-link="{path: '/groups/' + notification.group._id}">《{{notification.group.name}}》</a>。<a href="#" @click.prevent="agreeJoinGroup(notification.group._id, notification.from._id)">同意</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'removeFromGroup'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">管理员将你移出诗社<a v-link="{path: '/groups/' + notification.group._id}">《{{notification.group.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'joinedGroup'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">你已经加入诗社<a v-link="{path: '/groups/' + notification.group._id}">《{{notification.group.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>

              <div class="content" v-if="notification.type === 'leaveFromGroup'">
                <a class="author" v-link="{path: '/users/' + notification.from._id}">{{notification.from.name}}</a>
                <div class="metadata">
                  <span class="date">{{notification._id | idToFromNow}}</span><span v-if="!notification.hasRead" class="notRead">(未读)</span>
                </div>
                <div class="text">退出了诗社<a v-link="{path: '/groups/' + notification.group._id}">《{{notification.group.name}}》</a></div>
                <div class="actions">
                  <a class="reply" @click.prevent="deleteNotification(notification._id)">删除</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <user-side-menu :user="user"></user-side-menu>
  </div>
</template>

<style lang="stylus">
@import "../variables.styl"

.v-notifications
  width $width
  margin 0 auto
  a
    color $themeColor
  .notRead
    color $themeColor
  .reply
    font-size 12px
    color #999
</style>

<script>
import API from '../services/API'
import User from '../mixins/User'
import LoadMore from '../mixins/LoadMore'

export default {
  mixins: [User, LoadMore],
  data() {
    return {
      user: {},
      notifications: []
    }
  },
  route: {
    data() {
      const uid = this.$route.params.uid
      return {
        user: API.getUser(uid),
        notifications: API.getNotifications()
      }
    }
  },
  methods: {
    loadMore: function () {
      const notifications = this.notifications
      const limit = 50
      if (!notifications.length) return
      return API.getNotifications(limit, notifications[notifications.length - 1]._id).then(_notifications => {
        this.notifications = notifications.concat(_notifications)
        if (_notifications.length === limit) {
          this.loading = false
        }
      })
    },
    deleteNotification: function (notificationId) {
      return API.deleteNotification(notificationId).then(() => {
        this.notifications = this.notifications.filter(notification => {
          return notification._id !== notificationId
        })
      })
    },
    agreeJoinGroup: API.agreeJoinGroup
  },
  components: {
    'UserSettingMenu': require('../components/User-Setting-Menu.vue'),
    'NavMenu': require('../components/Nav-Menu.vue'),
    'UserSideMenu': require('../components/User-Side-Menu.vue')
  }
}
</script>