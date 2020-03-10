<template>
  <div class="tinymce-editor">
    <editor v-model="myValue" :init="init"></editor>
  </div>
</template>
<script>
import tinymce from "tinymce/tinymce";
import Editor from "@tinymce/tinymce-vue";
import "tinymce/themes/silver";
// 编辑器插件plugins
// 更多插件参考：https://www.tiny.cloud/docs/plugins/
import "tinymce/plugins/image"; // 插入上传图片插件
import "tinymce/plugins/media"; // 插入视频插件
import "tinymce/plugins/table"; // 插入表格插件
import "tinymce/plugins/lists"; // 列表插件
import "tinymce/plugins/wordcount"; // 字数统计插件
import "../assets/tinymce/plugins/lineheight/plugin"; // 设置行高插件

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
      default: "lists image media table wordcount lineheight"
    },
    toolbar: {
      type: [String, Array],
      default:
        "undo redo |  lineheight | formatselect | bold italic forecolor backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat"
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