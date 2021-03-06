<template>
    <div :class="['navigation-container', {'open':navigationOpenState}, {'endabled':navigationIsEnabled}]">
		<div
			:class="['navigation-skrim', {'enabled':navigationOpenState}]"
			@click="toggleNavigationMenu" />
        <div class="navigation-poly">
            <nav>
                <navigation-item
                    v-for="item in navItems"
                    :key="`${item.label}-${$uuid.v4()}`"
                    v-match-route:class.active="item.routeName"
                    :text="item.label"
                    :styles="{ color:'#646468', fontSize:'3rem' }"
                    @click.native="navigateToRoute({ name: item.routeName })"/>
            </nav>
        </div>
        <header :class="[{ open:headerState }]">
            <div class="header--logo-container">
                <div
                    class="app-logo"
                    @click="navigateToRoute({ name: 'home' })">
                    <img :src="headerLogo"/>
                </div>
                <div class="divider"/>
                <div class="breadcrumb">
                    <p>{{routeName}}</p>
                </div>
            </div>
            <div
                class="nav-menu-button"
                @click="toggleNavigationMenu">
                <hamburger-menu :menu-state="navigationOpenState"/>
            </div>
        </header>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'

import {
	VUEX_UI_NAVIGATION_TOGGLE_OPENSTATE,
	VUEX_UI_HEADER_LOGO
} from '@/store/constants/ui'
import {
	VUEX_ROUTING_PUSH_ROUTE
} from '@/store/constants/routing'

import sitenav from '@/config/sitenav'

import HamburgerMenu from '@/components/_global/Hamburger_Menu'
import NavigationItem from '@/components/Navigation/Navigation_Item'

export default {
	name: 'navigation',

	components: {
		'hamburger-menu': HamburgerMenu,
		'navigation-item': NavigationItem
	},

	data: () => ({
		navItems: sitenav
	}),

	computed: {
		...mapState({
			headerState: state => state.ui.headerState,
			navigationOpenState: state => state.ui.navigation.openState,
			navigationIsEnabled: state => state.ui.navigation.isEnabled,
			routeTitle: state => state.ui.navigation.title
		}),

		headerLogo () {
			return VUEX_UI_HEADER_LOGO
		},

		routeName () {
			return this.routeTitle ? this.routeTitle : this.$route.name
		}
	},

	methods: {
		...mapActions({
			toggleNavigationMenu: VUEX_UI_NAVIGATION_TOGGLE_OPENSTATE,
			navigateToRoute: VUEX_ROUTING_PUSH_ROUTE
		})
	}
}
</script>
