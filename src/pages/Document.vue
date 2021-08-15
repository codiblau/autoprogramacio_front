<template>
  <q-page class="flex flex-center">
    {{ resultat }}
    <button @click="prova">prova document</button>
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
      resultat: "hiiiii"
    };
  },
  methods: {
    prova: function () {
      console.log("holaaaa");
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
          children: [
            new TableOfContents("Índex", {
              hyperlink: false,
              headingStyleRange: "1-5",
              style: "paragraf",
            }),
            /* 1. Introducció */
            new Paragraph({
              heading: HeadingLevel.HEADING_1,
              text: "Introducció",
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
            new Paragraph({
              heading: HeadingLevel.HEADING_2,
              text: "Normativa"
            }),
            new Paragraph({
                style: "paragraf",
                text: "La legislació vigent que condiciona la programació ve establerta per la següent normativa:"
              }
            ),
            new Paragraph({
                style: "paragraf",
                text: "- Llei Orgànica 2/2006, de 3 de maig, d'Educació modificada per la Llei Orgànica 8/2013, de 9 de desembre, per a la millora de la qualitat educativa."
              }
            ),
            new Paragraph({
                style: "paragraf",
                text: "- Decret 91/2012, de 23 de novembre, que estableix l'ordenació general de la formació professional del sistema educatiu en el sistema integrat de formació professional a les Illes Balears."
              }
            ),
            new Paragraph({
                style: "paragraf",
                text: "- Ordre EDU / 2887/2010, de 2 de novembre de 2010, pel qual s’estableix el currículum del cicle formatiu de Grau Superior corresponent al títol de Desenvolupament d'Aplicacions Web."
              }
            ),
            new Paragraph({
              heading: HeadingLevel.HEADING_2,
              text: "Unitats de Competència i Qualificacions Professionals"
            }),
            new Paragraph({
              style: "paragraf",
              text: "El mòdul d'aquesta programació, Desenvolupament Web en Entorn Client (0612) juntament amb el mòdul de Disseny d'Interfícies Web (0615) porten a l'acreditació de la unitat de competència UC0491_3 Desenvolupar elements de programari en l'entorn client."
            }),
            new Paragraph({
              style: "paragraf",
              text: "La unitat de competència anterior (UC0491_3) juntament amb les unitats de competència UC0492_3 Desenvolupar elements de programari en l'entorn servidor i UC0493_3 Implementar, verificar i documentar aplicacions web en entorns internet, intranet i extranet condueixen a l'obtenció de la qualificació professional de \"Desenvolupament d'aplicacions amb tecnologies Web\" (IFC154_3) (Reial Decret 1087/2005 de 16 de setembre) del Catàleg Nacional de Qualificacions Professionals."
            }),
            new Paragraph({
              heading: HeadingLevel.HEADING_1,
              text: "Contextualització del mòdul en el centre"
            }),,
            new Paragraph({
              heading: HeadingLevel.HEADING_2,
              text: "Característiques del centre educatiu"
            }),
            new Paragraph({
              style: "paragraf",
              text: "El present mòdul s'impartirà en el col·legi concertat \"Es Liceu\" situat a es Pont d'Inca en el municipi de Marratxí amb una població aproximada de 15.500 habitants. L’entorn del centre és un barri d'habitatges plurifamiliars de blocs de pisos i cases unifamiliars. Els carrers del barri són amples, cosa que afavoreix la circulació i l'accés."
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
