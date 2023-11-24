<script lang="ts">
    import { ConverterMethod } from "$lib/scripts/global";
    import { convert as latinJavaConvert } from "$lib/scripts/latinlontara";
    import { convert as javaLatinConvert } from "$lib/scripts/javalatin";
    import { convert as javaKawiConvert } from "$lib/scripts/javakawi";
    import { convert as latinLontaraConvert } from "$lib/scripts/latinlontara";
    import { javaDefaultKeyboard, javaCapslockKeyboard } from "$lib/scripts/javakeyboard";
    
    let input:string = "";
    let output:string = "";
    
    let textareaEl:HTMLTextAreaElement;
    let isPepetTypeMode:boolean = true;
    let isIgnoreSpace:boolean = true;
    let isMurda:boolean = false;
    let isDiphtong:boolean = false;
    let isVirtualKeyboardActive:boolean = false;
    let isCapslock:boolean = false;
    
    const latinToJavaSpecialCharacters = ['Ê', 'ê', 'ā', 'ī', 'ū', 'ḍ', 'ḍh', 'ṣ', 'ś', 'ṭ', 'ṭh', 'ṇ', 'ñ', 'ŋ'];
    let javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
    var javaKeyboardDictionary:{ [id: string]: string; } = { };
    javaDefaultKeyboard.forEach(x => {
        javaKeyboardDictionary[x[0]] = x[1];
    });
    javaCapslockKeyboard.forEach(x => {
        javaKeyboardDictionary[x[0]] = x[1];
    });
    
    
    let tooltipEl:HTMLElement;
    let tooltipMessage:string = "";
    
    export let method:ConverterMethod = ConverterMethod.None;
    
    function inputTitle():string
    {
        switch(method) { 
            case ConverterMethod.LatinToLontara:
                return "Latin Script"
            case ConverterMethod.LatinToLontara:
                return "Latin Script"
            case ConverterMethod.JavaToKawi: 
            case ConverterMethod.JavaToLatin: 
                return "Lontara Script"
        } 
    
        return "Undefined Script";
    }
    
    function outputTitle():string
    {
        switch(method) { 
            case ConverterMethod.LatinToLontara: 
                return "Lontara Script";
            case ConverterMethod.JavaToLatin: 
                return "Latin Script";
            case ConverterMethod.JavaToKawi:
                return "Kawi Script";
            case ConverterMethod.LatinToLontara:
                return "Lontara Script"
        }
    
        return "Undefined Script";
    }
    
    function onInputUpdate()
    {
        switch(method) { 
            case ConverterMethod.LatinToLontara: 
            { 
                output = latinJavaConvert(input, isIgnoreSpace, isMurda, isDiphtong);
                break; 
            }
            case ConverterMethod.JavaToLatin: 
            {
                output = javaLatinConvert(input) 
                break; 
            }
            case ConverterMethod.JavaToKawi: 
            {
                output = javaKawiConvert(input) 
                break; 
            }
        } 
    }
    
    function onInputKeyDown(e:KeyboardEvent) {
        if (e.ctrlKey) return;
    
        if (method == ConverterMethod.LatinToLontara) 
        {
            if(isPepetTypeMode == false) return;
    
            if(e.key === "X") {
                insertToTextarea("Ê");
                onInputUpdate();
                e.preventDefault();
                e.stopPropagation();
                return;
            }
    
            if(e.key === "x") {
                insertToTextarea("ê");
                onInputUpdate();
                e.preventDefault();
                e.stopPropagation();
            }
    
            return;
        }
    
        if (method == ConverterMethod.JavaToLatin || method == ConverterMethod.JavaToKawi) 
        {
            if(isVirtualKeyboardActive == false) return;
    
            isCapslock = e.getModifierState("CapsLock");
            javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
    
            if (javaKeyboardDictionary.hasOwnProperty(e.key))
            {
                insertToTextarea(javaKeyboardDictionary[e.key]);
                onInputUpdate();
                e.preventDefault();
                e.stopPropagation();
            }
            
            return;
        }
    }
    
    function onDocumentKeyDown(e:KeyboardEvent) {
        if (e.key == "CapsLock")
        {
            isCapslock = e.getModifierState("CapsLock");
            javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
        }
    
        if (e.key == "Shift")
        {
            isCapslock = e.getModifierState("CapsLock") ? !e.getModifierState("Shift") : e.getModifierState("Shift");
            javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
        }
    }
    
    function onDocumentKeyUp(e:KeyboardEvent) {
        if (e.key == "Shift")
        {
            isCapslock = e.getModifierState("CapsLock") ? !e.getModifierState("Shift") : e.getModifierState("Shift");
            javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
        }
    }
    
    function insertToTextarea(str:string) {
        var nextCursorPos = textareaEl.selectionStart + str.length;
        textareaEl.value = textareaEl.value.substring(0, textareaEl.selectionStart) + str + textareaEl.value.substring(textareaEl.selectionEnd, textareaEl.value.length);
        input = textareaEl.value;
        textareaEl.focus();
        textareaEl.setSelectionRange(nextCursorPos, nextCursorPos);
    }
    
    function onPressBackspace() {
        if (textareaEl.selectionStart == 0 && textareaEl.selectionEnd == 0) 
        {
            textareaEl.focus();
            textareaEl.setSelectionRange(textareaEl.selectionStart, textareaEl.selectionEnd);
            return;
        }
    
        var selectionLength = textareaEl.selectionStart - textareaEl.selectionEnd;
        var nextCursorPos = selectionLength == 0 ? textareaEl.selectionStart - 1 : textareaEl.selectionStart;
        textareaEl.value = textareaEl.value.substring(0, nextCursorPos) + textareaEl.value.substring(textareaEl.selectionEnd, textareaEl.value.length);
        input = textareaEl.value;
    
        textareaEl.focus();
        textareaEl.setSelectionRange(nextCursorPos, nextCursorPos);
    }
    
    function toggleCapslock()
    {
        isCapslock = !isCapslock;
        javaKeyboard = isCapslock ? javaCapslockKeyboard : javaDefaultKeyboard;
        textareaEl.focus();
        textareaEl.setSelectionRange(textareaEl.selectionStart, textareaEl.selectionEnd);
    }
    
    function copyToClipboard(text:string) {
        navigator.clipboard.writeText(text)
            .then(res => {
                tooltipMessage = "Copied";
                tooltipEl.classList.add("show");
            }).catch(error => {
                tooltipMessage = "Not copied";
                tooltipEl.classList.add("show");
            });
    }
    
    function onClickCopyButton()
    {
        copyToClipboard(output);
    }
    
    function onPointerLeaveCopyButton()
    {
        tooltipEl.classList.remove("show");
    }
    </script>
    
    <svelte:head>
        <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" />
        <link rel="preconnect" href="https://fonts.googleapis.com">
        <link rel="preconnect" href="https://fonts.gstatic.com">
        <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Buginese&display=swap" rel="stylesheet">
    </svelte:head>
    
    <svelte:document on:keydown={ onDocumentKeyDown } on:keyup={ onDocumentKeyUp } />
    
    <section class="converter">
        <div class="row">
            <div class="col-12 converter-input">
                <h4>{ inputTitle() }</h4>
                <textarea bind:this={ textareaEl } bind:value={input} on:input={ onInputUpdate } on:keydown={ onInputKeyDown }></textarea>
        
                {#if method == ConverterMethod.LatinToLontara }
                <div style="margin-block-start: 1em; display: none;">
                    <label style="margin-right: .5em;">
                        <input type="checkbox" role="switch" bind:checked={ isPepetTypeMode }>
                        Mode Ketik Pepet
                    </label>
        
                    <label style="margin-right: .5em;">
                        <input type="checkbox" role="switch" bind:checked={ isIgnoreSpace } on:change={ onInputUpdate }>
                        Abaikan Spasi
                    </label>
        
                    <label style="margin-right: .5em;">
                        <input type="checkbox" role="switch" bind:checked={ isMurda } on:change={ onInputUpdate }>
                        Murda
                    </label>
        
                    <label style="margin-right: .5em;">
                        <input type="checkbox" role="switch" bind:checked={ isDiphtong } on:change={ onInputUpdate }>
                        Diftong
                    </label>
                </div>
                
                <!-- <h5>Tombol Karakter Spesial</h5> -->
                <div class="virtual-keyboard">
                    {#each latinToJavaSpecialCharacters as char}
                    <button class="button outline icon-only" on:click= { () => { insertToTextarea(char); onInputUpdate(); } }>{char}</button>
                    {/each}
                </div>
                {/if}
        
                {#if method == ConverterMethod.JavaToLatin || method == ConverterMethod.JavaToKawi }
                <div style="margin-block-start: 1em;">
                    <label style="margin-right: .5em;">
                        <input type="checkbox" role="switch" bind:checked={ isVirtualKeyboardActive }>
                        Aktifkan Keyboard Virtual Aksara Jawa
                    </label>
                </div>
                    {#if isVirtualKeyboardActive }
                    <div class="virtual-keyboard layout">
                        {#each javaKeyboard as key, i}<button class="button outline icon-only" on:click= { () => { insertToTextarea(key[1]); onInputUpdate(); } }>{key[1]}</button>{#if i == 12 || i == 25}<br/>{/if}{#if i == 36}<button class="button outline icon-only material-symbols-outlined" style="width: 12%;font-size: 1.25em;" on:click= { () => { insertToTextarea('\n'); onInputUpdate(); } }>keyboard_return</button><br/><button class="button activatable outline icon-only material-symbols-outlined" class:active={ isCapslock } style="width: 12%;font-size: 1.25em;" on:dblclick={ toggleCapslock } on:click={ toggleCapslock }>keyboard_capslock</button>{/if}{/each}<button class="button outline icon-only material-symbols-outlined" style="width: 12%;font-size: 1.25em;" on:click={ () => { onPressBackspace(); onInputUpdate(); } }>backspace</button><br/><button class="button outline icon-only material-symbols-outlined" style="width: 50%;font-size: 1.25em;" on:click= { () => { insertToTextarea(' '); onInputUpdate(); } }>space_bar</button>
                    </div>
                    {/if}
                {/if}
            </div>
            <div class="col-12 converter-output">
                <h4>{ outputTitle() }</h4>
                    <div class="content">
                    <span>{ output }</span>
                    <div class="action-button-area">
                        <button bind:this={ tooltipEl } class="button copy icon-only clear tooltip" data-text={ tooltipMessage } on:click={ onClickCopyButton } on:pointerleave={ onPointerLeaveCopyButton }>
                            <span class="material-symbols-outlined">content_copy</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    {#if method == ConverterMethod.LatinToLontara }
    <section class="row" style="display: none;">
        <div class="col">
            <h4>Reserved for Notes and Instructions</h4>
        </div>
    </section>
    {/if}