<template>
  <van-cell class="comment-item">
    <van-image
      slot="icon"
      class="avatar"
      round
      fit="cover"
      :loading="loading"
      :src="comment.aut_photo"
    />
    <div slot="title" class="title-wrap">
      <div class="user-name">{{ comment.aut_name }}</div>
      <van-button
        @click="onCommentLike"
        class="like-btn"
        :class="{ active: comment.is_liking }"
        :icon="comment.is_liking ? 'good-job' : 'good-job-o'"
      >
        {{ comment.like_count || '赞' }}
      </van-button>
    </div>

    <div slot="label">
      <p class="comment-content">{{ comment.content }}</p>
      <div class="bottom-info">
        <span class="comment-pubdate">{{
          comment.pubdate | relativeTime
        }}</span>
        <van-button
          @click="$emit('reply-click', comment)"
          class="reply-btn"
          round
          >回复{{ comment.reply_count }}</van-button
        >
      </div>
    </div>
  </van-cell>
</template>

<script>
import { addCommentLike, deleteCommentLike } from '@/api/comment'
export default {
  name: 'CommentItem',
  components: {},
  props: {
    comment: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      loading: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    async onCommentLike () {
      this.loading = true
      try {
        if (this.comment.is_liking) {
          await deleteCommentLike(this.comment.com_id)
          this.comment.like_count--
        } else {
          await addCommentLike(this.comment.com_id)
          this.comment.like_count++
        }
        this.loading = false
        this.comment.is_liking = !this.comment.is_liking
        this.$toast('操作成功')
      } catch (e) {
        this.loading = false
      }
    }
  }
}
</script>

<style scoped lang="less">
.comment-item {
  .like-btn {
    &.active {
      color: red;
    }
  }
  .avatar {
    width: 72px;
    height: 72px;
    margin-right: 25px;
  }
  .title-wrap {
    display: flex;
    justify-content: space-between;
    align-items: center;
    .user-name {
      color: #406599;
      font-size: 26px;
    }
  }
  .comment-content {
    font-size: 32px;
    color: #222222;
    word-break: break-all;
    text-align: justify;
  }
  .comment-pubdate {
    font-size: 19px;
    color: #222;
    margin-right: 25px;
  }
  .bottom-info {
    display: flex;
    align-items: center;
  }
  .reply-btn {
    width: 140px;
    height: 48px;
    line-height: 48px;
    font-size: 21px;
    color: #222;
  }
  .like-btn {
    height: 30px;
    padding: 0;
    border: none;
    font-size: 19px;
    line-height: 30px;
    margin-right: 7px;
    .van-icon {
      font-size: 30px;
    }
  }
}
</style>
