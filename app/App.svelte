<script>
   import TodoTab from "./components/TodoTab.svelte";
	import CompletedTab from "./components/CompletedTab.svelte";
</script>

<page>
   <actionBar title="My Tasks"/>

	<tabs tabsPosition='bottom'>
		<tabStrip>
			<tabStripItem title='To Do'/>
			<tabStripItem title='Completed'/>
		</tabStrip>

		<TodoTab />

		<CompletedTab />
	</tabs>

</page>
