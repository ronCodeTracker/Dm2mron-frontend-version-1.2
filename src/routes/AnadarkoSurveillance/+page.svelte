


<script>
  import { onMount } from 'svelte';
  import VideoList from '$lib/components/VideoList.svelte';

  let videos = [];
  let selectedOption = '';
  let videoKeyToDelete = '';
  let fileToUpload = null;



  
  onMount(async () => {
    const response = await fetch('https://allenskywolf.com/api/videos');
    videos = await response.json();
  });


// Use dummy data for testing
/*
  let videos = [
    { key: 'test1', name: 'Test Video 1', category: 'Demo' },
    { key: 'test2', name: 'Test Video 2', category: 'Demo' }
  ];

*/

  const downloadVideos = async () => {
    let url = 'https://allenskywolf.com/api/download?';
    if (selectedOption === 'firstFive') {
      url += 'count=5';
    } else if (selectedOption === 'lastFive') {
      url += 'count=-5';
    } else {
      url += 'count=all';
    }
    window.open(url, '_blank');
  };

  const deleteVideo = async () => {
    if (videoKeyToDelete) {
      await fetch(`https://allenskywolf.com/api/videos/${videoKeyToDelete}`, {
        method: 'DELETE',
      });
      videos = videos.filter(video => video.key !== videoKeyToDelete);
      videoKeyToDelete = '';
    }
  };

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
</script>

<div class="min-h-screen bg-gray-100 p-6">
  <nav class="bg-white shadow mb-6 p-4">
    <h1 class="text-2xl font-bold text-gray-800">Anadarko Surveillance</h1>
  </nav>

  <div class="bg-white rounded-lg shadow p-6">
    <h2 class="text-xl font-semibold mb-4">Manage Videos</h2>

    <div class="mb-4">
      <label for="videoSelect" class="block text-gray-700">Download Videos:</label>
      <select id="videoSelect" bind:value={selectedOption} class="mt-1 block w-full border-gray-300 rounded-md shadow-sm">
        <option value="firstFive">First Five Videos</option>
        <option value="lastFive">Last Five Videos</option>
        <option value="all">All Videos</option>
      </select>
    </div>

    <button on:click={downloadVideos} class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Download</button>

    <div class="mt-6 mb-4">
      <label for="videoKey" class="block text-gray-700">Delete Video by Key:</label>
      <input id="videoKey" type="text" bind:value={videoKeyToDelete} class="mt-1 block w-full border-gray-300 rounded-md shadow-sm" placeholder="Enter video key" />
      <button on:click={deleteVideo} class="mt-2 bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600">Delete</button>
    </div>

    <div class="mt-6 mb-4">
      <label for="videoUpload" class="block text-gray-700">Upload MP4 Video:</label>
      <input id="videoUpload" type="file" accept="video/mp4" on:change={e => fileToUpload = e.target.files[0]} class="mt-1 block w-full border-gray-300 rounded-md shadow-sm" />
      <button on:click={uploadVideo} class="mt-2 bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Upload</button>
    </div>

    <VideoList {videos} />
  </div>
</div>





