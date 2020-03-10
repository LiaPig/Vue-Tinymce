# tinymce-vue-demo

# 一、相关资料
[tinymce编辑器官网](https://www.tiny.cloud/docs/quick-start/)，有点卡，大家要耐心等待。
再推荐一个，翻译的很好的：[tinymce中文文档](http://tinymce.ax-z.cn/configure/integration-and-setup.php)

我使用的是vue-cli 4.2
![](https://upload-images.jianshu.io/upload_images/7016617-514b6e6658e17920.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 二、正式开始
### 1.进入到项目目录，安装依赖
```
// 目录名请匹配
cd tinymce-vue-demo

npm install --save tinymce
npm install --save @tinymce/tinymce-vue
```

### 2. 搬运资源
1. 为了方便打包后静态资源都在`static`文件夹里，先在`/public`目录下新建`static`文件夹:
![新建static文件夹](https://upload-images.jianshu.io/upload_images/7016617-6818923e1feba86b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在`node_modules`里找到`tinymce`文件夹，将其中的`/skins` 文件夹以及里面的所有内容，复制到第一步新建的`/public/static`文件夹内:
![node_modules里](https://upload-images.jianshu.io/upload_images/7016617-79b85600e38de695.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![成功拷贝](https://upload-images.jianshu.io/upload_images/7016617-dd518366325dc1f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 后续还会用到的中文汉化包，在[http://tinymce.ax-z.cn/](http://tinymce.ax-z.cn/)页面中搜`zh_CN.js`，按提示操作即可。最后放到`/public/static/tinymce`目录下：
![/public/static/tinymce/zh-CN.js](https://upload-images.jianshu.io/upload_images/7016617-5eca4e420b4f2b31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 3. 新增一个组件，名为`tinymce-editor.vue`，开始引入tinymce
```
import tinymce from "tinymce/tinymce";
import Editor from "@tinymce/tinymce-vue";
import "tinymce/themes/silver";  // 主题资源，用的/node_modules里的
// 编辑器插件plugins
// 更多插件参考：https://www.tiny.cloud/docs/plugins/
import "tinymce/plugins/image"; // 插入上传图片插件
import "tinymce/plugins/media"; // 插入视频插件
import "tinymce/plugins/table"; // 插入表格插件
import "tinymce/plugins/lists"; // 列表插件
import "tinymce/plugins/wordcount"; // 字数统计插件

export default {
  components: {
    Editor
  },
  props: {
    value: {
      type: String,
      default: ""
    },
    plugins: {
      type: [String, Array],
      default: "lists image media table wordcount"
    },
    toolbar: {
      type: [String, Array],
      default:
        "undo redo |  formatselect | bold italic forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat"
    }
  },
  data() {
    return {
      init: {
        language_url: "/static/tinymce/zh_CN.js", //public目录下
        language: "zh_CN",
        skin_url: "/static/tinymce/skins/ui/oxide", //public目录下
        height: 300,  // 高度
        plugins: this.plugins, // 引入的插件
        toolbar: this.toolbar,  // 工具栏
      },
      myValue: this.value
    };
  },
  mounted() {
    tinymce.init({});
  },
  methods: {
  },
  watch: {
    value(newValue) {
      this.myValue = newValue;
    },
    myValue(newValue) {
      this.$emit("input", newValue);
    }
  }
};
</script>
```

### 4. 在`App.vue`中引入`TinymceEditor.vue`子组件
```
 <template>
  <div id="app">
    <tinymce-editor ref="editor" v-model="contents"></tinymce-editor>
  </div>
</template>

 <script>
import TinymceEditor from "@/components/TinyEditor";

export default {
  name: "app",
  components: {
    TinymceEditor
  },
  data() {
    return {
      contents: ""
    };
  }
};
</script>
```

这时候已经能显示出来啦：
![PC端](https://upload-images.jianshu.io/upload_images/7016617-9d4c13a20f0bf89a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![移动端，单行模式的工具栏可以横向滚动，也可以换作多行模式，请参照文档](https://upload-images.jianshu.io/upload_images/7016617-b74557eda2ec34d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 三、引入设置行高插件
参考文档：
[http://tinymce.ax-z.cn/more-plugins/lineheight.php](http://tinymce.ax-z.cn/more-plugins/lineheight.php)

1. 按照参考文档那样下载后解压得到`lineheight`文件夹

![](https://upload-images.jianshu.io/upload_images/7016617-9487d316f87d74ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在`/src/asstes`目录下，新建一个`tinymce`文件夹，再在里面新建一个`plugins`文件夹，将刚解压得到的`lineheight`文件夹以及里面的内容拷贝过去：

![](https://upload-images.jianshu.io/upload_images/7016617-1ee62ef209c720de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.在`TinymceEditor.vue`页面中引入
```
import "../assets/tinymce/plugins/lineheight/plugin"; // 设置行高插件
```

4.在`plugins`和`toolbar`引入，引入后最好是重新运行项目哦
```
 plugins: {
      type: [String, Array],
      default: "lists image media table wordcount lineheight"
},
toolbar: {
     type: [String, Array],
     default:
        "undo redo |  lineheight | formatselect | bold italic forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat"
}
```

![](https://upload-images.jianshu.io/upload_images/7016617-6945fc377824909d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
