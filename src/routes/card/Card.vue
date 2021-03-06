<template lang="pug">
.route-card
  css-transition(
    effect="simpleFlip"
    direction="xInvert"
    mode="out-in"
    auto-height
    :disabled="!prefs.transitionsEnabled"
  )
    .box(:class="{'is-expanded': isBoxExpanded}" v-if="!isFlipped" key="1")
      //- corner menus/buttons
      .top.left
        el-popover(placement="top" trigger="hover" popper-class='settings-menu')
          button.button.is-text(slot='reference' aria-label='settings')
            icon(name='cog' scale=1.5)

          .buttons(style='margin-bottom: 0;')
            el-tooltip(
              :content="`Transitions: ${prefs.transitionsEnabled ? 'ON' : 'OFF'}`"
              placement='top'
            )
              button.button.is-text(
                :class="{'has-text-grey-lighter': !prefs.transitionsEnabled}"
                @click="toggleTransitionsEnabled()"
              )
                icon(name='exchange-alt' scale=1.5)

            el-tooltip(
              :content="`Lights: ${prefs.darkModePreference === 0 ? 'Auto' : !darkMode ? 'ON' : 'OFF'}`"
              placement="top"
            )
              button.button.is-text(
                :class="{'has-text-grey-lighter': darkMode}"
                @click='cycleDarkMode()'
              )
                icon(:name="prefs.darkModePreference === 0 ? 'regular/clock' : 'regular/lightbulb'" scale=1.5)

            //- el-tooltip(content='Theme: ' placement='top')
            //-   button.button.is-text(@click='changeTheme()')
            //-     icon(name='fill-drip' scale='2')

      .top.right
        button.button.is-text(@click="isFlipped = true" aria-label="Contact information")
          icon(name='address-card' scale=1.5)
          icon(name='chevron-right' scale=1 style='width: .75rem;')

      div.has-text-centered

        //- branding
        span.name.is-brand-font Caleb Roseland
        span.sub Web App Developer

        css-transition(effect="fade" auto-height :disabled="!prefs.transitionsEnabled || !contentTransitions")
          div.sub-experience(v-show="isBoxExpanded")
            div
              .field.is-grouped.is-grouped-multiline
                .control(v-for="{label, since} in tags")
                  .tags.has-addons
                    span.tag(:class="`${darkMode ? 'is-black' : 'is-info'}`") {{label}}

        //- menu system
        .menu.columns.links(:class="{'is-flex-mobile has-text-centered-mobile': !isBoxExpanded}")
          //- github
          ul.menu-list.column
            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                p.menu-label
                  span Projects/Repos
                  icon(name="external-link-square-alt" scale=.75)

            li
              external-link(to='https://github.com/calebroseland')
                icon(name='brands/github' scale=1.75)
                span(:class="{'is-hidden-mobile': !isBoxExpanded}")  GitHub

            css-transition(v-bind="contentTransitionProps")
              li(v-show='isBoxExpanded')
                ul
                  li
                    //- http://calebroseland.com/card#andwereback
                    external-link(to='https://github.com/calebroseland/calebroseland-com/blob/master/src/routes/card/Card.vue#L83')
                      span
                        code this
                  li
                    external-link(to='https://github.com/calebroseland/vue-dom-portal')
                      span vue-dom-portal
                ul
                  li
                    external-link(to='https://gist.github.com/calebroseland')
                      span Gists

          //- medium
          ul.menu-list.column
            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                p.menu-label
                  span Writings
                  icon(name="external-link-square-alt" scale=.75)
            li
              external-link(to='https://medium.com/@calebroseland')
                icon(name='brands/medium' scale=1.75)
                span(:class="{'is-hidden-mobile': !isBoxExpanded}")  Medium
            css-transition(v-bind="contentTransitionProps")
              li(v-show='isBoxExpanded')
                ul
                  li
                    external-link(to='https://medium.com/@calebroseland/state-management-in-vue-525ffe12ad81')
                      span State Management in Vue
                ul
                  li
                    external-link(to='https://medium.com/@calebroseland/has-recommended')
                      span Claps
                  li
                    external-link(to='https://medium.com/@calebroseland/highlights')
                      span Highlights

          //- social links
          ul.menu-list.column
            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                p.menu-label
                  span Social
                  icon(name="external-link-square-alt" scale=.75)

            li
              external-link(to='https://linkedin.com/in/calebroseland')
                icon(name='brands/linkedin' scale=1.75)
                span(:class="{'is-hidden-mobile': !isBoxExpanded}")  LinkedIn

            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                external-link(to='https://twitter.com/calebroseland')
                  icon(name='brands/twitter' scale=1.75)
                  span  Twitter

            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                external-link(to='https://npmjs.com/~calebroseland')
                  icon(name='brands/npm' scale=1.75)
                  span  npm

            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                external-link(to='https://stackoverflow.com/users/1352410')
                  icon(name='brands/stack-overflow' scale=1.75)
                  span  Stack Overflow

            css-transition(v-bind="contentTransitionProps")
              li(v-show="isBoxExpanded")
                external-link(to='https://pluralsight.com/profile/caleb-roseland')
                  icon(name='pluralsight' scale=1.75)
                  span  Pluralsight

        //- expand button
        button.button.is-text.is-fullwidth(@click='isBoxExpanded = !isBoxExpanded' aria-label='Expand')
          span show {{isBoxExpanded ? 'less' : 'more'}}
          //icon(:name="isBoxExpanded ? 'chevron-circle-up' : 'chevron-circle-down'" scale=2)
    //- reverse side
    .box.flipped(v-else key=2)
      .left.has-text-left
        p.name.is-brand-font Caleb Roseland
        p.sub Web App Developer
        .details
          p
            a(href='tel:+1 507 476 1225')
              i.el-icon-phone
              span  +1 507 476 1225
          p
            a(href='mailto:caleb@calebroseland.com')
              i.el-icon-message
              span  caleb@calebroseland.com
          p
            external-link(to='https://maps.google.com/?q=Minnesota,+USA')
              i.el-icon-location
              span  Minnesota, USA
      .right.has-text-right
        button.button.is-text(@click="toCard()")
          icon(name="times" scale=1.5)

