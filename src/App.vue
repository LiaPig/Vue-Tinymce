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