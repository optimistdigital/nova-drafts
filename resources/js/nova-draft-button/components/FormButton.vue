<template>
  <div>
    <button
      ref="createNovaDraftButton"
      type="button"
      class="ml-3 btn btn-default btn-primary"
      id="create-draft-button"
      v-on:click="createDraft"
      v-if="!field.isDraft"
    >
      {{ __('novaDrafts.createDraftButtonText') }}
    </button>

    <input name="draft" v-model="draft" type="hidden" />
  </div>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova';

export default {
  mixins: [FormField, HandlesValidationErrors],
  props: ['resource', 'resourceId', 'field'],

  data() {
    return {
      draft: void 0,
      postId: this.resourceId,
    };
  },

  mounted() {
    if (!this.field.isDraft) {
      const positionDraftButton = this.actionButton.parentNode.querySelector('[id=create-draft-button]');
      if (positionDraftButton && positionDraftButton !== this.$refs.createNovaDraftButton) {
        this.actionButton.parentNode.removeChild(positionDraftButton);
      }
      this.actionButton.parentNode.append(this.$refs.createNovaDraftButton);
    }
  },

  methods: {
    fill(formData) {
      if (this.draft) {
        formData.append(this.field.attribute, this.draft);
      }
    },

    createDraft() {
      this.draft = true;

      this.$nextTick(() => {
        this.actionButton.click();
      });
    },
  },

  computed: {
    actionButton() {
      return document
        .querySelector('.content')
        .querySelector(`[dusk="${!!this.resourceId ? 'update-button' : 'create-button'}"]`);
    },
  },
};
</script>
