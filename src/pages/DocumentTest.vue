<template>
  <q-page class="flex flex-center">
    {{resultat}}
    <button @click="prova">prova document</button>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { Document, Packer, Paragraph, TextRun } from "docx";
import { saveAs } from 'file-saver';


export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      resultat: "hiiiii"
    };
  },
  methods:{
    prova: function(){
      console.log("holaaaa");
      const doc = new Document({
        sections: [{
          properties: {},
          children: [
            new Paragraph({
              children: [
                new TextRun("Hello World"),
                new TextRun({
                  text: "Foo Bar",
                  bold: true,
                }),
                new TextRun({
                  text: "\tGithub is the best",
                  bold: true,
                }),
              ],
            }),
          ],
        }]
      });

      Packer.toBlob(doc).then((blob) => {
        // saveAs from FileSaver will download the file
        saveAs(blob, "example.docx");
      });

    }
  }
})
</script>
