<script>
    import BreezeGuestLayout from "@/Layouts/Breeze/Guest.svelte";
    import BreezeCheckbox from "@/Components/Breeze/Checkbox.svelte";
    import BreezeInput from "@/Components/Breeze/Input.svelte";
    import BreezeApplicationLogo from "@/Components/Breeze/ApplicationLogo.svelte";
    import BreezeValidationErrors from "@/Components/Breeze/ValidationErrors.svelte";
    import { inertia, Link, useForm } from "@inertiajs/svelte";
    let err = {};
    export let errors = {};
    export let status;
    let showPassword = false;

    const form = useForm({
        input_type: null,
        password: null,
        remember: false,
    });

    $: {
        err = errors;
    }

    const onSubmit = () => {
        $form.post("/login", {
            onSuccess: () => $form.reset(),
        });
    };
</script>

<svelte:head>
    <title>Log in</title>
</svelte:head>

<BreezeGuestLayout>
    <BreezeValidationErrors class="mb-4" errors={err} />

    {#if status}
        <div class="mb-4 font-medium text-sm text-green-600">
            {status}
        </div>
    {/if}

    <div class="flex min-h-full flex-col justify-center py-12 sm:px-6 lg:px-8">
        <div class="sm:mx-auto sm:w-full sm:max-w-md">
            <BreezeApplicationLogo class="mx-auto h-10 w-auto" />
            <h2 class="mt-6 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">Sign in to your account</h2>
        </div>

        <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-[480px]">
            <div class="bg-white px-6 py-12 shadow sm:rounded-lg sm:px-12">
                <form class="space-y-6" on:submit|preventDefault={onSubmit}>
                    <div>
                        <label for="input_type" class="block text-sm font-medium leading-6 text-gray-900">Email/Username</label>
                        <div class="mt-2">
                            <BreezeInput
                                id="input_type"
                                type="text"
                                value={form.input_type}
                                autofocus
                                autocomplete="username"
                                on:input={(evt) => ($form.input_type = evt.detail)}
                            />
                        </div>
                    </div>

                    <div>
                        <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
                        <div class="mt-2 flex rounded-md shadow-sm">
                            <div class="relative flex flex-grow items-stretch focus-within:z-10">
                                <BreezeInput
                                    id="password"
                                    class="rounded-none rounded-l-md"
                                    type={showPassword ? 'text' : 'password'}
                                    value={form.password}
                                    autocomplete="current-password"
                                    on:input={(evt) => ($form.password = evt.detail)}
                                />
                            </div>
                            <button on:click={()=>showPassword = !showPassword} type="button" class="relative -ml-px inline-flex items-center gap-x-1.5 rounded-r-md px-3 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
                                {#if showPassword}
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="-ml-0.5 h-5 w-5 text-gray-400" aria-hidden="true"><path d="M12 15a3 3 0 1 0 0-6 3 3 0 0 0 0 6Z" /><path fill-rule="evenodd" d="M1.323 11.447C2.811 6.976 7.028 3.75 12.001 3.75c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113-1.487 4.471-5.705 7.697-10.677 7.697-4.97 0-9.186-3.223-10.675-7.69a1.762 1.762 0 0 1 0-1.113ZM17.25 12a5.25 5.25 0 1 1-10.5 0 5.25 5.25 0 0 1 10.5 0Z" clip-rule="evenodd" /></svg>
                                {:else}
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="-ml-0.5 h-5 w-5 text-gray-400" aria-hidden="true"><path d="M3.53 2.47a.75.75 0 0 0-1.06 1.06l18 18a.75.75 0 1 0 1.06-1.06l-18-18ZM22.676 12.553a11.249 11.249 0 0 1-2.631 4.31l-3.099-3.099a5.25 5.25 0 0 0-6.71-6.71L7.759 4.577a11.217 11.217 0 0 1 4.242-.827c4.97 0 9.185 3.223 10.675 7.69.12.362.12.752 0 1.113Z" /><path d="M15.75 12c0 .18-.013.357-.037.53l-4.244-4.243A3.75 3.75 0 0 1 15.75 12ZM12.53 15.713l-4.243-4.244a3.75 3.75 0 0 0 4.244 4.243Z" /><path d="M6.75 12c0-.619.107-1.213.304-1.764l-3.1-3.1a11.25 11.25 0 0 0-2.63 4.31c-.12.362-.12.752 0 1.114 1.489 4.467 5.704 7.69 10.675 7.69 1.5 0 2.933-.294 4.242-.827l-2.477-2.477A5.25 5.25 0 0 1 6.75 12Z" /></svg>
                                {/if}
                            </button>
                        </div>
                    </div>

                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <BreezeCheckbox name="remember" bind:checked={form.remember} />
                            <label for="remember" class="ml-3 block text-sm leading-6 text-gray-900">Remember me</label>
                        </div>

                        <div>
                            <a
                                href="/register"
                                use:inertia
                                class="underline text-sm text-gray-600 hover:text-gray-900"
                            >
                                Not registered?
                            </a>
                        </div>
                    </div>

                    <div>
                        <button type="submit" sclass:opacity-25={form.processing} class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600" disabled={form.processing}>Sign in</button>
                    </div>
                </form>

                <div>
                    <div class="relative mt-10">
                        <div class="absolute inset-0 flex items-center" aria-hidden="true">
                            <div class="w-full border-t border-gray-200"></div>
                        </div>
                        <div class="relative flex justify-center text-sm font-medium leading-6">
                            <span class="bg-white px-6 text-gray-900">Or continue with</span>
                        </div>
                    </div>

                    <div class="mt-6 grid grid-cols-2 gap-4">
                        <a href="/auth/google/redirect" class="flex w-full items-center justify-center gap-3 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus-visible:ring-transparent">
                            <svg class="h-5 w-5" viewBox="0 0 24 24" aria-hidden="true">
                                <path d="M12.0003 4.75C13.7703 4.75 15.3553 5.36002 16.6053 6.54998L20.0303 3.125C17.9502 1.19 15.2353 0 12.0003 0C7.31028 0 3.25527 2.69 1.28027 6.60998L5.27028 9.70498C6.21525 6.86002 8.87028 4.75 12.0003 4.75Z" fill="#EA4335" />
                                <path d="M23.49 12.275C23.49 11.49 23.415 10.73 23.3 10H12V14.51H18.47C18.18 15.99 17.34 17.25 16.08 18.1L19.945 21.1C22.2 19.01 23.49 15.92 23.49 12.275Z" fill="#4285F4" />
                                <path d="M5.26498 14.2949C5.02498 13.5699 4.88501 12.7999 4.88501 11.9999C4.88501 11.1999 5.01998 10.4299 5.26498 9.7049L1.275 6.60986C0.46 8.22986 0 10.0599 0 11.9999C0 13.9399 0.46 15.7699 1.28 17.3899L5.26498 14.2949Z" fill="#FBBC05" />
                                <path d="M12.0004 24.0001C15.2404 24.0001 17.9654 22.935 19.9454 21.095L16.0804 18.095C15.0054 18.82 13.6204 19.245 12.0004 19.245C8.8704 19.245 6.21537 17.135 5.2654 14.29L1.27539 17.385C3.25539 21.31 7.3104 24.0001 12.0004 24.0001Z" fill="#34A853" />
                            </svg>
                            <span class="text-sm font-semibold leading-6">Google</span>
                        </a>

                        <a href="/auth/github/redirect" class="flex w-full items-center justify-center gap-3 rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus-visible:ring-transparent">
                            <svg class="h-5 w-5 fill-[#24292F]" fill="currentColor" viewBox="0 0 20 20" aria-hidden="true">
                                <path fill-rule="evenodd" d="M10 0C4.477 0 0 4.484 0 10.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0110 4.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.203 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.942.359.31.678.921.678 1.856 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0020 10.017C20 4.484 15.522 0 10 0z" clip-rule="evenodd" />
                            </svg>
                            <span class="text-sm font-semibold leading-6">GitHub</span>
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</BreezeGuestLayout>
