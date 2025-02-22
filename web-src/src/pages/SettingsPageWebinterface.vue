<template>
  <div class="fd-page-with-tabs">
    <tabs-settings />
    <content-with-heading>
      <template #heading-left>
        <div class="title is-4" v-text="$t('page.settings.general.language')" />
      </template>
      <template #content>
        <dropdown-menu v-model="locale" :options="locales" />
      </template>
    </content-with-heading>
    <content-with-heading>
      <template #heading-left>
        <div
          class="title is-4"
          v-text="$t('page.settings.general.navigation-items')"
        />
      </template>
      <template #content>
        <p
          class="content"
          v-text="$t('page.settings.general.navigation-item-selection')"
        />
        <div
          class="notification is-size-7"
          v-text="$t('page.settings.general.navigation-item-selection-info')"
        />
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_playlists"
        >
          <template #label>
            <span v-text="$t('page.settings.general.playlists')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_music"
        >
          <template #label>
            <span v-text="$t('page.settings.general.music')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_podcasts"
        >
          <template #label>
            <span v-text="$t('page.settings.general.podcasts')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_audiobooks"
        >
          <template #label>
            <span v-text="$t('page.settings.general.audiobooks')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_radio"
        >
          <template #label>
            <span v-text="$t('page.settings.general.radio')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_files"
        >
          <template #label>
            <span v-text="$t('page.settings.general.files')" />
          </template>
        </settings-checkbox>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_menu_item_search"
        >
          <template #label>
            <span v-text="$t('page.settings.general.search')" />
          </template>
        </settings-checkbox>
      </template>
    </content-with-heading>
    <content-with-heading>
      <template #heading-left>
        <div
          class="title is-4"
          v-text="$t('page.settings.general.album-lists')"
        />
      </template>
      <template #content>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_cover_artwork_in_album_lists"
        >
          <template #label>
            <span v-text="$t('page.settings.general.show-coverart')" />
          </template>
        </settings-checkbox>
      </template>
    </content-with-heading>
    <content-with-heading>
      <template #heading-left>
        <div
          class="title is-4"
          v-text="$t('page.settings.general.now-playing-page')"
        />
      </template>
      <template #content>
        <settings-checkbox
          category_name="webinterface"
          option_name="show_composer_now_playing"
        >
          <template #label>
            <span v-text="$t('page.settings.general.show-composer')" />
          </template>
          <template #info>
            <span v-text="$t('page.settings.general.show-composer-info')" />
          </template>
        </settings-checkbox>
        <settings-textfield
          category_name="webinterface"
          option_name="show_composer_for_genre"
          :disabled="!settings_option_show_composer_now_playing"
          placeholder="Genres"
        >
          <template #label>
            <span v-text="$t('page.settings.general.show-composer-genres')" />
          </template>
          <template #info>
            <p
              class="help"
              v-text="$t('page.settings.general.show-composer-genres-info-1')"
            />
            <p
              class="help"
              v-text="$t('page.settings.general.show-composer-genres-info-2')"
            />
            <p
              class="help"
              v-text="$t('page.settings.general.show-composer-genres-info-3')"
            />
          </template>
        </settings-textfield>
      </template>
    </content-with-heading>
    <content-with-heading>
      <template #heading-left>
        <div
          class="title is-4"
          v-text="$t('page.settings.general.recently-added-page')"
        />
      </template>
      <template #content>
        <settings-intfield
          category_name="webinterface"
          option_name="recently_added_limit"
        >
          <template #label>
            <span
              v-text="$t('page.settings.general.recently-added-page-info')"
            />
          </template>
        </settings-intfield>
      </template>
    </content-with-heading>
  </div>
</template>

<script>
import ContentWithHeading from '@/templates/ContentWithHeading.vue'
import TabsSettings from '@/components/TabsSettings.vue'
import SettingsCheckbox from '@/components/SettingsCheckbox.vue'
import SettingsTextfield from '@/components/SettingsTextfield.vue'
import SettingsIntfield from '@/components/SettingsIntfield.vue'
import DropdownMenu from '@/components/DropdownMenu.vue'

export default {
  name: 'SettingsPageWebinterface',
  components: {
    ContentWithHeading,
    TabsSettings,
    SettingsCheckbox,
    SettingsTextfield,
    SettingsIntfield,
    DropdownMenu
  },

  computed: {
    settings_option_show_composer_now_playing() {
      return this.$store.getters.settings_option_show_composer_now_playing
    },
    locale: {
      get() {
        let languages = this.$i18n.availableLocales
        let locale = languages.find(lang => lang === this.$i18n.locale)
        let partial = this.$i18n.locale.split('-')[0]
        if (!locale) {
          locale = languages.find(lang => lang === partial)
        }
        if (!locale) {
          locale = languages.forEach(lang => lang.split('-')[0] === partial)
        }
        return locale
      },
      set(locale) {
        this.$i18n.locale = locale
      }
    },
    locales: {
      get() {
        return this.$i18n.availableLocales.map((item) => {
          return { id: item, name: this.$t('language.' + item) }
        })
      }
    }
  }
}
</script>

<style></style>
