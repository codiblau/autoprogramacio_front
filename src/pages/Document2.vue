<template>
  <q-page class="flex flex-center">
    <button @click="generate">prova document</button>
  </q-page>
</template>

<script>
import {defineComponent} from 'vue';
import {
  Document,
  Packer,
  Paragraph,
  TextRun,
  HeadingLevel,
  TableOfContents,
  Header,
  Footer,
  AlignmentType,
  NumberFormat,
  PageNumber
} from "docx";
import {saveAs} from 'file-saver';


export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      resultat: "hiiiii",
      programacio: {}
    };
  },
  async created() {
    const programacio = await this.$axios.get("http://localhost:8080/programacio/15")
    this.programacio = programacio.data;
  },
  methods: {
    generate: function () {
      const seccions = this.programacio.seccions;
      seccions.sort((a,b)=>a.ordre-b.ordre);
      seccions.forEach(s=>{
        s.paragrafs.sort((a,b)=>a.ordre-b.ordre);
      })
      /*
        new Paragraph({
              heading: HeadingLevel.HEADING_1,
              text: "Introducció",
            }),
            new Paragraph({
              style: "paragraf",
              text: "El present mòdul s'impartirà en el col·legi concertat \"Es Liceu\" situat a es Pont d'Inca en el municipi de Marratxí amb una població aproximada de 15.500 habitants. L’entorn del centre és un barri d'habitatges plurifamiliars de blocs de pisos i cases unifamiliars. Els carrers del barri són amples, cosa que afavoreix la circulació i l'accés."
            }),
            new Paragraph({
              style: "paragraf",
              children: [
                new TextRun("La programació desenvolupada en aquest document és la del mòdul Desenvolupament Web en Entorn Client (codi 0612), que s’imparteix en el segon curs del títol “Tècnic en desenvolupament d’aplicacions Web” durant el curs acadèmic "),
                new TextRun({
                  text: "2019/20",
                  bold: true,
                }),
                new TextRun(". El títol capacita a l’alumnat a desenvolupar, desplegar i mantenir aplicacions web amb la capacitat de gestionar l’accés a dades de forma òptima i segura. En concret, el mòdul de Desenvolupament Web en Entorn Client és molt important per assolir aquestes capacitats, ja que és en la part del client on es presenta la informació cap a l'usuari i també és el camp on més s'ha innovat en els darrers anys."),
              ],
            }),
        */
      const seccionsMap = [];
      seccions.forEach(s=>{
        let heading = 1;
        switch (s.heading){
          case 1: heading = HeadingLevel.HEADING_1; break;
          case 2: heading = HeadingLevel.HEADING_2; break;
          case 3: heading = HeadingLevel.HEADING_3; break;
          case 4: heading = HeadingLevel.HEADING_4; break;
          case 5: heading = HeadingLevel.HEADING_5; break;
          case 6: heading = HeadingLevel.HEADING_6; break;
          default:
        }
        const pHeading = new Paragraph({
          heading: heading,
          text: s.titol,
        })
        seccionsMap.push(pHeading)
        s.paragrafs.forEach(p=>{
          const pParagraf = new Paragraph({
            style: "paragraf",
            text: p.descripcio
          })
          seccionsMap.push(pParagraf);
        })
     })

      const doc = new Document({
        styles: {
          default: {
            heading1: {
              paragraph: {
                spacing: {
                  before: 240,
                  after: 120,
                }
              },
            },
            heading2: {
              spacing: {
                before: 240,
                after: 120,
              }
            },
            paragraph: {
              spacing: {
                before: 240,
                after: 120,
              },
            },
          },
          /*listParagraph: {
            run: {
              color: "#FF0000",
            },
          },
        },*/
          paragraphStyles: [
            {
              id: "paragraf",
              name: "Paràgraf",
              basedOn: "Normal",
              quickFormat: true,
              paragraph: {
                indent: {
                  firstLine: 300,
                },
                spacing: {
                  before: 240,
                  after: 120,
                }
              },
            },
          ],
        },
        sections: [{
          properties: {},
          children: [
            new Paragraph({
              children: [
                new TextRun("Hello World11"),
                new TextRun({
                  text: "Foo Bar11",
                  bold: true,
                }),
                new TextRun({
                  text: "\tGithub is the best11",
                  bold: true,
                }),
              ],
            }),
          ],
        }, {
          properties: {},
          headers: {
            default: new Header({
              children: [
                new Paragraph({
                  style: "paragraf",
                  text: "Joan Galmés Riera"
                }),
              ],
            }),
          },
          footers: {
            default: new Footer({
              children: [
                new Paragraph({
                  alignment: AlignmentType.CENTER,
                  children: [
                    new TextRun({
                      children: ["Pàgina ", PageNumber.CURRENT, " de ", PageNumber.TOTAL_PAGES],
                    })
                  ],
                }),
              ],
            }),
          },
          children: seccionsMap,
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
