<template>

    <v-timeline align-top>
            <v-timeline-item
            v-for="(item, i) in items"
            :key="i"
            :color="item.color"
            :icon="item.icon"
            fill-dot
            small
            >

            <span slot="opposite">
            <p>Notarized by {{ item.user }} {{ item.datetime }}</p>

            </span>
            <v-card
                :color="item.color"
                dark
            >
                <v-card-title class="title">Version #{{item.version}}</v-card-title>
                <v-card-text class="white text--primary">
                    <p class="subject">{{ item.subject }}</p>
                    <p>{{ item.text }}</p>
                    <p class="caption text-truncate"><span class="grey--text">{{item.hash}}</span></p>
                    <v-btn
                        :color="item.color"
                        class="mx-0"
                        outline
                    >
                        Validar
                    </v-btn>
                </v-card-text>
            </v-card>
            </v-timeline-item>
        </v-timeline>

</template>

<script>

export default {
  props: ['items']
}
</script>

<style scoped>

p.subject {
  font-size: 1.2em;
  font-weight: 300;
}

</style>
