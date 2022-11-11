---
title: Vue2组件通信
date: 2022-11-10
categories: 
- 面试准备
- Vue2
tag: Vue2
---

## Vue2组件通信

<!-- 说到Vue2的组件通信我们肯定能想到，`父子组件通信`、`兄弟组件通信`、`跨级组件通信`。 -->

#### props
说到父子组件通信我们最先想到的应该就是props，父组件通过给子组件传递props来进行父子组件通信。
父组件通过下面这种方式向子组件传递值，下面是传递了一个动态的值，当然你也可以传递静态的值。
```vue
<Children :msg="msg"/>
```
子组件通过props接收传递的值。
```vue
props: ['msg']
```
子组件接收到值后就可以直接使用`this.msg`的方式获取到。
除了上面这种props的写法，你还可以给props的值设置不同的类型。
```vue
props: {
  msg: String
},
```
那怎么子组件怎么给父组件传递值呢？当然，你仍然可以使用props，父组件给子组件传递一个函数，子组件通过函数改变父组件的值。

#### .sync
帮助父子组件双向绑定，当修改了一方的值，另一方的值也会跟着改变。
```vue
//parent.vue
<template>
  <div>
    <Children :msg.sync="msg"/>
    {{msg}}
  </div>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: 'App',
  components: {
    Children
  },
  data(){
    return {
      msg: 'parent component'
    }
  }
}
</script>
```
```vue
//children.vue
<template>
  <button @click="modifyPraent">修改父组件的msg</button>
</template>
<script>
export default {
  name: 'Children',
  props: ['msg'],
  computed: {
    childrenMsg: {
      get(){
        return this.msg;
      },
      set(value){
        this.$emit('update:msg', value);
      }
    }
  },
  methods:{
    modifyPraent(){
      this.childrenMsg = 'children modify parent component'
    }
  }
}
</script>
```

#### v-module
```vue
//parent.vue
<template>
  <div>
    <Children v-model="msg"/>
    <button @click="modify">click modify parent msg property with data</button>
    parent msg:{{msg}}
  </div>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  data(){
    return {
      msg: 'parent component data',
    }
  },
  methods:{
    modify(){
      this.msg = 'click parent button modify msg'
    },
    getMsg(){
      return this.msg
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <div>
    <div>children msg:{{value}}</div>
    <button @click="getParent">点击改变父组件msg</button>
  </div>
</template>
<script>
export default {
  name: 'Children',
  props: ['value'],
  methods:{
    getParent(){
      this.$emit('input', 'children component modify data');
    }
  }
}
</script>
```


#### ref
ref如果在普通的DOM元素上就是DOM，但是如果ref在组件上是获取的这个组件的实例，所以如果父组件可以通过ref获取子组件的实例来控制子组件。

可以在父组件控制子组件的值修改。

```vue
//parent.vue
<template>
  <Children ref="child"/>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  mounted(){
    this.$refs.child.message();
  }
};
</script>
```
```vue
//children.vue
<template>
  <div></div>
</template>
<script>
export default {
  name: 'Children',
  methods:{
    message(){
      console.log('子组件');
    }
  }
}
</script>
```

#### $emit / v-on
子组件通过$emit向父组件传递信息
```vue
//parent.vue
<template>
  <div>
    <Children v-on:sendChild="getChild"/>
    <!-- 简写 -->
    <Children @sendChild="getChild"/>
  </div>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  methods:{
    getChild(msg){
      console.log(msg);
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <div></div>
</template>
<script>
export default {
  name: 'Children',
  mounted(){
    this.$emit("sendChild",'子组件向父组件发送信息')
  }
}
</script>
```

#### $attrs / $listeners
父组件给子孙组件传递数据该怎么办呢？如果我们使用props一层一层传的话，会显得十分麻烦，这时候我们会想到使用Vuex，但是使用Vuex的话又有种大材小用的感觉。那么这时候我们就可以使用$attrs和$listeners。
$attrs的定义是这样的，包含父作用域上除`class选择器`和`style样式`之外的非`props`合集。
$listeners的定义是这样的，父作用域里的`.native`除外的`监听事件`集合。
```vue
//parent.vue
<template>
  <Children
    :name = "name"
    :age = "age"
    :sex = "sex"
  />
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  data(){
    return {
      name: 'name',
      age: 'age',
      sex: 'sex'
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <Grandson v-bind="$attrs"/>
</template>
<script>
import Grandson from './Grandson.vue';
export default {
  name: 'Children',
  props:['name'],
  components:{
    Grandson
  },
  mounted(){
    //这里输出{age: 'age', sex: 'sex'}，因为我们props里面写了name，如果不写name的话也会输出name:'name'
    console.log('children',this.$attrs);
  }
}
</script>
```
```vue
<template>
  <div></div>
</template>
<script>
export default {
  name: 'Grandson',
  props: ['age'],
  mounted(){
    //这里输出{sex: 'sex'}，如果我们没在props里面写age，会输出{sex: 'sex', age: 'age'}
    console.log('grandson',this.$attrs);
  }
}
</script>
```

