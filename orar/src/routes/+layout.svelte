<script lang="ts">
	import { ModeWatcher, setMode } from 'mode-watcher';
	import '../app.css';


	setMode("dark")
</script>

<ModeWatcher />
<slot />
