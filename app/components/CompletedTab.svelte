<script>
	import { action } from "@nativescript/core";
	import { Template } from "svelte-native/components";
	
   import { dones, todos } from "~/store";
   import addToList from "~/actions/addToList";
   import removeFromList from "~/actions/removeFromList";

	const onDoneTap = async (args) => {
	  let result = await action("What do you want to do with this task?", 
		  "Cancel", 
		  [
			  "Mark To Do", 
			  'Delete forever'
		  ]
	  );

	  console.log({ result });

	  let item = $dones[args.index];

	  switch (result) {
		  case 'Mark To Do':
			  todos.set(addToList($todos, item));
			  dones.set(removeFromList($dones, item));
			  break;
		  case 'Delete forever':
		  	  dones.set(removeFromList($dones, item));
			  break;
	  
		  case "Cancel" || undefined:
			  break;

		  default:
			  break;
	  }
	}
</script>

<tabContentItem>
   <listView items={$dones} on:itemTap={onDoneTap} class="todo-item completed" >
      <Template let:item>
         <label text={item.name} textWrap='true' />
      </Template>
   </listView>
</tabContentItem>

<style>
	.completed {
		color: #939393;
		text-decoration: line-through;
	}
</style>