#### $children / $parent
$children: 当前实例的直接子组件，不包括孙组件，不能保证顺序，并且也不是响应式的。
$parent: 获取他的第一个父实例，如果有父实例的话。
```vue
//parents.vue
<template>
  <Children/>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: 'App',
  components: {
    Children
  },
  data(){
    return {
      msg: 'parent component'
    }
  },
  mounted(){
    //输出children component
    console.log(this.$children[0].msg);
  }
}
</script>
```
```vue
//children.vue
<template>
  <div></div>
</template>
<script>
export default {
  name: 'Children',
  data(){
    return {
      msg: 'children component'
    }
  },
  mounted(){
    //输出parent component
    console.log(this.$parent.msg);
  }
}
</script>
```

#### provide / inject
```yarm
//我们可以使用这种方法来传递数据，但是这样的数据只能写死，不能从this中获取
provide:{
  msg: 'this is freezing data'
},
```
```yarm
//我们通过这种方法可以传递this中的数据
provide(){
  return {
    name:"name",
    msg: this.msg //data`s property
  }
},
```
```vue
//parent.vue
<template>
  <div>
    <Children/>
    <button @click="modify">click modify parent msg property with data</button>
    parent msg:{{msg}}
  </div>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  data(){
    return {
      msg: 'parent component data',
      change: this.msg
    }
  },
  provide(){
    return {
      name:"name",
      msg: this.msg,
      getMsg: this.getMsg
    }
  },
  methods:{
    modify(){
      this.msg = 'click parent button modify msg'
    },
    getMsg(){
      return this.msg
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <div>
    <div>children msg:{{msg}}</div>
    <button @click="getParent">点击获取父组件msg</button>
  </div>
</template>
<script>
export default {
  name: 'Children',
  inject: ['msg', 'getMsg'],
  mounted(){
    console.log(this.msg); 
  },
  methods:{
    getParent(){
      console.log(this.getMsg());
    }
  }
}
</script>
```

#### EventBus
EventBus是中央事件总线。
首先我们先定义事件总线。
```yarm
//方法一
//我们可以创建一个js文件
import Vue from "vue"
export default new Vue()
//然后在要用到的地方，直接import引入使用

//方法二
//直接挂载到Vue的prototype上
//main.js
Vue.prototype.$bus = new Vue()

//方法三
//直接注入到Vue根对象上
//main.js
new Vue({
    el:"#app",
    data:{
        Bus: new Vue()
    }
})
```
```vue
//parent.vue
<template>
  <div>
    <Children/>
    <button @click="send">click send msg</button>
  </div>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  methods:{
    send(){
      this.$bus.$emit('sendMsg','parent send msg');
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <div>{{msg}}</div>
</template>
<script>
export default {
  name: 'Children',
  data(){
    return {
      msg: ''
    }
  },
  mounted(){
    //监听sendMsg
    this.$bus.$on('sendMsg', data => {
      this.msg = data;
    })
  },
  beforeDestroy(){
    //卸载监听
    this.$bus.$off('sendMsg');
  }
}
</script>
```

#### Vuex

#### $root
$root可以拿到App.vue里面的数据和方法。
```vue
//children.vue
<template>
  <div></div>
</template>
<script>
export default {
  name: 'Children',
  mounted(){
    console.log(this.$root);
  }
}
</script>
```

#### slot
子组件的数据通过插槽的方式传递给父组件，然后父组件再插回去，父组件在插回去的时候可以对数据做一下处理。
```vue
//parent.vue
<template>
  <Children v-slot="slotProps">
    {{msg + ":" + slotProps.msg}}
  </Children>
</template>
<script>
import Children from './components/Children.vue';
export default {
  name: "App",
  components: {
    Children
  },
  data(){
    return {
      msg: 'parent'
    }
  }
};
</script>
```
```vue
//children.vue
<template>
  <div>
    <slot :msg="msg"></slot>
  </div>
</template>
<script>
export default {
  name: 'Children',
  data(){
    return {
      msg: 'children component data'
    }
  }
}
</script>
```