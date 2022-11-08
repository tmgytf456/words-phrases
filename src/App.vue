<template>
    <div class="input-wrap">
      <label >标题：</label>
      <input v-model="title" type="text">
      <br />
      <label >内容：</label>
      <textarea v-model="content" name="" id="" cols="30" rows="10"></textarea>
      <button @click="print">打印</button>
      <input id="show" type="radio" v-model="hideWord" :value="false" />
      <label for="show"></label>
      <span>显示</span>
      <input id="hide" type="radio" v-model="hideWord" :value="true" />
      <label for="hide"></label>
      <span>隐藏</span>
    </div>
    <div class="content">
      <div class="title-wrap">{{title}}</div>
      <div class="content-wrap" :class="{'hiden-word':hideWord}">
        <div class="word-wrap" v-for="(item, index) in contentList" :key="index">
            <div class="pinyin-line">
              <i v-for="(singlepy, pyIndex) in singlePinyinList[index].split(' ')" :key="pyIndex">
                <span v-show="singlepy">{{singlepy}}</span>
              </i>
            </div>
            <div class="word-line" >
              <i v-for="(singleWd, wdIndex) in item" :key="wdIndex">
                <img :src="gezImg" alt="背景田字格"/>
                <span v-show="singleWd">{{singleWd}}</span>
              </i>
            </div>
        </div>
     </div>
    </div>
</template>

<script lang="ts">
import {pinyin} from 'pinyin-pro'
import gezi from './assets/gezi.png'

  export default {
    data() {
      return {
        title: '',
        content:'',
        hideWord:false,
        gezImg:gezi
      }
    },
    computed:{
      contentList(){
        const {content} = this
        const newContent = content.trim().replace(/\n/g,' ')
        const list = newContent.split(' ')
        let wordList:any=[]
        list.forEach(item=>{
          if(item!==''){
            wordList.push(item)
          }
        })
        return wordList
      },
      singlePinyinList(){
        const {contentList} = this
        let pinyinList:any =[]
        contentList.forEach((item: string)=>{
          const wordPinyin = pinyin(item)
          pinyinList.push(wordPinyin)
        })
        console.log(pinyinList)
        return pinyinList
      }
    },
    methods:{
      print(){
        if(this.contentList){
          this.hideWord = false
          window.print()
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
@media print{
  .input-wrap{
    display: none;
  }
}
.content{
  width: 1068px;
  margin: 0 auto;
  .title-wrap{
    font-size: 30px;
    line-height: 100px;
    text-align: center;
    font-family: 'KaiTi', 'STKaiti';
  }
}
.hiden-word{
  .word-line span{
    display: none;
  }
}
.content-wrap{
  display: flex;
  flex-wrap: wrap;
  .word-wrap{
    padding:0 10px;
    i{
      display: inline-block;
      width: 80px;
      text-align: center;
      font-style: normal;
    }
  }
  
  .pinyin-line{
    i{
      font-size: 30px;
      line-height: 50px;
      margin-right: -1px;
    }
  }
  .word-line{
    i{
      position: relative;
      overflow: hidden;
      font-family: 'KaiTi', 'STKaiti';
      height: 80px;
      line-height: 80px;
      font-size: 60px;
      margin-right: -1px;
      img{
        height: 80px;
        width: 80px;
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
      }
    }
    
  }
}
</style>