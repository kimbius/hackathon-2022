<script>
    let on_analyze = false;
    let fileinput;
    let imgpreview = "";
    let max_star = 3;
    let star = 2;
    let is_fetching = false;

    import { fly } from "svelte/transition";
    import AnalyzeIcon from "../components/Icons/AnalyzeIcon.svelte";
    import CloudUploadIcon from "../components/Icons/CloudUploadIcon.svelte";
    import LoadingElement from "../components/Icons/LoadingElement.svelte";
    import StarFilled from "../components/Icons/StarFilled.svelte";
    import StarOutlined from "../components/Icons/StarOutlined.svelte";
    import XIcon from "../components/Icons/XIcon.svelte";

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
                <LoadingElement />
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
                    <CloudUploadIcon />
                    <h1 class="text-3xl">Upload your image</h1>
                </div>
                <button
                    on:click={analyze}
                    class="hidden border-2 border-gray-300 bg-primary text-gray-500 px-2 py-3 text-xl rounded flex justify-center items-center gap-2 active:scale-95 transition"
                >
                    <AnalyzeIcon />
                    Analyze
                </button>
            </div>
        {/if}
    </div>
</div>
{#if on_analyze}
    <div
        transition:fly={{ y: -50, duration: 500 }}
        on:click={() => {}}
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
                    <XIcon />
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
                                    <StarFilled />
                                {/each}
                                {#each Array(max_star - star) as _, i}
                                    <StarOutlined />
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
