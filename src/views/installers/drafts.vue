<template>
  <div class="app-container">
    <submission-table
      :loading="draftsLoading"
      :submissions="drafts"
    />
  </div>
</template>

<script>
import SubmissionTable from './submissionTable'
import { fetchDrafts } from '@/api/installers'
export default {
  name: 'Drafts',
  components: { SubmissionTable },
  data() {
    return {
      drafts: null,
      draftsLoading: true
    }
  },
  created() {
    this.getDrafts()
  },
  methods: {
    getDrafts() {
      this.draftsLoading = true
      fetchDrafts().then(response => {
        this.drafts = []
        for (let i = 0; i < response.data.results.length; i++) {
          const draft = response.data.results[i]
          if (draft.version_set.length === 0) {
            continue
          }
          this.drafts.push(draft)
        }
        this.draftsLoading = false
      })
    }
  }
}
</script>
