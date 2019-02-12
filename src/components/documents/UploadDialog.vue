<template>
    <div>
         <v-btn
      fab
      bottom
      right
      color="pink"
      dark
      fixed
      @click.stop="dialog = !dialog"
    >
      <v-icon>add</v-icon>
    </v-btn>

     <v-dialog v-model="dialog" width="800px">
      <v-card>
        <v-card-title
          class="grey lighten-4 py-4 title"
        >
          Notarizar documentos
        </v-card-title>
        <v-container grid-list-sm class="pa-4">
          <v-layout row wrap>
            <v-flex xs12 align-center justify-space-between>
              <v-layout align-center>
                <v-text-field
                  placeholder="Asunto"
                  v-model="subject"
                ></v-text-field>
              </v-layout>
            </v-flex>
            <v-flex xs12>
              <v-text-field
                placeholder="Texto"
                v-model="text"
              ></v-text-field>
            </v-flex>
            <v-flex xs6>
            <input type="file" @change="loadFileContent">
            <!--<upload-btn
              title="Seleccione el documento"
              :fileChangedCallback="fileChanged"
              :multiple="true"
              :ripple="false"
              >
               <template slot="icon-left">
                <v-icon left>add</v-icon>
                </template>
              </upload-btn>-->
            </v-flex>
           <!--< <v-flex xs6>
              <div class="text-xs-center" v-for="file in files" :key="file.name">
                 <v-chip close>{{ file.name }}</v-chip>
              </div>
            </v-flex>-->
          </v-layout>
          <v-layout>
            <v-flex xs12>
              File hash is: {{ hash }}
            </v-flex>
          </v-layout>

        </v-container>
        <v-card-actions>

          <v-spacer></v-spacer>
          <v-btn flat color="primary" @click="dialog = false">Cancelar</v-btn>
          <v-btn flat @click="notarizeDocs" :disabled="!formIsValid()">Procesar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    </div>
</template>

<script>
// import UploadButton from 'vuetify-upload-button'
import sha256 from 'simple-sha256'

export default {
  data () {
    return {
      dialog: false,
      subject: '',
      text: '',
      file: '',
      currentFile: '',
      hash: ''
    }
  },
  methods: {
    notarizeDocs () {
      this.dialog = false
    },
    formIsValid () {
      return (this.file !== '' && this.subject !== '' && this.text !== '' && this.hash !== '')
    },
    readFileContent (inputFile) {
      const temporaryFileReader = new FileReader()

      return new Promise((resolve, reject) => {
        temporaryFileReader.onerror = () => {
          temporaryFileReader.abort()
          reject(new DOMException('Problem parsing input file.'))
        }

        temporaryFileReader.onload = () => {
          resolve(temporaryFileReader.result)
        }
        temporaryFileReader.readAsText(inputFile)
      })
    },
    async loadFileContent (event) {
      const file = event.target.files[0]
      this.file = file
      try {
        const fileContents = await this.readFileContent(file)
        this.hash = this.hashFileContent(fileContents)
      } catch (e) {
        console.log(e.message)
      }
    },
    hashFileContent (fileContent) {
      return sha256.sync(fileContent)
    }
  },
  components: {
    // 'upload-btn': UploadButton
  }
}
</script>
