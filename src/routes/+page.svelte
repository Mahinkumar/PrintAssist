<script>
  import { invoke } from "@tauri-apps/api/core"; 
  import { platform } from '@tauri-apps/plugin-os';

  import { message } from '@tauri-apps/plugin-dialog';

  

  let name = $state("");
  let greetMsg = $state("");
  const currentPlatform = platform();
  async function greet() {
    // Learn more about Tauri commands at https://tauri.app/develop/calling-rust/
    greetMsg = await invoke("greet", { name });
  }
  
  async function native_dialog(){
    await message('Hello This is a native Dialog', { title: 'PrintAssist Information', kind: 'warning' });
  }
</script>

<main class="container flex flex-col justify-center items-center h-screen w-full bg-gray-100 space-y-12">
  <h1 class="text-3xl">Welcome to Print Assist</h1>
  <form class="flex justify-evenly space-x-4" onsubmit={greet}>
    <input id="greet-input" placeholder="Enter a name..." bind:value={name} />
    <button type="submit" class="bg-gray-300 p-4">Greet</button>
    <button onclick={native_dialog} class="bg-gray-300 p-4">Dialog</button>
  </form>
  <p class="text-xl">Your Platform: {currentPlatform}</p>
  <p class="text-xl">{greetMsg}</p>
</main>

