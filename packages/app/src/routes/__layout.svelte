<script context="module">
	import 'sveltekit-ui/style.css';
	import '$lib/style.css';
	import { goto } from '$app/navigation';
	import { page } from '$app/stores';
	// import AuthTrigger from '$lib/components/Auth/AuthTrigger/index.svelte';
	import { Amplify, Analytics } from 'aws-amplify';
	import awsExports from 'aws-exports';
	Amplify.configure({ ...awsExports, ssr: true });
	import { initAuth } from '$lib/components/Auth/store';
	initAuth();
</script>

<script>
  import AuthModal from '$lib/components/Auth/AuthModal/index.svelte';
	import Logo from '$lib/assets/static/Logo/index.svelte';
	// import { Layout, Button } from 'sveltekit-ui';  
  import Layout from "sveltekit-ui/Layout/index.svelte"
  import Button from "sveltekit-ui/Button/index.svelte"
  import { 
    isDarkTheme, 
    screenWidth, 
    screenHeight, 
    screenWidthThreshold, 
    isSmallScreen, 
    isAppScreen,
    isApp,
    isAllNavBarHidden,
    isNavBarHidableFromScroll,
    isNavBarHiddenFromScroll,
    isNavBarShown,
    isFullNavPrevented,
    isFullNavToggledOn,
    isFullNavShown,
    navBarHeight,
    shownNavBarHeight,
    availableContentHeight
  } from "$lib/store"
  import ContactModal from "$lib/components/ContactModal/index.svelte"
  import {
    openContactModalTrigger
  } from "$lib/components/ContactModal/store";
  import Footer from "$lib/views/Footer/index.svelte"
  // import { navigating } from "$app/stores"
  import { onMount } from "svelte"

  onMount(() => {
    Analytics.autoTrack('session', {
      enable: true,
      provider: 'AWSPinpoint'
    });
    Analytics.autoTrack('pageView', {
      enable: true,
      eventName: 'pageView',
      type: 'SPA',
      provider: 'AWSPinpoint',
      getUrl: () => {
        return window.location.origin + window.location.pathname;
      }
    });
  })

	const fullNavLinks = [
		// { name: 'Home', path: '/' },
		// { name: 'Settings', path: '/settings' }
	];

	const appFullNavLinks = [
		// { name: 'Home', path: '/' },
		// { name: 'Settings', path: '/settings' }
	];

	const navBarLinks = [
		// { name: 'Home', path: '/' },
		// { name: 'Settings', path: '/settings' }
		// { name: 'Settings', path: '/settings', subLinks: [
		//   { name: 'Profile', path: '/profile' },
		// ]},
	];

	const appNavBarLinks = [
		// { name: 'Home', path: '/' },
		// { name: 'Settings', path: '/settings' }
	];

  // let alertAdd

</script>

<Layout
	{fullNavLinks}
	{appFullNavLinks}
	{navBarLinks}
	{appNavBarLinks}
	page={$page}
	{goto}
	isShowHamburger={false}
	bind:isDarkTheme={$isDarkTheme}
	bind:screenWidth={$screenWidth}
	bind:screenHeight={$screenHeight}
	bind:screenWidthThreshold={$screenWidthThreshold}
	bind:isSmallScreen={$isSmallScreen}
	bind:isAppScreen={$isAppScreen}
	bind:isApp={$isApp}
	bind:isAllNavBarHidden={$isAllNavBarHidden}
	bind:isNavBarHidableFromScroll={$isNavBarHidableFromScroll}
	bind:isNavBarHiddenFromScroll={$isNavBarHiddenFromScroll}
	bind:isNavBarShown={$isNavBarShown}
	bind:isFullNavPrevented={$isFullNavPrevented}
	bind:isFullNavToggledOn={$isFullNavToggledOn}
	bind:isFullNavShown={$isFullNavShown}
	bind:navBarHeight={$navBarHeight}
	bind:shownNavBarHeight={$shownNavBarHeight}
	bind:availableContentHeight={$availableContentHeight}
	navMarginX={2}
	borderRadius={4}
