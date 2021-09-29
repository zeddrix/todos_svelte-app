<script>
   import { action } from "@nativescript/core";
   import { Template } from "svelte-native/components";

   import { dones, todos } from "~/store";
   import addToList from "~/actions/addToList";
   import removeFromList from "~/actions/removeFromList";

   let textFieldValue = '';

   const onAddButtonTap = () => {
		 if (textFieldValue === '') return;
		 
		 console.log(`New task added: ${textFieldValue}.`)
		 
		 todos.set([{name: textFieldValue}, ...$todos])
		 
		 textFieldValue = ''
	};

   const onTaskItemTap = async (args) => {
	  let result = await action("What do you want to do with this task?", 
		  "Cancel", 
		  [
			  "Mark completed", 
			  'Delete forever'
		  ]
	  );

	  console.log({ result });

	  let item = $todos[args.index]

	  switch (result) {
		  case "Mark completed":
			  dones.set(addToList($dones, item));
			  todos.set(removeFromList($todos, item))
			  break;
		  case "Delete forever":
			  todos.set(removeFromList($todos, item))
			  break;
		  case "Cancel" || undefined:
			  break;

		  default:
			  break;
	  }
  };
</script>

<tabContentItem>
   <gridLayout columns="*,120" rows=70,*>
      <textField col='0' row='0' bind:text={textFieldValue} hint='Type new task...' editable='true' on:returnPress={onAddButtonTap}/>
      <button col='1' row='0' text='Add task' on:tap={onAddButtonTap} class="-primary" />

      <listView items={$todos} on:itemTap={onTaskItemTap} row='1' colSpan='2' class="todo-item">
         <Template let:item>
            <label text={item.name} textWrap='true'/>
         </Template>
      </listView>
   </gridLayout>
</tabContentItem>

<style>
	textField {
		font-size: 20;
	}
	button {
		font-size: 20;
	}
</style>
