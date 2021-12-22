# VUE



### Installation

```
vue create first-app (manually select)
cd first-app
npm run serve
```

```shell
npm install vue-router@next --save
npm install --save vuex@next
npm install --save axios
npm install element-plus --save
npm install unplugin-vue-components // 按需自动导入
```

```js
// vue.config.js  重新启动
const Components = require('unplugin-vue-components/webpack');
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers');

module.exports = {
  configureWebpack: {
    plugins: [
      Components({
        resolvers: [ElementPlusResolver()],
      }),
    ],
  },
};

```

### Reload

​	App.vue

```vue
<router-view v-if="isRouterAlive"></router-view>

export default {
	provide() {
    return {
      reload: this.reload,
    };
  },
  data() {
    return {
      //控制视图是否显示的变量
      isRouterAlive: true,
    };
  },
  methods: {
    reload() {
      this.isRouterAlive = false;
      this.$nextTick(function () {
        this.isRouterAlive = true;
      });
    },
  },
}
```

component.vue

```vue
export default {
	inject: ['reload'],
}
```