>
	<div slot="navBarLogo" class="logoContainer">
		<Logo />
	</div>
	<div slot="appNavBarLogo" class="logoContainer">
		<Logo />
	</div>
	<div slot="extraLinks">
		<!-- <AuthTrigger /> -->
		<Button
			isPill={true}
			pl={2}
			pr={2}
			mr={0}
			type="primary"
			minHeight={5}
			on:click={() => $openContactModalTrigger++}
		>
			<h5>Contact Us</h5>
		</Button>
	</div>
	<div slot="content">
		<slot />
		<Footer />
	</div>
	<div slot="additional">
		<AuthModal />
		<ContactModal />
	</div>
</Layout>

<style>
	.logoContainer {
		min-height: 6rem;
		min-width: 14rem;
		display: flex;
		align-items: center;
	}
	:root {
		--primary: #2ca01c;
		--primary-dark: #108000;
		--primary-light: #53b700;
		--primary-transparent: #1977192a;

		--bg-light: #ffffff;
		/* --bg-med: #f4f4f4; */
		--bg-med: #eef1f8;
		--gray-light: #dddddd;
	}
	:global(body.dark) {
		--primary: #2ca01c;
		--primary-dark: #108000;
		--primary-light: #53b700;
		--primary-transparent: #1977192a;
		--red-error: #d6263d;
		--shadow: #0000003a;
		--shadow-soft: #00000040;
		--shadow-strong: #0000008a;
		--tint: #ffffff3a;
		--tint-strong: #ffffff6a;
		--tint-soft: #ffffff1a;
		--white-light: #efefef;
		--white-med: #e6e6e6;
		--white-dark: #c2c2c2;
		--gray-light: #666666;
		--gray-med: #555555;
		--bg-light: #404040;
		--bg-med: #181a20;
		--bg-med-transparent: #181a20dd;
		--bg-highlight: #242424;
		--bg-highlight-strong: #333333;
		--bg-highlight-stronger: #555555;
		--bg-dark: #000000;
		--contrast-soft: #a4aebc;
		--contrast-med: #eaecef;
		--contrast-strong: #fafcff;
		--contrast-overlay: #ffffff20;
		--bg-modal: #242424;
	}
	:global(*) {
		-webkit-tap-highlight-color: transparent;
		/* color: var(--contrast-med); */
		box-sizing: border-box;
		font-family: 'Lato', 'Avenir Next', 'Helvetica', 'Hiragino Sans', 'Verdana', 'PT Sans',
			'Helvetica Neue';
		font-size: 16px;
		font-size: 1.6rem;
		margin: 0;
	}
	:global(html) {
		text-align: center;
		font-size: 62.5%;
	}
	:global(body) {
		background: var(--bg-med);
		color: var(--contrast-med);
		font-size: 16px;
		font-size: 1.6rem;
		margin: 0;
	}
	:global(h1) {
		font-size: 30px;
		font-size: 3rem;
		font-weight: 700;
		margin: 0;
	}
	:global(h2) {
		font-size: 24px;
		font-size: 2.4rem;
		font-weight: 700;
		margin: 0;
	}
	:global(h3) {
		font-size: 20px;
		font-size: 2rem;
		font-weight: 700;
		margin: 0;
	}
	:global(h4) {
		font-size: 18px;
		font-size: 1.8rem;
		font-weight: 700;
		margin: 0;
	}
	:global(h5) {
		font-size: 16px;
		font-size: 1.6rem;
		font-weight: 700;
		margin: 0;
	}
	:global(div) {
		font-size: 16px;
		font-size: 1.6rem;
	}
	:global(p) {
		font-size: 16px;
		font-size: 1.6rem;
		margin: 0;
	}
	:global(input) {
		font-size: 16px;
		font-size: 1.6rem;
		margin: 0;
	}
	:global(textarea) {
		font-size: 16px;
		font-size: 1.6rem;
		margin: 0;
	}
	:global(::placeholder) {
		color: var(--contrast-soft);
		opacity: 1;
	}
</style>
