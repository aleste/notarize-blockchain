<template>
  <div class="notarizations">
    <div class="layout">
      <div class="flex xs12">
         <v-text-field
        solo
        prepend-inner-icon="search"
        label="Por favor indique el ID del documento"
        v-on:keyup.enter="search"
        v-model="documentId"
      ></v-text-field>
      </div>
    </div>
    <div class="layout" v-if="items.length > 0">
      <div class="flex xs12">
        <v-card>
          <v-card-title>
            <h3>Documento #{{documentId}}</h3>
          </v-card-title>
          <v-card-text>
            <DocumentTimeline :items="items"/>
          </v-card-text>
        </v-card>
      </div>
    </div>
    <UploadDialog />
  </div>

</template>

<script>

// import moment from 'moment'
import DocumentTimeline from '@/components/documents/DocumentTimeline'
import UploadDialog from '@/components/documents/UploadDialog'

export default {
  methods: {
    async search () {
      try {
        this.items = []
        const res = await this.$axios.get('items.json')
        this.items = res.data
      } catch (error) {
        console.log(error)
      }
    }
  },
  data () {
    return {
      documentId: null,
      items: []
    }
  },
  components: {
    DocumentTimeline,
    UploadDialog
  }
}
</script>
