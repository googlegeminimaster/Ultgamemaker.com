<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate Game Generator</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400..900&family=Roboto:wght@300;400;700&display=swap" rel="stylesheet">

    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Roboto', sans-serif; background: linear-gradient(135deg, #1a1a2e, #16213e, #0f3460); color: #e0e0e0; line-height: 1.6; display: flex; justify-content: center; align-items: flex-start; min-height: 100vh; padding: 30px 15px; overflow-x: hidden; }
        .container { width: 100%; max-width: 800px; display: flex; flex-direction: column; align-items: center; gap: 25px; }
        .card { background-color: rgba(22, 33, 62, 0.8); padding: 30px; border-radius: 15px; box-shadow: 0 8px 25px rgba(0, 0, 0, 0.4); width: 100%; border: 1px solid rgba(128, 128, 255, 0.2); backdrop-filter: blur(5px); transition: transform 0.3s ease, box-shadow 0.3s ease; }
        .card:hover { transform: translateY(-5px); box-shadow: 0 12px 35px rgba(0, 0, 0, 0.5); }
        #main-title { font-family: 'Orbitron', sans-serif; font-size: 2.8rem; font-weight: 700; text-align: center; margin-bottom: 20px; position: relative; letter-spacing: 1px; cursor: default; padding: 10px; }
        .subtitle { display: block; font-size: 1rem; font-weight: 400; color: #a0a0ff; margin-top: 5px; letter-spacing: normal; }
        .rainbow-text { background: linear-gradient(to right, hsl(0, 100%, 70%), hsl(30, 100%, 70%), hsl(60, 100%, 70%), hsl(120, 100%, 70%), hsl(180, 100%, 70%), hsl(240, 100%, 70%), hsl(300, 100%, 70%), hsl(360, 100%, 70%)); -webkit-background-clip: text; background-clip: text; color: transparent; animation: rainbow-cycle 10s linear infinite; background-size: 200% 100%; }
        @keyframes rainbow-cycle { 0% { background-position: 0% 50%; } 100% { background-position: 200% 50%; } }
        .input-section { margin-bottom: 25px; }
        .input-section label { display: block; font-weight: 500; margin-bottom: 10px; color: #c0c0ff; font-size: 1.1rem; }
        textarea#game-prompt { width: 100%; padding: 15px; border-radius: 8px; border: 1px solid rgba(128, 128, 255, 0.3); background-color: rgba(15, 20, 40, 0.9); color: #e0e0e0; font-family: 'Roboto', sans-serif; font-size: 1rem; resize: vertical; min-height: 100px; transition: border-color 0.3s ease, box-shadow 0.3s ease; }
        textarea#game-prompt:focus { outline: none; border-color: #8080ff; box-shadow: 0 0 10px rgba(128, 128, 255, 0.5); }
        .radio-group { display: flex; gap: 20px; flex-wrap: wrap; }
        .radio-group input[type="radio"] { display: none; }
        .radio-label { display: inline-flex; align-items: center; padding: 12px 20px; background-color: rgba(30, 40, 70, 0.7); border: 2px solid rgba(128, 128, 255, 0.3); border-radius: 25px; cursor: pointer; transition: background-color 0.3s ease, border-color 0.3s ease, transform 0.2s ease; color: #c0c0ff; font-size: 1rem; }
        .radio-label .icon { margin-right: 8px; font-size: 1.2rem; transition: transform 0.3s ease; }
        .radio-group input[type="radio"]:checked + .radio-label { background-color: #5050a0; border-color: #a0a0ff; color: #ffffff; font-weight: 500; transform: scale(1.05); }
        .radio-group input[type="radio"]:checked + .radio-label .icon { transform: rotate(10deg); }
        .radio-label:hover { background-color: rgba(50, 60, 100, 0.9); border-color: #a0a0ff; }
        #generate-button { display: flex; align-items: center; justify-content: center; width: 100%; padding: 15px 30px; font-family: 'Orbitron', sans-serif; font-size: 1.3rem; font-weight: 600; color: #ffffff; background: linear-gradient(45deg, #4f4fff, #8e44ad); border: none; border-radius: 10px; cursor: pointer; transition: all 0.3s ease; position: relative; overflow: hidden; text-transform: uppercase; letter-spacing: 1px; margin-top: 10px; }
        .button-icon { margin-right: 10px; font-size: 1.5rem; display: inline-block; transition: transform 0.3s ease; }
        #generate-button:hover { background: linear-gradient(45deg, #6a6aff, #a052cc); box-shadow: 0 5px 15px rgba(142, 68, 173, 0.4); transform: translateY(-3px); }
        #generate-button:hover .button-icon { transform: rotate(360deg) scale(1.2); }
        #generate-button:active { transform: translateY(-1px); box-shadow: 0 2px 8px rgba(142, 68, 173, 0.3); }
        .glow-on-hover { position: relative; z-index: 0; }
        .glow-on-hover:before { content: ''; background: linear-gradient(45deg, #ff0000, #ff7300, #fffb00, #48ff00, #00ffd5, #002bff, #7a00ff, #ff00c8, #ff0000); position: absolute; top: -3px; left:-3px; background-size: 400%; z-index: -1; filter: blur(8px); width: calc(100% + 6px); height: calc(100% + 6px); animation: glowing 20s linear infinite; opacity: 0; transition: opacity .3s ease-in-out; border-radius: 10px; }
        .glow-on-hover:hover:before { opacity: 1; }
        @keyframes glowing { 0% { background-position: 0 0; } 50% { background-position: 400% 0; } 100% { background-position: 0 0; } }
        #generate-button:disabled { background: #555; cursor: not-allowed; opacity: 0.7; }
        #generate-button:disabled:hover { background: #555; box-shadow: none; transform: none; }
        #generate-button:disabled:hover:before { opacity: 0; }

        #loading-indicator { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.7); display: flex; flex-direction: column; align-items: center; justify-content: center; z-index: 2000; text-align: center; padding: 20px; opacity: 0; visibility: hidden; transition: opacity 0.3s ease, visibility 0.3s ease; }
        #loading-indicator.visible { opacity: 1; visibility: visible; }
        .spinner { width: 60px; height: 60px; border: 6px solid rgba(255, 255, 255, 0.3); border-top-color: #a0a0ff; border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 15px; transition: border-top-color 0.3s ease; }
        @keyframes spin { to { transform: rotate(360deg); } }
        #loading-indicator p { font-size: 1.3rem; color: #e0e0e0; transition: color 0.3s ease; }
        #loading-indicator.error-state .spinner { border-top-color: #ff6b6b; animation-play-state: paused; }
        #loading-indicator.error-state p { color: #ffdddd; }

        .hidden { display: none !important; }

        @media (max-width: 768px) { /* ... */ }
        @media (max-width: 480px) { /* ... */ }
    </style>
</head>
<body>
    <div id="loading-indicator">
        <div class="spinner"></div>
        <p id="loading-message">Loading...</p>
    </div>

    <div class="container" id="generator-container">
         <h1 id="main-title" class="rainbow-text">
            Ultimate Game Generator
            <span class="subtitle">3D and 2D - powered by Gemini</span>
        </h1>
        <div class="card">
            <section class="input-section">
                <label for="game-prompt">1: Describe the game you want Gemini to create:</label>
                <textarea id="game-prompt" rows="6" placeholder="Example: A complete HTML file for a 2D platformer game..."></textarea>
            </section>
            <section class="input-section">
                <label>2: What device is this game made for?</label>
                <div class="radio-group">
                    <input type="radio" id="device-phone" name="device" value="Phone" checked>
                    <label for="device-phone" class="radio-label"><span class="icon">📱</span> Phone</label>
                    <input type="radio" id="device-computer" name="device" value="Computer">
                    <label for="device-computer" class="radio-label"><span class="icon">💻</span> Computer</label>
                </div>
            </section>
            <section class="input-section">
                <label>3: Style of the game:</label>
                <div class="radio-group">
                    <input type="radio" id="style-2d" name="game-style" value="2D" checked>
                    <label for="style-2d" class="radio-label"><span class="icon">🖼️</span> 2D</label>
                    <input type="radio" id="style-3d" name="game-style" value="3D">
                    <label for="style-3d" class="radio-label"><span class="icon">🧊</span> 3D</label>
                </div>
            </section>
            <button id="generate-button" class="glow-on-hover">
                <span class="button-icon">✨</span> Generate & Play Game
            </button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const generatorContainer = document.getElementById('generator-container');
            const gamePromptTextarea = document.getElementById('game-prompt');
            const generateButton = document.getElementById('generate-button');
            const loadingIndicator = document.getElementById('loading-indicator');
            const loadingMessage = document.getElementById('loading-message');

            const _cfg = (() => {
                const _k_parts = [
                    String.fromCharCode(65, 73, 122, 97, 83, 121),
                    'ATG6', 'eeAX', '8LBK', String.fromCharCode(116, 117, 81),
                    '04xO', '8QYV', 'Tg4j', String.fromCharCode(116, 101, 105),
                    'Xhc'
                ];
                const _key = _k_parts.join('');

                const _b_parts = [
                    'https:', String.fromCharCode(47, 47), 'generat', 'ivelan', 'guage.', 'google',
                    'apis.com', '/v1beta', '/models/', 'gemini-2', '.0-flash',
                    String.fromCharCode(58) + 'generate', 'Content', '?key='
                ];
                const _base = _b_parts.join('');

                const _endpoint = _base + _key;
                return { _finalUrl: _endpoint };
            })();
            const apiServiceEndpoint = _cfg._finalUrl;
            const absoluteGeneratorUrl = window.location.href.split('?')[0];

            const SESSION_STORAGE_EDIT_PROMPT = 'ugg_editPrompt';
            const SESSION_STORAGE_GAME_SOURCE = 'ugg_gameSourceToEdit';

            let gameObjectUrl = null;
            let errorTimeoutId = null;

            setLoading(false);
            checkForEditAction();

            generateButton.addEventListener('click', handleGenerateClick);

            function checkForEditAction() {
                const editPrompt = sessionStorage.getItem(SESSION_STORAGE_EDIT_PROMPT);
                const originalHtml = sessionStorage.getItem(SESSION_STORAGE_GAME_SOURCE);
                sessionStorage.removeItem(SESSION_STORAGE_EDIT_PROMPT);
                sessionStorage.removeItem(SESSION_STORAGE_GAME_SOURCE);
                if (editPrompt && originalHtml) {
                    console.log("Edit action detected via sessionStorage.");
                    generatorContainer.classList.add('hidden');
                    handleEditRequest(editPrompt, originalHtml);
                } else {
                    generatorContainer.classList.remove('hidden');
                }
            }

            async function handleEditRequest(editPrompt, originalHtml) {
                setLoading(true, "Asking Gemini to modify the game...");
                try {
                    const modificationPrompt = `Hi Gemini! Modify the following HTML based on the request. Preserve core functionality and the in-game edit menu unless asked otherwise.

**User Request:**
${editPrompt}

**Original HTML Code:**
\`\`\`html
${originalHtml}
\`\`\`

**Output Requirements:**
*   Respond with **only** the complete, modified, self-contained HTML (\`<!DOCTYPE html>...\`).
*   The required in-game menu ('...', Leave, Edit, Regenerate) MUST be present.
*   The 'Leave' button's JS MUST navigate to: \`${absoluteGeneratorUrl}\`
*   The 'Regenerate' button's JS **MUST use sessionStorage**: Store the current document's outerHTML in sessionStorage ('${SESSION_STORAGE_GAME_SOURCE}') and the new prompt in ('${SESSION_STORAGE_EDIT_PROMPT}'), then navigate to \`${absoluteGeneratorUrl}\` without parameters.`;

                    const requestBody = {
                        contents: [{ parts: [{ text: modificationPrompt }] }],
                        generationConfig: { maxOutputTokens: 8192 }
                    };
                    const apiResponse = await fetch(apiServiceEndpoint, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(requestBody),
                    });
                    const data = await apiResponse.json();
                    console.log("Received modification response.");
                    const modifiedHtml = data?.candidates?.[0]?.content?.parts?.[0]?.text;

                    if (modifiedHtml) {
                        console.log("Modification successful, navigating...");
                        setLoading(false);
                        navigateToGeneratedGame(modifiedHtml);
                    } else {
                        throw new Error("Gemini didn't provide modified game code.");
                    }
                } catch (error) {
                    console.error('Error during edit request:', error);
                    showTemporaryError(`Edit Failed: ${error.message}`);
                }
            }

            async function handleGenerateClick() {
                const userPrompt = gamePromptTextarea.value.trim();
                const selectedDeviceInput = document.querySelector('input[name="device"]:checked');
                const selectedStyleInput = document.querySelector('input[name="game-style"]:checked');

                if (!userPrompt || !selectedDeviceInput || !selectedStyleInput) {
                    alert('Please fill in all fields: Prompt, Device, and Style.');
                    if (!userPrompt) gamePromptTextarea.focus(); return;
                }

                const selectedDevice = selectedDeviceInput.value;
                const selectedStyle = selectedStyleInput.value;

                setLoading(true, "Gemini is generating your game...");
                generateButton.disabled = true;

                if (gameObjectUrl) { URL.revokeObjectURL(gameObjectUrl); gameObjectUrl = null; }

                const initialGenerationPrompt = `Hi Gemini! Generate a **complete, single HTML file** for a game:
${userPrompt}

**Specs:** Style: ${selectedStyle}, Device: ${selectedDevice}

**Output Requirements:**
*   **Only** raw HTML code (\`<!DOCTYPE html>...\`). Self-contained.
*   Complex JS (>500 lines ideal).
*   **Include the In-Game Menu/Edit System below (using sessionStorage).**

**Controls:** Computer: WASD/Arrows. Phone/2D: Joystick if move, no swipe cam. Phone/3D: Joystick AND swipe cam.

**In-Game Menu/Edit System (Mandatory Inclusion - Uses sessionStorage):**
*   **HTML (near end of <body>):**
    \`\`\`html
    <div id="inGameMenuContainer" style="position: fixed; bottom: 10px; right: 10px; z-index: 1000; font-family: sans-serif;">
        <button id="inGameMenuToggle" style="padding: 8px 12px; background-color: rgba(0,0,0,0.7); color: white; border: 1px solid white; border-radius: 50%; cursor: pointer; font-size: 16px;">...</button>
        <div id="inGameMenuPanel" style="position: absolute; bottom: 45px; right: 0; background-color: rgba(0,0,0,0.8); border: 1px solid #555; border-radius: 5px; padding: 10px; display: flex; flex-direction: column; gap: 8px; opacity: 0; visibility: hidden; transform: translateY(10px); transition: opacity 0.3s ease, transform 0.3s ease, visibility 0.3s;">
            <button id="inGameLeaveButton" style="padding: 5px 10px; background-color: #555; color: white; border: none; border-radius: 3px; cursor: pointer;">Leave</button>
            <button id="inGameEditButton" style="padding: 5px 10px; background-color: #337ab7; color: white; border: none; border-radius: 3px; cursor: pointer;">Edit</button>
        </div>
        <div id="inGameEditPanel" style="position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background-color: rgba(20, 20, 40, 0.95); border: 1px solid #88f; border-radius: 8px; padding: 20px; width: 90%; max-width: 400px; box-shadow: 0 5px 15px rgba(0,0,0,0.5); opacity: 0; visibility: hidden; transition: opacity 0.3s ease, visibility 0.3s; z-index: 1001;">
             <button id="inGameEditCloseButton" style="position: absolute; top: 5px; right: 10px; background: none; border: none; color: white; font-size: 20px; cursor: pointer;">&times;</button>
             <label for="inGameEditPrompt" style="display: block; margin-bottom: 8px; color: #ccf; font-size: 14px;">Edit Request:</label>
             <textarea id="inGameEditPrompt" rows="4" style="width: 100%; padding: 8px; border-radius: 4px; border: 1px solid #559; background-color: #112; color: white; font-size: 14px; margin-bottom: 10px; resize: vertical;"></textarea>
             <button id="inGameRegenerateButton" style="width: 100%; padding: 10px; background-color: #4CAF50; color: white; border: none; border-radius: 5px; cursor: pointer; font-size: 16px;">Regenerate</button>
        </div>
    </div>
    \`\`\`
*   **JavaScript (in final <script>):**
    \`\`\`javascript
    const menuToggle = document.getElementById('inGameMenuToggle');
    const menuPanel = document.getElementById('inGameMenuPanel');
    const editButton = document.getElementById('inGameEditButton');
    const leaveButton = document.getElementById('inGameLeaveButton');
    const editPanel = document.getElementById('inGameEditPanel');
    const editCloseButton = document.getElementById('inGameEditCloseButton');
    const regenerateButton = document.getElementById('inGameRegenerateButton');
    const editPromptTextarea = document.getElementById('inGameEditPrompt');

    const generatorPageUrl = '${absoluteGeneratorUrl}';
    const storageKeyPrompt = '${SESSION_STORAGE_EDIT_PROMPT}';
    const storageKeySource = '${SESSION_STORAGE_GAME_SOURCE}';

    if (menuToggle && menuPanel) {
        menuToggle.addEventListener('click', () => {
            const isVisible = menuPanel.style.opacity === '1';
            menuPanel.style.opacity = isVisible ? '0' : '1';
            menuPanel.style.visibility = isVisible ? 'hidden' : 'visible';
            menuPanel.style.transform = isVisible ? 'translateY(10px)' : 'translateY(0)';
        });
    } else { console.error("In-game menu toggle/panel missing!"); }

    if (leaveButton) {
        leaveButton.addEventListener('click', () => {
            window.location.href = generatorPageUrl;
        });
    } else { console.error("In-game leave button missing!"); }

    if (editButton && editPanel && menuPanel) {
         editButton.addEventListener('click', () => {
            editPanel.style.opacity = '1'; editPanel.style.visibility = 'visible';
            menuPanel.style.opacity = '0'; menuPanel.style.visibility = 'hidden'; menuPanel.style.transform = 'translateY(10px)';
        });
    } else { console.error("In-game edit button/panel missing!"); }

    if (editCloseButton && editPanel) {
        editCloseButton.addEventListener('click', () => {
            editPanel.style.opacity = '0'; editPanel.style.visibility = 'hidden';
        });
    } else { console.error("In-game edit close button/panel missing!"); }

    if (regenerateButton && editPromptTextarea) {
        regenerateButton.addEventListener('click', () => {
            const editPrompt = editPromptTextarea.value.trim();
            if (!editPrompt) { alert('Please enter an edit instruction.'); return; }

            try {
                const currentGameHtml = document.documentElement.outerHTML;
                sessionStorage.setItem(storageKeyPrompt, editPrompt);
                sessionStorage.setItem(storageKeySource, currentGameHtml);
                window.location.href = generatorPageUrl;
            } catch (e) {
                console.error("Error saving state to sessionStorage or navigating:", e);
                alert("Could not initiate regeneration. Error saving state.");
            }
        });
    } else { console.error("In-game regenerate button/prompt missing!"); }
    \`\`\`
Remember ONLY the final HTML code.`;

                const requestBody = {
                    contents: [{ parts: [{ text: initialGenerationPrompt }] }],
                    generationConfig: { maxOutputTokens: 8192 }
                };

                try {
                    const response = await fetch(apiServiceEndpoint, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(requestBody),
                    });

                    const data = await response.json();
                    console.log("Received initial generation response.");
                    const generatedHtml = data?.candidates?.[0]?.content?.parts?.[0]?.text;

                    if (generatedHtml) {
                        console.log("Initial generation successful, navigating...");
                        setLoading(false);
                        navigateToGeneratedGame(generatedHtml);
                    } else {
                        const errorDetails = data?.error?.message || "Gemini didn't provide initial game code.";
                        throw new Error(errorDetails);
                    }
                } catch (error) {
                    console.error('Fetch Error during initial generation:', error);
                    showTemporaryError(`Generation Failed: ${error.message}`);
                }
            }

            function navigateToGeneratedGame(htmlContent) {
                try {
                    const blob = new Blob([htmlContent], { type: 'text/html' });
                    if (gameObjectUrl) { URL.revokeObjectURL(gameObjectUrl); }
                    gameObjectUrl = URL.createObjectURL(blob);
                    console.log("Created object URL:", gameObjectUrl);
                    window.location.href = gameObjectUrl;
                } catch (e) {
                    console.error("Error creating Blob or navigating:", e);
                    showTemporaryError(`Launch Failed: ${e.message}`);
                    if (gameObjectUrl) { URL.revokeObjectURL(gameObjectUrl); gameObjectUrl = null; }
                }
            }

            function setLoading(isLoading, message = "Loading...", isError = false) {
                if (errorTimeoutId) { clearTimeout(errorTimeoutId); errorTimeoutId = null; }
                if (isLoading) {
                    loadingMessage.textContent = message;
                    loadingIndicator.classList.toggle('error-state', isError);
                    loadingIndicator.classList.add('visible');
                } else {
                    loadingIndicator.classList.remove('visible');
                    loadingIndicator.classList.remove('error-state');
                }
            }

            function showTemporaryError(message) {
                 setLoading(true, message, true);
                 errorTimeoutId = setTimeout(() => {
                     setLoading(false);
                     resetUI();
                 }, 4500);
            }

            function resetUI() {
                 setLoading(false);
                 generateButton.disabled = false;
                 generatorContainer.classList.remove('hidden');
            }

        });
    </script>
</body>
</html>
