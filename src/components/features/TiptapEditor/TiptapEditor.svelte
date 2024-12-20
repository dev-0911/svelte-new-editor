<script>
    import cn from "classnames";
    import { onMount as onComponentMount, onDestroy as onComponentDestroy } from "svelte";
    import { Editor } from "@tiptap/core";
    import useExtensions from "./extensions/useExtensions";
    import Toolbar from "./toolbars/Toolbar.svelte";

    export let onCreate;
    export let onDestroy;
    export let onClose;
    export let onApply;
    export let className = "";

    // Initial content for the editor
    const content = `<h1>One morning, when Gregor Samsa woke from troubled 
dreams.</h1>
<p>One morning, when Gregor Samsa woke from troubled 
dreams, he found himself transformed in his bed into 
a horrible vermin. He lay on his armour-like back, 
and if he lifted his head a little he could see his 
brown belly, slightly domed and divided by arches into 
stiff sections. The bedding was hardly able to cover 
<strong>strong</strong> it and seemed ready to slide 
off any moment. His many legs, pitifully thin 
compared with the size of the rest of him, 
<a class="external ext" href="#">link</a> waved about 
helplessly as he looked. "What's happened to me? " he 
thought. It wasn't a dream. His room, a proper human 
room although a little too small, lay peacefully 
between its four familiar walls.</p>
<h1>One morning, when Gregor Samsa woke from troubled 
dreams.</h1>
<h2>The bedding was hardly able to cover it.</h2>
<p>It showed a lady fitted out with a fur hat and fur 
boa who sat upright, raising a heavy fur muff that 
covered the whole of her lower arm towards the 
viewer.</p>
<h2>The bedding was hardly able to cover it.</h2>
<p>It showed a lady fitted out with a fur hat and fur 
boa who sat upright, raising a heavy fur muff that 
covered the whole of her lower arm towards the 
viewer.</p>
<ul>
  <li>Lorem ipsum dolor sit amet consectetuer.</li>
  <li>Aenean commodo ligula eget dolor.</li>
  <li>Aenean massa cum sociis natoque penatibus.</li>
</ul>
<p>One morning, when Gregor Samsa woke from troubled 
dreams, he found himself transformed in his bed into 
a horrible vermin. He lay on his armour-like back, 
and if he lifted his head a little he could see his 
brown belly, slightly domed and divided by arches into 
stiff sections. The bedding was hardly able to cover 
<strong>strong</strong> it and seemed ready to slide 
off any moment. His many legs, pitifully thin 
compared with the size of the rest of him, 
<a class="external ext" href="#">link</a> waved about 
helplessly as he looked. "What's happened to me? " he 
thought. It wasn't a dream. His room, a proper human 
room although a little too small, lay peacefully 
between its four familiar walls.</p>
<p>It showed a lady fitted out with a fur hat and fur 
boa who sat upright, raising a heavy fur muff that 
covered the whole of her lower arm towards the 
viewer.</p>`;

    let element;
    let editor;

    // const words = writable(0);
    // const characters = writable(0);
    const extensions = useExtensions();

    const handleClose = () => {
        if (typeof onClose === "function") {
            onClose();
        }
    };

    const handleApply = () => {
        if (typeof onApply === "function") {
            onApply();
        }
        handleClose();
    };

    const handleCreate = (editorInstance) => {
        if (typeof onCreate === "function") {
            onCreate(editorInstance);
        }
    };

    const handleDestroy = () => {
        if (typeof onDestroy === "function") {
            onDestroy();
        }
    };

    onComponentMount(() => {
        editor = new Editor({
            element,
            extensions,
            content,
            editorProps: {
                attributes: {
                    class: "focus:outline-none w-full",
                },
                spellcheck: "false",
                suppressContentEditableWarning: "true",
            },
            immediatelyRender: false,
            onCreate: handleCreate,
            onDestroy: handleDestroy,
            onTransaction: () => {
                // force re-render so `editor.isActive` works as expected
                editor = editor;
            },
        });
    });

    onComponentDestroy(() => {
        if (editor) {
            editor.destroy();
        }
    });
</script>

<div class="w-screen h-screen flex flex-col overflow-hidden bg-background border border-border-toolbar text-text">
    {#if editor}
        <Toolbar className="flex-0 w-full px-10 bg-background-toolbar border-b border-b-border-toolbar" {editor} />
    {/if}

    <div class={cn(className, "flex-1 px-12 py-5")} bind:this={element}></div>
</div>