</template>

<script>
import './icons.ts'
import { Popover, Checkbox } from 'element-ui'
import { CssTransition } from '@/components/transitions'
import { mapState, mapGetters, mapActions } from 'vuex'
import { DARK_MODE } from '@/store/modules/preferences/types'

export default {
  transition: {
    effect: 'tada',
    flow: null
  },
  name: 'card',
  components: {
    [Popover.name]: Popover,
    [Checkbox.name]: Checkbox,
    CssTransition
  },
  data() {
    return {
      isBoxExpanded: false,
      isFlipped: false,
      contentTransitions: true,
      tags: [
        {
          label: 'JavaScript',
          since: '2012'
        },
        {
          label: 'Vue.js',
          since: '2015'
        },
        {
          label: 'ASP.NET',
          since: '2014'
        }
      ]
    }
  },
  computed: {
    ...mapState('preferences', {
      prefs: state => state
    }),
    ...mapGetters('preferences', [DARK_MODE]),
    contentTransitionProps() {
      return {
        effect: 'fade',
        autoHeight: true,
        autoWidth: true,
        disabled: !this.prefs.transitionsEnabled || !this.contentTransitions
      }
    }
  },
  methods: {
    ...mapActions('preferences', ['toggleTransitionsEnabled', 'cycleDarkMode']),
    toCard() {
      this.contentTransitions = false
      this.isFlipped = false
      setTimeout(() => {
        this.contentTransitions = true
      }, 1000)
    }
  }
}
</script>

<style lang="scss">
@import '@/components/transitions/animations/simpleFlip';
</style>

<style lang="sass" scoped>
@import '~open-color/open-color.scss'
@import '~bulma/sass/utilities/_all.sass'

.route-card
  $page-bg: $oc-blue-6
  display: flex
  flex-direction: column
  align-items: center
  justify-content: center

  body &
    background-color: $page-bg

  body[dark-mode] &
    background-color: darken($oc-blue-9, 30%)

.box
  display: flex
  position: relative
  flex-direction: column
  margin: 3rem 0
  //padding: 1.25rem 0 0 0
  font-size: 1.5rem
  max-width: 100vw
  body[has-transitions] &
    animation-duration: .9s
    transition-duration: .9s
    animation-timing-function: ease
    transition: background-color .3s ease-in-out, color .3s ease, height .75s ease
    .is-text,
    .menu-list a
      transition: color .3s ease

  overflow: hidden

  body[dark-mode] &
    background: $oc-gray-9
    color: whitesmoke

    a
      color: whitesmoke
      &:hover,
      &:focus
        color: white

    .is-text,
    .menu-list a
      color: whitesmoke
      &:hover:not(:active),
      &:focus:not(:active)
        background-color: $oc-gray-8
        color: white

      &:active
        background-color: $oc-gray-7

      &:focus:not(:active),
      &.is-focused:not(:active)
        box-shadow: 0 0 0 0.125em transparentize($oc-gray-6, 0.25)

      body[has-transitions] &
         transition: background-color .3s ease, color .3s ease

span.name
  font-size: 3rem
  line-height: 1.25
  display: block
  text-align: center
  margin-top: 1rem
  +mobile
    margin-top: 2rem

.sub-experience
  > div
    padding: .5rem
  body[has-transitions] &
    overflow: visible
    animation-duration: .5s
  .field
    justify-content: center
  .tag
    body[has-transitions] &
      transition: background-color .3s ease, color .3s ease

span.sub
  text-align: center
  .inline-icons
    margin-left: .5rem
  .icon
    vertical-align: middle

.links
  padding: 1rem 0 0
  margin: 0
  white-space: nowrap
  .icon + span
    padding-left: .25rem

.top
  position: absolute
  top: .5rem
  line-height: 1
  &.left
    left: .5rem

  &.right
    right: .5rem

.settings-menu
  button
    margin-bottom: 0

.menu
  text-align: left

  a
    font-size: 1.25rem
    line-height: 1

  .menu-label
    padding-left: .95rem
    margin-bottom: .5rem
    line-height: 2

  .menu-list
    ul
      // gotta keep those pixels aligned
      margin-left: .95rem
      padding-left: 13px

.el-button
  font-size: 2rem

.cta-primary
  margin-bottom: 3rem

.box.flipped
  display: flex
  justify-content: space-between
  flex-direction: row
  margin: 3rem 0
  font-size: 1.25rem
  +tablet
    width: 30rem

  .route-contact
    display: flex
    flex-direction: column
    align-items: center
    justify-content: center

  .details
    margin: 2rem 0 0

  .left
    padding: .25rem 0 1rem 1rem

  .close
    padding: .5rem

  .name
    font-size: 2.5rem

</style>

<style>
.el-popover.settings-menu {
  min-width: 100px;
}
</style>
