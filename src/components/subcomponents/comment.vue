<template>
  <div class="cmt-container">
    <h3>发表评论</h3>

    <hr />

    <textarea placeholder="请输入要评论的内容" v-model="msg" maxlength="120"></textarea>

    <mt-button type="primary" size="large" @click="postComment">发表评论</mt-button>

    <div class="cmt-list">
      <div class="cmt-item" v-for="(item,i) in comments" :key="item.add_time">
        <div
          class="cmt-title"
        >第{{ i }}楼&nbsp;&nbsp;用户:{{ item.user_name }}&nbsp;&nbsp;发表时间:{{ item.add_time | dateFormat }}</div>
        <div class="cmt-body">{{ item.content === 'undefined' ? '此用户很懒,啥都没说' : item.content }}</div>
      </div>
    </div>

    <mt-button type="danger" size="large" plain @click="getMore">加载更多</mt-button>
  </div>
</template>

<script>
import {Toast} from 'mint-ui'

export default {
  data(){
    return {
      pageIndex: 1, //默认展示第一页数据
      comments: [], //所有的评论数据
      msg:''
    }
  },
  props: 
  [
    'id'
  ],
  created(){
    this.getComments()
  },
  methods: {
    getComments(){
      this.$http.get('api/getcomments/'+this.id+'?pageindex='+this.pageIndex)
      .then(result => {
        if(result.body.status === 0){
          //每当获取新评论数据的时候,不要把老数据清空,而是应该以老数据拼接上新数据
          this.comments = this.comments.concat(result.body.message)
          console.log('!get!')
        }else{
          Toast('获取评论失败!')
        }
      })
    },
    getMore(){
      this.pageIndex++;
      this.getComments()
    },
    postComment(){
      if(this.msg.trim().length === 0){
        Toast('评论内容不能为空!')
      }

      this.$http.post('api/postcomment/'+ this.id,{
        content:this.msg.trim()
      })
      .then(function(result){
        if(result.body.status === 0){
          let cmt = { user_name: '匿名用户',add_time:Date.now(),content: this.msg.trim()}
          this.comments.unshift(cmt)
          this.msg = ''
        }else{
          Toast('发布评论失败')
        }
      })
    }
  }
}
</script>

<style scoped lang="scss">
.cmt-container {
  h3 {
    font-size: 16px;
  }

  textarea {
    font-size: 14px;
    height: 85px;
    margin: 0;
  }

  .cmt-list {
    margin-top: 5px;
    .cmt-item {
      font-size: 13px;
      .cmt-title {
        background-color: #ccc;
        line-height: 30px;
      }
      .cmt-body {
        line-height: 35px;
        text-indent: 2em;
      }
    }
  }
}
</style>