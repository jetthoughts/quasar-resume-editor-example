<template>
  <q-layout view="hHh lpR fFf" class="bg-grey-1">
    <q-header elevated class="bg-white text-grey-8 q-py-xs" height-hint="58">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          aria-label="Menu"
          icon="menu"
        />

        <q-btn flat no-caps no-wrap class="q-ml-xs" v-if="$q.screen.gt.xs">
          <q-toolbar-title shrink class="text-weight-bold">
            Resume Editor
          </q-toolbar-title>
        </q-btn>

        <q-space />

        <div class="q-gutter-sm row items-center no-wrap">
          <q-btn color="primary" label="Print" />
          <q-btn color="primary" label="Download" />
        </div>
      </q-toolbar>
    </q-header>

    <div class="fixed-left" style="z-index: 99">
      <q-drawer
        v-model="leftDrawerOpen"
        show-if-above
        bordered
        content-class="bg-grey-2"
        :width="240"
      >
        <q-scroll-area class="fit">
          <q-list padding>
            <q-item v-for="link in links" :key="link.text" v-ripple clickable @click="goto(link.value)">
              <q-item-section avatar>
                <q-icon color="grey" :name="link.icon" />
              </q-item-section>
              <q-item-section>
                <q-item-label>{{ link.text }}</q-item-label>
              </q-item-section>
            </q-item>

            <q-separator class="q-mt-md q-mb-xs" />

          </q-list>
        </q-scroll-area>
      </q-drawer>
    </div>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',
  methods: {
    goto (value) {
      const element = document.getElementById(value)
      element.scrollIntoView({ behavior: 'smooth', block: 'start' })
    }
  },

  data () {
    return {
      leftDrawerOpen: false,
      links: [
        { icon: 'photo', text: 'Photo', value: 'photo' },
        { icon: 'edit', text: 'Name', value: 'name' },
        { icon: 'title', text: 'Subtitle', value: 'subtitle' },
        { icon: 'contacts', text: 'Contacts', value: 'contacts' },
        { icon: 'work', text: 'Experience', value: 'experience' },
        { icon: 'school', text: 'Education', value: 'education' },
        { icon: 'sports_football', text: 'Skills', value: 'skills' },
        { icon: 'language', text: 'Languages', value: 'languages' },
        { icon: 'cake', text: 'Summary', value: 'summary' }
      ]
    }
  }
}
</script>
