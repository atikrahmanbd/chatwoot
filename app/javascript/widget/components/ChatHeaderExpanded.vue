<script>
import HeaderActions from './HeaderActions.vue';
import { useDarkMode } from 'widget/composables/useDarkMode';
import { mapGetters } from 'vuex';

export default {
  name: 'ChatHeaderExpanded',
  components: {
    HeaderActions,
  },
  props: {
    avatarUrl: {
      type: String,
      default: '',
    },
    introHeading: {
      type: String,
      default: '',
    },
    introBody: {
      type: String,
      default: '',
    },
    showPopoutButton: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    ...mapGetters({
      widgetColor: 'appConfig/getWidgetColor',
    }),
    textColor() {
      return getContrastingTextColor(this.widgetColor);
    }
  },
  setup() {
    const { getThemeClass } = useDarkMode();
    return { getThemeClass };
  },
};
</script>

<template>
  <header
    class="header-expanded pt-6 pb-4 px-5 relative box-border w-full bg-transparent"
    :style="{background: `linear-gradient(45deg, rgb(0, 0, 0) 0%, ${widgetColor} 100%)`}"
  >
    <div
      class="flex items-start"
      :class="[avatarUrl ? 'justify-between' : 'justify-end']"
    >
    <span class="rounded-full bg-white shadow-md">
      <img
          v-if="avatarUrl"
          class="h-12 p-2"
          :src="avatarUrl"
          alt="Avatar"
        />
    </span>
      <HeaderActions
        :show-popout-button="showPopoutButton"
        :show-end-conversation-button="false"
      />
    </div>
    <h2
      v-dompurify-html="introHeading"
      class="mt-4 text-2xl mb-1.5 font-medium"
      :class="getThemeClass('text-slate-50', 'dark:text-white')"
    />
    <p
      v-dompurify-html="introBody"
      class="text-base leading-normal"
      :class="getThemeClass('text-slate-100', 'dark:text-slate-50')"
    />
  </header>
</template>
