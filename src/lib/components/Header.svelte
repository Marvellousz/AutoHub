<!-- Header component for site navigation -->
<script lang="ts">
    import { page } from '$app/stores';
    import { onMount } from 'svelte';
    import { authStore, initAuth, logout } from '$lib/auth/auth';
    import { initTheme } from '$lib/stores/themeStore';
    
    // Initialize auth and theme on mount
    onMount(() => {
        initAuth();
        initTheme();
    });
    
    // Handle logout
    function handleLogout() {
        logout();
        // Redirect to home page
        window.location.href = '/';
    }

    // Mobile menu state
    let isMobileMenuOpen = false;
    
    // Toggle mobile menu
    function toggleMobileMenu() {
        isMobileMenuOpen = !isMobileMenuOpen;
    }
</script>

<header class="bg-white shadow-md dark:bg-gray-900 dark:shadow-gray-800">
    <div class="container mx-auto px-4">
        <nav class="flex items-center justify-between py-4">
            <a href="/" class="text-xl font-bold text-blue-600 dark:text-blue-400 flex items-center">
                <i class="fas fa-robot mr-2"></i> AutoHub
            </a>
            
            <!-- Mobile menu button -->
            <button 
                class="md:hidden p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all"
                on:click={toggleMobileMenu}
                aria-label="Toggle mobile menu"
            >
                <i class="fas {isMobileMenuOpen ? 'fa-times' : 'fa-bars'} text-gray-700 dark:text-gray-300"></i>
            </button>            <!-- Navigation menu -->
            <ul class={`hidden md:flex space-x-3 items-center lg:space-x-6 ${isMobileMenuOpen ? '!flex flex-col fixed inset-x-0 top-[72px] bg-white dark:bg-gray-900 p-4 space-y-4 shadow-lg border-t border-gray-200 dark:border-gray-700 z-50' : ''}`}>
                <li class="md:w-auto w-full">
                    <a href="/" class={`px-3 py-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all block w-full md:w-auto ${$page.url.pathname === '/' ? 'text-blue-600 dark:text-blue-400 font-medium' : 'text-gray-700 dark:text-gray-300'}`}>
                        <i class="fas fa-home mr-1"></i> Home
                    </a>
                </li>
                <li class="md:w-auto w-full">
                    <a href="/scripts" class={`px-3 py-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all block w-full md:w-auto ${$page.url.pathname === '/scripts' ? 'text-blue-600 dark:text-blue-400 font-medium' : 'text-gray-700 dark:text-gray-300'}`}>
                        <i class="fas fa-code mr-1"></i> Browse Scripts
                    </a>
                </li>
                <li class="md:w-auto w-full">
                    <a href="/submit" class={`px-3 py-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all block w-full md:w-auto ${$page.url.pathname === '/submit' ? 'text-blue-600 dark:text-blue-400 font-medium' : 'text-gray-700 dark:text-gray-300'}`}>
                        <i class="fas fa-upload mr-1"></i> Submit Script
                    </a>
                </li>
                <li class="md:w-auto w-full">
                    <a href="/about" class={`px-3 py-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all block w-full md:w-auto ${$page.url.pathname === '/about' ? 'text-blue-600 dark:text-blue-400 font-medium' : 'text-gray-700 dark:text-gray-300'}`}>
                        <i class="fas fa-info-circle mr-1"></i> About
                    </a>
                </li>
                <!-- Authentication Links -->
                {#if $authStore.isAuthenticated}
                    <li class="relative group md:w-auto w-full">
                        <button class="flex items-center text-gray-700 dark:text-gray-300 hover:text-blue-600 dark:hover:text-blue-400 focus:outline-none px-3 py-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-all w-full md:w-auto">
                            <i class="fas fa-user-circle mr-1"></i>
                            <span>{$authStore.user?.username || 'User'}</span>
                            <i class="fas fa-chevron-down ml-1 text-xs"></i>
                        </button>
                        <div class="{isMobileMenuOpen ? 'relative md:absolute w-full mt-2' : 'absolute'} right-0 w-48 py-2 mt-1 bg-white dark:bg-gray-800 rounded-lg shadow-xl {isMobileMenuOpen ? 'block' : 'invisible group-hover:visible'} z-10 border border-gray-200 dark:border-gray-700 transition-all">
                            <a href="/profile" class="flex items-center px-4 py-2 text-gray-700 dark:text-gray-200 hover:bg-blue-500 hover:text-white transition-colors">
                                <i class="fas fa-id-card mr-2"></i> Profile
                            </a>
                            <button on:click={handleLogout} class="flex items-center w-full text-left px-4 py-2 text-gray-700 dark:text-gray-200 hover:bg-blue-500 hover:text-white transition-colors">
                                <i class="fas fa-sign-out-alt mr-2"></i> Logout
                            </button>
                        </div>
                    </li>
                {:else}
                    <li class="md:w-auto w-full">
                        <a href="/login" class={`px-4 py-2 rounded-lg hover:bg-blue-100 dark:hover:bg-blue-900/40 transition-all block w-full md:w-auto text-center ${$page.url.pathname === '/login' ? 'text-blue-600 dark:text-blue-400 font-bold' : 'text-gray-700 dark:text-gray-300'}`}>
                            <i class="fas fa-sign-in-alt mr-1"></i> Login
                        </a>
                    </li>
                    <li class="md:w-auto w-full">
                        <a href="/register" class={`px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-all shadow-sm block w-full md:w-auto text-center ${$page.url.pathname === '/register' ? 'font-bold' : ''}`}>
                            <i class="fas fa-user-plus mr-1"></i> Register
                        </a>
                    </li>
                {/if}
            </ul>
        </nav>
    </div>
</header>

<style>
    /* All styling handled by Tailwind CSS classes */
</style>
