<script>
  import { invoke } from "@tauri-apps/api/core";
  import { platform } from "@tauri-apps/plugin-os";
  import { message } from "@tauri-apps/plugin-dialog";
  import {
    isPermissionGranted,
    requestPermission,
    sendNotification,
  } from "@tauri-apps/plugin-notification";

  let name = $state("user");
  let greetMsg = $state("");
  const currentPlatform = platform();
  async function greet() {
    // Learn more about Tauri commands at https://tauri.app/develop/calling-rust/
    greetMsg = await invoke("greet", { name });
    await message(greetMsg, {
      title: "From Backend",
      kind: "info",
    });
  }

  async function os_info() {
    await message("Current Platform Information", {
      title: "Current Platform:" + currentPlatform,
      kind: "info",
    });
  }

  async function native_dialog() {
    await message("Hello This is a native Dialog", {
      title: "PrintAssist Information",
      kind: "warning",
    });
  }

  async function notif(){
    let permissionGranted = await isPermissionGranted();
    if (!permissionGranted) {
      const permission = await requestPermission();
      permissionGranted = permission === 'granted';
    }

    if (permissionGranted) {
      sendNotification({ title: 'Tauri', body: 'Tauri is awesome!' });
    }
    
  }

  
</script>

<main class="container h-screen w-full bg-gray-100">
  <nav
    class="lg:h-14 h-10 w-screen items-center flex justify-between bg-gray-400"
  >
    <p class="text-lg w-1/3 text-center">Test Build v0.0.1</p>
    <h1 class="text-xl w-1/3 text-center">PrintAssist</h1>
    <p class="text-lg w-1/3 text-center">{currentPlatform}</p>
  </nav>
  <div
    class="lg:h-56 h-36 w-screen items-center flex justify-evenly bg-gray-300"
  >
    Info Panel
  </div>

  <nav
    class="lg:h-14 h-10 w-screen items-center flex justify-between pl-16 pr-16 bg-gray-400"
  >
    <button class="w-64 lg:h-14 h-12 bg-slate-500" onclick={native_dialog}>Dialog Test</button>
    <button class="w-64 lg:h-14 h-12 bg-slate-500" onclick={greet}>Backend Test</button>
    <button class="w-64 lg:h-14 h-12 bg-slate-500" onclick={os_info}>OS Info Test</button>
    <button class="w-64 lg:h-14 h-12 bg-slate-500" onclick={notif}>Notification Test</button>
  </nav>
</main>
