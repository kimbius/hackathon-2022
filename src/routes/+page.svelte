<script>
    let on_analyze = false;
    let fileinput;
    let imgpreview = "";
    let star = 2;
    let is_fetching = false;

    import { fly } from "svelte/transition";

    const analyze = (img) => {
        is_fetching = true;
        setTimeout(() => {
            is_fetching = false;
            on_analyze = true;
            imgpreview = img;
        }, 1000);
    };

    const onFileSelected = (e) => {
        let image = e.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(image);
        reader.onload = (e) => {
            analyze(e.target.result);
            fileinput.value = "";
        };
    };
</script>

<div class="min-h-screen container flex items-center justify-center py-10">
    <div class="bg-gray-100 p-3 rounded w-full max-w-xl">
        {#if is_fetching}
            <div class="flex justify-center text-gray-300 py-5">
                <svg
                    version="1.1"
                    id="L4"
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    width="100"
                    height="100"
                    viewBox="-20 0 100 100"
                    enable-background="new 0 0 0 0"
                    xml:space="preserve"
                >
                    <circle fill="#fff" stroke="none" cx="6" cy="50" r="6">
                        <animate
                            attributeName="opacity"
                            dur="1s"
                            values="0;1;0"
                            repeatCount="indefinite"
                            begin="0.1"
                        />
                    </circle>
                    <circle fill="#fff" stroke="none" cx="26" cy="50" r="6">
                        <animate
                            attributeName="opacity"
                            dur="1s"
                            values="0;1;0"
                            repeatCount="indefinite"
                            begin="0.2"
                        />
                    </circle>
                    <circle fill="#fff" stroke="none" cx="46" cy="50" r="6">
                        <animate
                            attributeName="opacity"
                            dur="1s"
                            values="0;1;0"
                            repeatCount="indefinite"
                            begin="0.3"
                        />
                    </circle>
                </svg>
            </div>
        {:else}
            <div class="flex flex-col gap-3">
                <div
                    on:click={() => {
                        fileinput.click();
                    }}
                    class="px-5 py-16 flex flex-col justify-center items-center bg-gray-100 text-gray-300 border-dashed border-2 border-gray-300 rounded-lg active:scale-95 active:bg-gray-200/25 transition cursor-pointer"
                >
                    <input
                        class="hidden"
                        type="file"
                        accept=".jpg, .jpeg, .png"
                        on:change={onFileSelected}
                        bind:this={fileinput}
                    />
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="60"
                        height="60"
                        fill="currentColor"
                        class="bi bi-cloud-arrow-up-fill"
                        viewBox="0 0 16 16"
                    >
                        <path
                            d="M8 2a5.53 5.53 0 0 0-3.594 1.342c-.766.66-1.321 1.52-1.464 2.383C1.266 6.095 0 7.555 0 9.318 0 11.366 1.708 13 3.781 13h8.906C14.502 13 16 11.57 16 9.773c0-1.636-1.242-2.969-2.834-3.194C12.923 3.999 10.69 2 8 2zm2.354 5.146a.5.5 0 0 1-.708.708L8.5 6.707V10.5a.5.5 0 0 1-1 0V6.707L6.354 7.854a.5.5 0 1 1-.708-.708l2-2a.5.5 0 0 1 .708 0l2 2z"
                        />
                    </svg>
                    <h1 class="text-3xl">Upload your image</h1>
                </div>
                <button
                    on:click={analyze}
                    class="hidden border-2 border-gray-300 bg-primary text-gray-500 px-2 py-3 text-xl rounded flex justify-center items-center gap-2 active:scale-95 transition"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="20"
                        height="20"
                        fill="currentColor"
                        class="bi bi-search-heart-fill"
                        viewBox="0 0 16 16"
                    >
                        <path
                            d="M6.5 13a6.474 6.474 0 0 0 3.845-1.258h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.008 1.008 0 0 0-.115-.1A6.471 6.471 0 0 0 13 6.5 6.502 6.502 0 0 0 6.5 0a6.5 6.5 0 1 0 0 13Zm0-8.518c1.664-1.673 5.825 1.254 0 5.018-5.825-3.764-1.664-6.69 0-5.018Z"
                        />
                    </svg>
                    Analyze
                </button>
            </div>
        {/if}
    </div>
</div>
{#if on_analyze}
    <div
        transition:fly={{ y: -50, duration: 500 }}
        on:click={()=>{}}
        class="top-0 right-0 absolute min-h-screen w-full flex items-center backdrop-blur-sm bg-white/30"
    >
        <div class="container flex items-center justify-center py-10">
            <div
                class="relative bg-gray-100 p-3 rounded-lg w-full max-w-xl shadow-sm"
            >
                <button
                    on:click={() => ((on_analyze = false), (imgpreview = ""))}
                    class="flex items-center justify-center text-gray-500 absolute top-0 right-0 h-[40px] w-[40px] -translate-y-[calc(35%+5px)] translate-x-[calc(35%+5px)] bg-primary rounded active:scale-95"
                >
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="23"
                        height="23"
                        fill="currentColor"
                        class="bi bi-x-lg"
                        viewBox="0 0 16 16"
                    >
                        <path
                            d="M2.146 2.854a.5.5 0 1 1 .708-.708L8 7.293l5.146-5.147a.5.5 0 0 1 .708.708L8.707 8l5.147 5.146a.5.5 0 0 1-.708.708L8 8.707l-5.146 5.147a.5.5 0 0 1-.708-.708L7.293 8 2.146 2.854Z"
                        />
                    </svg>
                </button>
                <div class="flex flex-col gap-3">
                    <div
                        class="h-[250px] w-full bg-slate-200/40 rounded bg-cover bg-center"
                        style={"background-image: url(" + imgpreview + ");"}
                    />
                    <div>
                        <div class="flex gap-2 item-center">
                            <p class="text-gray-700 font-bold">
                                ระดับความรุนแรง
                            </p>
                            <div class="flex gap-1 items-center">
                                {#each Array(star) as _, i}
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        width="16"
                                        height="16"
                                        fill="currentColor"
                                        class="bi bi-star-fill text-yellow-500"
                                        viewBox="0 0 16 16"
                                    >
                                        <path
                                            d="M3.612 15.443c-.386.198-.824-.149-.746-.592l.83-4.73L.173 6.765c-.329-.314-.158-.888.283-.95l4.898-.696L7.538.792c.197-.39.73-.39.927 0l2.184 4.327 4.898.696c.441.062.612.636.282.95l-3.522 3.356.83 4.73c.078.443-.36.79-.746.592L8 13.187l-4.389 2.256z"
                                        />
                                    </svg>
                                {/each}
                                {#each Array(3 - star) as _, i}
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        width="16"
                                        height="16"
                                        fill="currentColor"
                                        class="bi bi-star"
                                        viewBox="0 0 16 16"
                                    >
                                        <path
                                            d="M2.866 14.85c-.078.444.36.791.746.593l4.39-2.256 4.389 2.256c.386.198.824-.149.746-.592l-.83-4.73 3.522-3.356c.33-.314.16-.888-.282-.95l-4.898-.696L8.465.792a.513.513 0 0 0-.927 0L5.354 5.12l-4.898.696c-.441.062-.612.636-.283.95l3.523 3.356-.83 4.73zm4.905-2.767-3.686 1.894.694-3.957a.565.565 0 0 0-.163-.505L1.71 6.745l4.052-.576a.525.525 0 0 0 .393-.288L8 2.223l1.847 3.658a.525.525 0 0 0 .393.288l4.052.575-2.906 2.77a.565.565 0 0 0-.163.506l.694 3.957-3.686-1.894a.503.503 0 0 0-.461 0z"
                                        />
                                    </svg>
                                {/each}
                            </div>
                        </div>
                        <p class="text-gray-500">
                            <span class="text-gray-700 font-bold">คำแนะนำ</span>
                            Lorem ipsum dolor sit amet consectetur adipisicing elit.
                            Harum voluptatum, ducimus neque quidem quo explicabo
                            qui modi exercitationem alias vero, animi in, debitis
                            sint autem!
                        </p>
                        <p class="text-gray-500">
                            <span class="text-gray-700 font-bold">สาร</span> Lorem
                            ipsum dolor sit amet.
                        </p>
                        <hr class="my-5" />
                        <div>
                            <p class="text-gray-700 font-bold">
                                ผลิตภัณฑ์ที่เราแนะนำ
                            </p>
                            <div class="grid grid-cols-4 gap-1">
                                {#each Array(4) as _, i}
                                    <div
                                        class="h-[150px] bg-gray-200/50 rounded-lg"
                                    />
                                {/each}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
{/if}
