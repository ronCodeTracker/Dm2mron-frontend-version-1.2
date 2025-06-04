



<script>
  export let videos = [];

  let selectedVideo = null;
  let loadingVideo = false;

  // Assumes video.key is the unique identifier and video.name is the display name
  function viewVideo(key) {
    loadingVideo = true;
    // Replace with your actual video streaming/playback URL
    selectedVideo = `https://allenskywolf.com/api/videos/${key}/stream`;
    setTimeout(() => {
      loadingVideo = false;
    }, 800); // Simulate loading
  }



/*
  function downloadVideo(key) {
    // Replace with your actual download URL
    window.open(`https://allenskywolf.com/api/videos/${key}/download`, '_blank');
  }

*/


</script>

<ul class="space-y-3 mb-8">
  {#each videos as video}
    <li class="flex items-center justify-between bg-blue-100 rounded p-3 shadow-sm">
      <div>

         {#if video.name}
          <span class="font-semibold text-blue-900">{video.name}</span>
        {:else}
          <span class="font-semibold text-blue-900">{video.key}</span>
        {/if}




        <!--<span class="font-semibold text-blue-900">{video.name}</span>-->
        {#if video.category}
          <span class="ml-2 text-blue-600 text-sm">({video.category})</span>
        {/if}
      </div>
      <div class="flex gap-2">
        <button
          on:click={() => viewVideo(video.key)}
          class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-1 rounded transition"
        >
          View
        </button>
        <button
          on:click={() => downloadVideo(video.key)}
          class="bg-green-500 hover:bg-green-600 text-white px-4 py-1 rounded transition"
        >
          Download
        </button>
      </div>
    </li>
  {/each}
</ul>

{#if loadingVideo}
  <div class="flex flex-col items-center justify-center py-8">
    <svg class="animate-spin h-8 w-8 text-blue-600 mb-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8z"></path>
    </svg>
    <p class="text-blue-700 font-semibold">Loading video...</p>
  </div>
{/if}

{#if selectedVideo && !loadingVideo}
  <div class="mt-8">
    <h2 class="text-xl font-bold text-blue-800 mb-2">Video Viewer</h2>
    <video
      controls
      autoplay
      playsinline
      muted
      width="600"
      class="rounded-lg border border-blue-300 shadow"
      style="max-width: 100%; height: auto;"
      src={selectedVideo}
    >
      Your browser does not support the video tag.
    </video>
  </div>
{/if}