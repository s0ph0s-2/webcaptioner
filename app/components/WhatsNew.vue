<template>
  <div>
    <div
      v-for="releaseNoteEntry in changelogSortedByVersionNumber"
      v-bind:key="releaseNoteEntry.version"
    >
      <!-- <h3 class="mt-4" v-if="hideTitle !== true">{{formatDate(releaseNoteEntry.date)}} <small class="text-dark pl-3 text-capitalize">Version {{releaseNoteEntry.version}}</small></h3> -->
      <h3 class="mt-4" v-if="hideTitle !== true">{{releaseNoteEntry.version}}</h3>
      <div
        v-if="Array.isArray(releaseNoteEntry.notes) && releaseNoteEntry.notes.length > 0"
        :class="hideTitle ? '' : 'pt-2'"
      >
        <div
          class="media"
          :class="{'mb-4' : index != releaseNoteEntry.notes.length - 1}"
          v-for="(note, index) in releaseNoteEntry.notes"
          v-bind:key="note.text"
        >
          <img class="mr-3" style="width:60px" :src="'/static/update-icons/' + note.icon" alt>
          <div class="media-body" :class="{'pt-3' : !note.title, 'small': small}">
            <h5
              class="mt-0 mb-1"
              v-if="note.title"
              :style="{'fontSize': small ? '1rem' : ''}"
            >{{note.title}}</h5>
            <span v-html="note.text"/>
          </div>
        </div>
      </div>
      <div v-else-if="typeof releaseNoteEntry.notes === 'string'" v-html="releaseNoteEntry.notes"></div>
      <div v-else class="ml-4">
        <fa :icon="['far', 'thumbs-up']" class="mr-1"/>Just working on making some stuff run better.
      </div>
    </div>
  </div>
</template>

<script>
import changelog from '~/mixins/data/changelog.js';
import { compareVersions } from 'compare-versions';
import dateFormatMixin from '~/mixins/dateFormat';

export default {
  name: 'whatsNew',
  mixins: [dateFormatMixin],
  props: ['limit', 'hideTitle', 'small'],
  computed: {
    changelogSortedByVersionNumber: function() {
      let sortedChangelog = changelog.sort(compareVersions);
      return sortedChangelog.slice(0, this.limit || 10);
    },
  },
  methods: {
    formatDate: function(date) {
      return this.dateFormat(date, 'MMM. D, yyyy');
    },
  },
};
</script>
