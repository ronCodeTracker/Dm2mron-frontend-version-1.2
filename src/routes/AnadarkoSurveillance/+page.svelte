


<script>
  import { onMount } from 'svelte';
  import VideoList from '$lib/components/VideoList.svelte';

  let videos = [];
  let selectedOption = '5';
  let videoKeyToDelete = '';
  let fileToUpload = null;

  let navOpen = false;

  let uploading = false;


  onMount(async () => {
    const response = await fetch('https://allenskywolf.com/api/videos', {
    mode: 'cors'

    });
    videos = await response.json();
  });



 // Compute the filtered videos based on selection
  $: filteredVideos = selectedOption === 'all'
    ? videos
    : videos.slice(0, Number(selectedOption));



// Use dummy data for testing
/*
  let videos = [
    { key: 'test1', name: 'Test Video 1', category: 'Demo' },
    { key: 'test2', name: 'Test Video 2', category: 'Demo' }
  ];

*/


const uploadVideo = () => {};






  async function deleteVideoByKey(key) {
  await fetch(`https://allenskywolf.com/api/videos/delete/${encodeURIComponent(key)}`, {
    method: 'DELETE'
  });
  videos = videos.filter(video => video.key !== key);
}




async function downloadVideoByKey(key) {
    const response = await fetch(`https://allenskywolf.com/api/download/${encodeURIComponent(key)}`);
    if (response.ok) {
      const blob = await response.blob();
      const url = window.URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
a.download = key; // or use a better filename if available
      document.body.appendChild(a);
      a.click();
      a.remove();
      window.URL.revokeObjectURL(url);
    } else {
      alert('Download failed.');
    }
  }


  /*
  const uploadVideo = async () => {
    if (fileToUpload) {
      const formData = new FormData();
      formData.append('file', fileToUpload);
      await fetch('https://allenskywolf.com/api/upload', {
        method: 'POST',
        body: formData,
      });
      fileToUpload = null;
    }
  };
*/
async function uploadVideoByFile(file) {
  console.log("upload started");
    if (file) {
      uploading = true;
      const formData = new FormData();
      formData.append('file', file);
      const response = await fetch('https://allenskywolf.com/api/upload', {
        method: 'POST',
        body: formData,
      });
 uploading =  false;
  if (response.ok) {
          // Optionally refresh the video list here
          alert('Upload successful!');
        } else {
          alert('Upload failed.');
        }
      }
  }

</script>

<div class="min-h-screen bg-gray-100 p-6">
  <nav class="bg-white shadow mb-6 p-4">
    <h1 class="text-2xl font-bold text-gray-800">Anadarko Surveillance</h1>
  </nav>


  <!-- Hamburger and nav at top right -->
    <div class="absolute top-8 right-8 flex flex-col items-end z-20">
        <button id="hamburger" class="bg-black/60 hover:bg-black/80 rounded-full p-4 shadow-lg text-white focus:outline-none transition-all duration-200"
          on:click={() => navOpen = !navOpen}>
            <svg class="w-12 h-12" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
            </svg>
        </button>
        {#if navOpen}
        <nav id="nav-menu" class="flex-col items-end mt-4 bg-black/70 rounded-lg p-4 shadow-lg">
            <a href="/" class="nav-link text-white py-2 px-4 rounded hover:bg-amber-400/80 hover:text-black transition">Home</a>
            <a href="/SearchandRescue" class="nav-link text-white py-2 px-4 rounded hover:bg-amber-400/80 hover:text-black transition">Search and Rescue</a>
            <a href="/AnadarkoSurveillance" class="nav-link text-white py-2 px-4 rounded hover:bg-amber-400/80 hover:text-black transition">Anadarko Surveillance</a>
            <a href="/" class="nav-link text-white py-2 px-4 rounded hover:bg-amber-400/80 hover:text-black transition">Link</a>
            <a href="/" class="nav-link text-white py-2 px-4 rounded hover:bg-amber-400/80 hover:text-black transition">Link 4</a>
            
        </nav>
        {/if}
    </div>



  <div class="bg-white rounded-lg shadow p-6">
    <h2 class="text-xl font-semibold mb-4">Manage Videos</h2>

   <!-- Add this just above the dropdown in your markup -->
   <div class="w-full h-2 rounded bg-blue-400 mb-4"></div>


     <div class="mb-4">
      <label for="videoSelect" class="block text-gray-700">Show Videos:</label>
      <select id="videoSelect" bind:value={selectedOption} class="mt-1 block w-full border-gray-300 rounded-md shadow-sm">
        <option value="5">First 5 Videos</option>
        <option value="10">First 10 Videos</option>
        <option value="all">All Videos</option>
      </select>
    </div>

    

    

    <div class="mt-6 mb-4">
      <label for="videoUpload" class="block text-gray-700">Upload MP4 Video:</label>
      <input id="videoUpload" type="file" accept="video/mp4" on:change={e => fileToUpload = e.target.files[0]} class="mt-1 block w-full border-gray-300 rounded-md shadow-sm" />
      <button on:click={() => uploadVideoByFile(fileToUpload)} class="mt-2 bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Upload</button>
    </div>
    {#if uploading}
    <span class="ml-2 text-blue-600 animate-pulse">Uploading...</span>
    {/if}

   <VideoList videos={filteredVideos} 
   on:delete={e => deleteVideoByKey(e.detail.key)}
   on:download={e => downloadVideoByKey(e.detail.key)}
   />
  </div>
</div>





