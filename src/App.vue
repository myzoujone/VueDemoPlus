<template>
  <div id="app">

    <!--使用Header组件。-->
    <DemoHeader  :recordN="ItemLists.length"></DemoHeader>
    <!--中间这里放了一个UL的列表，将数组ppp送出去了，-->
    <ItemList :ppp="ItemLists"></ItemList>
    <!--再声明了一下底部，-->
    <demoFooter  :allredords="ItemLists"></demoFooter>


  </div>
</template>

<script>


import demoFooter from "@/components/demoFooter";
import DemoHeader from "@/components/DemoHeader";
import ItemList from "@/components/ItemList";
export default {
  name: 'App',

  components:{demoFooter,DemoHeader,ItemList},          //这里是声明了三个组件
  methods:{                                             //这里是声明了三个组件
    recieve(x){                                         //接收函数
      this.ItemLists.unshift(x)                         //如果接收到了OBJ的对象，加入数组
    },
    change1(id){                                         //当某一个前面的checkbox被选择了
      this.ItemLists.forEach((obj)=>{                   //循环全部数组
        if (obj.id==id){                                //如果某一个的ID等于传值的ID
          obj.done=!obj.done                            //勾选的标识取反
        }
      })
    },
    deleteOneRecord(id){                                  //删除一个记录
      //数组删除，删除的数据开始位是ID出现的那个的索引的位置，取一位删除。
      this.ItemLists.splice(this.ItemLists.findIndex(item => item.id === id), 1)
    },
    selectappAll(flag){                                 //接收全选的标识
      this.ItemLists.forEach((obj)=>{obj.done=flag})    //循环将全部标识更换
    },
    deleteAppAll(){                                     //删除全部的
      //将数组等于数组过滤之后的，过滤内容是done等于假
      this.ItemLists=this.ItemLists.filter(item => item.done === false)
    }
  },
  mounted() {                                                   //当加载完成之后
    this.$bus.$on("recieve",this.recieve)                 //给总线加上接收
    this.$bus.$on("change1",this.change1)                 //给总线加上变化
    this.$bus.$on("deleteOneRecord",this.deleteOneRecord) //给总给删除一条记录
    this.$bus.$on("selectappAll",this.selectappAll)       //给总线加上选择全部
    this.$bus.$on("deleteAppAll",this.deleteAppAll)       //给总线加上删除全部
  },
  beforeDestroy() {                                             //在消毁之前
    this.$bus.$off("recieve")                             //消毁接收总线
    this.$bus.$off("change1")                             //消毁变化总线
    this.$bus.$off("deleteOneRecord")                     //消毁删除一条总线
    this.$bus.$off("selectappAll")                        //消毁选择全部
    this.$bus.$off("deleteAppAll")                        //消毁删除全部的数据线
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
