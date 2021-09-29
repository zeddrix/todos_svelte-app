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
            <label text='List of completed tasks for tracking'/>
        </tabContentItem>
    </tabs>

</page>

<script>
    import { Template  } from "svelte-native/components";

    let todos = [];
    let textFieldValue = ''

    const onItemTap = (args) => console.log(`Item ${todos[args.index].name} at index: ${args.index} was tapped.`)

    const onButtonTap = () => {
        if (textFieldValue === '') return;

        console.log(`New task added: ${textFieldValue}.`)

        todos= [{name: textFieldValue}, ...todos]

        textFieldValue =''
    }
</script>