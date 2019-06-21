# Picture hot_zone.

---
根据项目需求而来，给图片创建不同热区进行跳转

![image](http://wx2.sinaimg.cn/large/005SNrnIgy1g2ensfkq03g30os0iethj.gif)

---
## Use
将此`vuehot`文件夹放入你的`vue`项目的src目录中,然后在你需要用到的组件中引入即可使用，也可以通过全局引入，使每个组件都能使用.
``` breach
//Use in Component
import Hot from "../../vuehot";  //此处根据自己的目录来进行引入.
export default {
  components:{
    Hot
  }
}
//Use in global
import Hot from "./vuehot";
Vue.use(Hot);

<Hot [options]></Hot>
```
## Options
#### Attribute
可以将他们设置在vue data()数据中

| 属性 | 类型 | 描述 | 值 |
| :-:  | :-:  | :-:  | :-: |
| image| String | 图片热区的图片(required) | |
| max | Number | 可允许最大热区数量 |  |
| types | String | 配合max使用（验证通过拖动or按钮达到max）| 'move' or 'btn' |
| zonesInit  | Array | 初始化zones的热区块儿 | item(widthPer heightPer topPer leftPer) |

## Events

| 事件名称 | 描述 | 参数 |
| :-:      | :-:  | :-:  |
| change   | 热区改变时触发 | 所有热区组成的数组 |
| add     | 可拖动添加热区(也可用按钮添加热区) 必填项 | 拖动所形成的热区位置大小信息（按钮点击无） |
| remove  | 删除热区同时会执行change事件 | 删除热区所对应的索引 |
| overRange | 热区数量到最大值(拖动)同时会触发change事件 | 热区的索引 |

## Code
``` breach
<template>
  <div class="hotapp">
    <Hot
     :image="image"
     :zonesInit="zones"
     :max="max"
     :types="types"
     @add="handleAdd" // 必填项(控制创建热区)
     @remove="handleRemove"
     @change="handleChange"
     @overRange="handleoverChange"
    >
    </Hot>
    <i-button type="primary" @click="handleAdd">添加热区</i-button>
    <input
      type="text"
      v-for="(zone, index) in zones"
      :key="index"
      v-model="zone.url"
      :placeholder="`Area ${index + 1} url`"
    >
  </div>
</template>
<script>
import Hot from '../../vuehot'
export default {
  components: {
    Hot
  },
  data () {
    return {
      image: 'https://pic1.zhimg.com/80/v2-62516c6cad7a4d192e68530f7bfc7797_hd.jpg',
      zones: [],
      max: 3,
      types: 'move'
    }
  },
  methods: {
    handleAdd (zon) {
      let zone
      if (zon.topPer) { // 存在此参数,则通过拖动创建热区
        zone = zon
        zone.url = 'https://github.com/ymtonny'
        this.types = 'move' // 存在max热区时，验证通过拖动或点击达到max
      } else { //通过按钮创建热区
        zone = {
          heightPer: 0.2027,//20.27%
          leftPer: 0.2027,
          topPer: 0.2027,
          widthPer: 0.1027,
          url: 'https://github.com/ymtonny'
        }
        this.types = 'btn' // 存在max热区时，验证通过拖动或点击达到max
      }
      this.zones.push(zone) // 热区数据
    },
    handleoverChange (e) {
      console.log(e)
    },
    handleRemove (index) {
    
    },
    handleChange (e) {
      console.log(e)
    }
  }
}
</script>
<style scoped>
.hotapp{
  width:800px;
  height:800px;
  margin:auto;
  margin-top:100px;
}

input {
  width: 100%;
  box-sizing: border-box;
  margin-top: 20px;
  padding: 10px;
  background: #fff;
  border: 1px solid #ccc;
  outline: none;
  color: #555;
  transition: all 0.30s ease-in-out;
}
input:focus {
  box-shadow: 0 0 5px #43D1AF;
  border: 1px solid #43D1AF;
}
</style>
```
