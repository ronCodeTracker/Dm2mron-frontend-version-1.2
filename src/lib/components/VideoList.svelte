



<script>

  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  export let videos = [];

  let uploadFile = null;

  let selectedVideo = null;
  let loadingVideo = false;




function viewVideo(key) {
  console.log('viewVideo');
  console.log("key: " + key);
  //key = "DaronredoSM2.mp4"
  loadingVideo = true;
  // Use the direct download URL as the video source
  selectedVideo = `https://allenskywolf.com/api/download/${encodeURIComponent(key)}`;
  setTimeout(() => {
    loadingVideo = false;
  }, 800); // Simulate loading
}



function handleDelete(key) {
    dispatch('delete', { key });
  }


function handleDownload(key) {
    dispatch('download', { key });
  }





  
  // Assumes video.key is the unique identifier and video.name is the display name
  
  /*
  function viewVideo(key) {
    console.log('viewVideo');
    loadingVideo = true;
    // Replace with your actual video streaming/playback URL
    selectedVideo = `https://allenskywolf.com/api/videos/stream/${key}`;
    setTimeout(() => {
      loadingVideo = false;
    }, 800); // Simulate loading
  }

*/

/*
  function downloadVideo(key) {
    // Replace with your actual download URL
    window.open(`https://allenskywolf.com/api/videos/${key}/download`, '_blank');
  }

*/


</script>

<ul class="space-y-2 sm:space-y-3 mb-6 sm:mb-8">
  {#each videos as video}
    <li class="flex flex-col sm:flex-row items-start sm:items-center justify-between bg-blue-100 rounded p-2 sm:p-3 shadow-sm gap-2">
      <div class="w-full sm:w-auto">
        {#if video.name}
          <span class="font-semibold text-blue-900 text-base sm:text-lg">{video.name}</span>
        {:else}
          <span class="font-semibold text-blue-900 text-base sm:text-lg">{video.key}</span>
        {/if}
        {#if video.category}
          <span class="ml-1 sm:ml-2 text-blue-600 text-xs sm:text-sm">({video.category})</span>
        {/if}
      </div>
      <div class="flex flex-col sm:flex-row gap-2 w-full sm:w-auto">
        <button
          on:click={() => viewVideo(video.key)}
          class="bg-blue-500 hover:bg-blue-600 text-white px-3 py-2 rounded transition text-sm w-full sm:w-auto"
        >
          View
        </button>
        <button
         on:click={() => handleDownload(video.key)}
          class="bg-green-500 hover:bg-green-600 text-white px-3 py-2 rounded transition text-sm w-full sm:w-auto"
        >
          Download
        </button>
        <button
          on:click={() => handleDelete(video.key)}
          class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 rounded transition text-sm w-full sm:w-auto"
        >
          Delete
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