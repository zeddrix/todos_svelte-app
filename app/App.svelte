<page>
    <actionBar title="My Tasks"/>

    <tabs tabsPosition='bottom'>
        <tabStrip>
            <tabStripItem title='To Do'/>
        <tabStripItem title='Completed'/>
        </tabStrip>

        <tabContentItem>
            <gridLayout columns="*,120" rows=70,*>
                <textField col='0' row='0' bind:text={textFieldValue} hint='Type new task...' editable='true' on:returnPress={onButtonTap}/>
                <button col='1' row='0' text='Add task' on:tap={onButtonTap}/>

                <listView items={todos} on:itemTap={onItemTap} row='1' colSpan='2'>
                    <Template let:item>
                        <label text={item.name} textWrap='true'/>
                    </Template>
                </listView>
            </gridLayout>


            
        </tabContentItem>
        <tabContentItem>
            <listView items={dones} on:itemTap={onDoneTap}>
					<Template let:item>
						<label text={item.name} textWrap='true' />
					</Template>
				</listView>
        </tabContentItem>
    </tabs>

</page>

<script>
    import { action } from "@nativescript/core";
    import { Template  } from "svelte-native/components";

    let todos = [];
    
    let dones = [];

    const removeFromList = (list, item) => list.filter(t => t !== item)
    const addToList = (list, item) => [item, ...list]
    
    let textFieldValue = ''

    
	const onItemTap = async (args) => {
		let result = await action("What do you want to do with this task?", 
			"Cancel", 
			[
				"Mark completed", 
				'Delete forever'
			]
		);


		console.log(result);

		let item = todos[args.index]

		switch (result) {
			case "Mark completed":
				dones = addToList(dones, item)
				todos = removeFromList(todos, item)
				break;
			case "Delete forever":
				todos = removeFromList(todos, item)
				break;
			case "Cancel" || undefined:
				break;

			default:
				break;
		}
	};
    
    const onButtonTap = () => {
        if (textFieldValue === '') return;
        
        console.log(`New task added: ${textFieldValue}.`)
        
        todos = [{name: textFieldValue}, ...todos]
        
        textFieldValue = ''
    }

	 const onDoneTap = async (args) => {
		let result = await action("What do you want to do with this task?", 
			"Cancel", 
			[
				"Mark To Do", 
				'Delete forever'
			]
		);

		console.log({ result });

		let item = dones[args.index];

		switch (result) {
			case 'Mark To Do':
				todos = addToList(todos, item);
				dones = removeFromList(dones, item);
				break;
			case 'Delete forever':
				dones = removeFromList(dones, item);
				break;
		
			case "Cancel" || undefined:
				break;

			default:
				break;
		}
	 }
</script>