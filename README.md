/this aris open ai code 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Chat Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class'
        }

    </script>
    <style>
        @keyframes fadeIn {
            from {
                opacity: 0;
            }

            to {
                opacity: 1;
            }
        }

        @keyframes pulse {

            0%,
            100% {
                opacity: 1;
            }

            50% {
                opacity: 0.5;
            }
        }

        .fade-in {
            animation: fadeIn 0.3s ease-out forwards;
            will-change: opacity, transform;
        }
        .typing-indicator span {
            animation: pulse 1.5s infinite ease-in-out;
            will-change: opacity;
        }

        .typing-indicator span:nth-child(2) {
            animation-delay: 0.2s;
        }

        .typing-indicator span:nth-child(3) {
            animation-delay: 0.4s;
        }

        .chat-height {
            height: calc(100vh - 200px);
        }

        @media (max-width: 640px) {
            .chat-height {
                height: calc(100vh - 180px);
            }
        }
    </style>
</head>

<body class="bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
    <div class="max-w-4xl mx-auto h-screen flex flex-col">
        <!-- Header -->
        <div id="navbar"
            class="bg-white dark:bg-gray-800 shadow-sm p-4 flex items-center justify-between border-b border-gray-200 dark:border-gray-700">
            <div class="flex items-center space-x-3">
                <img src="logo.png" alt="AI Assistant Logo" class="w-10 h-10 rounded-full" />
                <h1 class="text-xl font-bold text-gray-900 dark:text-white">aris</h1>
            </div>
            <!-- No additional <script src="..."> needed here; Tailwind is already loaded in <head> -->


            <div class="flex items-center space-x-2">
                <button type="button" id="theme-toggle"
                    class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors"
                    title="Toggle theme" aria-label="Toggle theme">
                    <svg id="theme-icon" class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none"
                        viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                    </svg>
                    <!-- Archive toggle button -->
                    <!-- Archive toggle button for sidebar (shows only on desktop) -->
                    <!-- <button type="button" id="sidebar-archive-toggle"
                        class="hidden md:inline-flex p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2"
                        title="Show/Hide Archive in Sidebar" aria-label="Show/Hide Archive in Sidebar">

                    </button> -->
                    <script>
                        // Archive toggle for sidebar
                        const sidebarArchiveToggle = document.getElementById('sidebar-archive-toggle');
                        const sidebarArchiveIcon = document.getElementById('sidebar-archive-icon');
                        if (sidebarArchiveToggle) {
                            sidebarArchiveToggle.addEventListener('click', () => {
                                showArchive = !showArchive;
                                sidebarArchiveIcon.classList.toggle('text-yellow-500', showArchive);
                                renderHistoryWithArchive();
                            });
                            sidebarArchiveToggle.style.display = 'none';
                        }
                        let lastLoadedIdx = null;
                        function loadHistory(idx) {
                            lastLoadedIdx = idx;
                            const messagesDiv = document.getElementById('messages');
                            messagesDiv.innerHTML = '';
                            currentSession = [];
                            chatHistory[idx].messages.forEach(msg => {
                                addMessage(msg.content, msg.isUser);
                                currentSession.push({ content: msg.content, isUser: msg.isUser });
                            });
                        }
                        // Override addMessage to handle session management
                        // const originalAddMessage = addMessage;
                        // let currentSession = [];
                        addMessage = function (content, isUser) {
                            originalAddMessage(content, isUser);
                            currentSession.push({ content, isUser });
                            // Save session as "New Chat" if first message, or use first user message as title
                            let title = currentSession.find(m => m.isUser)?.content?.slice(0, 30) || 'New Chat';
                            saveHistory(title, [...currentSession]);
                        };
                        // Initialize currentSession from last loaded history
                        if (lastLoadedIdx !== null && chatHistory[lastLoadedIdx]) {
                            currentSession = [...chatHistory[lastLoadedIdx].messages];
                        } else {
                            currentSession = [];
                            const navbarEl = document.getElementById('navbar');

                            if (navbarEl) {
                                navbarEl.classList.add('py-6', 'px-8');
                                navbarEl.classList.remove('p-4');
                                // Optionally increase text/icon size
                                navbarEl.querySelectorAll('h1').forEach(h => h.classList.add('text-2xl'));
                                navbarEl.querySelectorAll('svg').forEach(svg => svg.classList.add('w-6', 'h-6'));
                            }
                            /* Add navbar icons: share, three-dot (archive/delete), user login */
                            const navbar = document.getElementById('navbar');
                            /* Remove all children except the left (logo/name) and right (icons) containers, then rebuild the right side */
                            let left = null, right = null;
                            if (navbar) {
                                left = navbar.querySelector('.flex.items-center.space-x-3');
                                right = navbar.querySelector('.flex.items-center.space-x-2');
                            }
                        }
                        if (right) {
                            right.innerHTML = ''; // Clear right side
                            // Archive button
                            const archiveBtn = document.createElement('button');
                            archiveBtn.type = 'button';
                            archiveBtn.id = 'archive-toggle';
                            archiveBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2';
                            archiveBtn.title = 'Show/Hide Archive';
                            archiveBtn.setAttribute('aria-label', 'Show/Hide Archive');
                            archiveBtn.innerHTML = `
                                <svg id="archive-icon" class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none"
                                    viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M20 7v10a2 2 0 01-2 2H6a2 2 0 01-2-2V7m16 0V5a2 2 0 00-2-2H6a2 2 0 00-2 2v2m16 0H4" />
                                </svg>
                            `;

                            // Share button
                            const shareBtn = document.createElement('button');
                            shareBtn.type = 'button';
                            shareBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2';
                            shareBtn.title = 'Share';
                            shareBtn.setAttribute('aria-label', 'Share');
                            shareBtn.innerHTML = `
                                <svg class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 8a3 3 0 11-6 0 3 3 0 016 0zm6 8a6 6 0 10-12 0 6 6 0 0012 0zm-6-6v6" />
                                </svg>
                            `;
                            shareBtn.addEventListener('click', () => {
                                if (navigator.share) {
                                    navigator.share({
                                        title: document.title,
                                        url: window.location.href
                                    });
                                } else {
                                    prompt('Copy this link:', window.location.href);
                                }
                            });
                            // User/login button
                            const userBtn = document.createElement('button');
                            userBtn.type = 'button';
                            userBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2';
                            userBtn.title = 'User Login';
                            userBtn.setAttribute('aria-label', 'User Login');
                            userBtn.innerHTML = `
                                <svg class="w-6 h-6 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.121 17.804A9 9 0 1112 21a9 9 0 01-6.879-3.196z" />
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                                </svg>
                            `;
                            userBtn.addEventListener('click', () => {
                                alert('User login/profile action');
                            });
                            // Theme toggle button (already present)
                            const themeToggle = document.getElementById('theme-toggle');
                            if (themeToggle) {
                                right.appendChild(themeToggle);
                            }
                            right.appendChild(archiveBtn);
                            right.appendChild(shareBtn);
                            right.appendChild(userBtn);
                        }
                        if (navbar) {
                            // Create share button
                            const shareBtn = document.createElement('button');
                            shareBtn.type = 'button';
                            shareBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2';
                            shareBtn.title = 'Share';
                            shareBtn.innerHTML = `
                                <svg class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 8a3 3 0 11-6 0 3 3 0 016 0zm6 8a6 6 0 10-12 0 6 6 0 0012 0zm-6-6v6" />
                                </svg>
                            `;
                            shareBtn.addEventListener('click', () => {
                                if (navigator.share) {
                                    navigator.share({
                                        title: document.title,
                                        url: window.location.href
                                    });
                                } else {
                                    prompt('Copy this link:', window.location.href);
                                }
                            });

                            // Create three-dot menu button
                            const menuBtn = document.createElement('button');
                            menuBtn.type = 'button';
                            menuBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2 relative';
                            menuBtn.title = 'More options';
                            menuBtn.innerHTML = `
                                <svg class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <circle cx="5" cy="12" r="1.5"/>
                                    <circle cx="12" cy="12" r="1.5"/>
                                    <circle cx="19" cy="12" r="1.5"/>
                                </svg>
                            `;
                            // Remove or adjust these lines to avoid layout issues
                            // navbar.style.width = '1000%';
                            // navbar.style.maxWidth = 'max-content';
                            // Dropdown menu for three-dot
                            const menu = document.createElement('div');
                            menu.className = 'absolute right-0 mt-2 w-40 bg-white dark:bg-gray-800 rounded shadow-lg border border-gray-200 dark:border-gray-700 z-50 hidden';
                            menu.innerHTML = `
                                <button class="w-full text-left px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-700" id="navbar-archive">Archive</button>
                                <button class="w-full text-left px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-700 text-red-500" id="navbar-delete">Delete</button>
                            `;
                            menuBtn.appendChild(menu);
                            menuBtn.addEventListener('click', (e) => {
                                e.stopPropagation();
                                menu.classList.toggle('hidden');
                            });
                            document.addEventListener('click', () => menu.classList.add('hidden'));
                            menu.querySelector('#navbar-archive').addEventListener('click', (e) => {
                                e.stopPropagation();
                                alert('Archive action from navbar');
                                menu.classList.add('hidden');
                            });
                            menu.querySelector('#navbar-delete').addEventListener('click', (e) => {
                                e.stopPropagation();
                                alert('Delete action from navbar');
                                menu.classList.add('hidden');
                            });

                            // Create user login icon
                            const userBtn = document.createElement('button');
                            userBtn.type = 'button';
                            userBtn.className = 'p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2';
                            userBtn.title = 'User Login';
                            userBtn.innerHTML = `
                                <svg class="w-6 h-6 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.121 17.804A9 9 0 1112 21a9 9 0 01-6.879-3.196z" />
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                                </svg>
                            `;
                            userBtn.addEventListener('click', () => {
                                alert('User login/profile action');
                            });

                            // Insert icons before the archive-toggle button
                            const archiveToggle = document.getElementById('archive-toggle');
                            if (archiveToggle) {
                                archiveToggle.parentNode.insertBefore(shareBtn, archiveToggle);
                                archiveToggle.parentNode.insertBefore(menuBtn, archiveToggle);
                                archiveToggle.parentNode.insertBefore(userBtn, archiveToggle.nextSibling);
                            }
                        }
                        // Initialize lastLoadedIdx to null
                        lastLoadedIdx = null;
                        // Add logic to handle session management
                        // This will be used to determine if we are continuing a session or starting a new one

                        // Initialize lastLoadedIdx to null
                        lastLoadedIdx = null; // No chat loaded initially
                        // Initialize currentSession to an empty array
                        /*
                         * When loading a chat from history, continue in that session (no "New Chat" is created).
                         * When sending a message, if lastLoadedIdx is set, append to that session.
                         * If not, create a new session.
                         * This logic is handled in the overridden addMessage below.
                         */

                        // When sending a message, continue in the current session if loaded from history
                        const originalAddMessageForContinue = addMessage;
                        addMessage = function (content, isUser) {
                            originalAddMessageForContinue(content, isUser);
                            if (lastLoadedIdx !== null && chatHistory[lastLoadedIdx]) {
                                currentSession.push({ content, isUser });
                                chatHistory[lastLoadedIdx].messages = [...currentSession];
                                // Update title if first user message
                                let title = currentSession.find(m => m.isUser)?.content?.slice(0, 30) || 'New Chat';
                                chatHistory[lastLoadedIdx].title = title;
                                localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                                renderHistoryWithArchive();
                            } else {
                                currentSession.push({ content, isUser });
                                let title = currentSession.find(m => m.isUser)?.content?.slice(0, 30) || 'New Chat';
                                saveHistory(title, [...currentSession]);
                                lastLoadedIdx = 0;
                            }
                        };
                    </script>
                    <button type="button" id="archive-toggle"
                        class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors ml-2"
                        title="Show/Hide Archive" aria-label="Show/Hide Archive">
                        <svg id="archive-icon" class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none"
                            viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M20 7v10a2 2 0 01-2 2H6a2 2 0 01-2-2V7m16 0V5a2 2 0 00-2-2H6a2 2 0 00-2 2v2m16 0H4" />
                        </svg>
                    </button>
                    
                    <script>

                        // Archive toggle 
                        /* Move sidebar toggle button below the header/logo on mobile */
                        function adjustSidebarToggleBtn() {
                            const sidebarToggleBtn = document.getElementById('sidebar-toggle');
                            const header = document.querySelector('.bg-white.shadow-sm.p-4.flex.items-center.justify-between');

                            if (window.innerWidth < 768 && sidebarToggleBtn && header) {
                                header.parentNode.insertBefore(sidebarToggleBtn, header.firstChild); // insert at start of header

                                sidebarToggleBtn.style.position = 'relative';
                                sidebarToggleBtn.style.left = '0';
                                sidebarToggleBtn.style.transform = 'none';
                                sidebarToggleBtn.style.marginTop = '0';
                                sidebarToggleBtn.style.marginLeft = '0.5rem';
                            } else if (sidebarToggleBtn) {
                                // Restore to fixed position for desktop
                                sidebarToggleBtn.style.position = 'fixed';
                                sidebarToggleBtn.style.top = '1rem';
                                sidebarToggleBtn.style.left = '1rem';
                                sidebarToggleBtn.style.transform = '';
                                sidebarToggleBtn.style.marginTop = '';
                                sidebarToggleBtn.style.marginLeft = '';
                            }
                        }

                        window.addEventListener('resize', adjustSidebarToggleBtn);
                        document.addEventListener('DOMContentLoaded', adjustSidebarToggleBtn);
                        adjustSidebarToggleBtn();
                        let showArchive = false;
                        const archiveToggle = document.getElementById('archive-toggle');
                        const archiveIcon = document.getElementById('archive-icon');

                        // Make sure historyList is defined before using it
                        // Only assign if not already declared
                        // Removed redeclaration to avoid SyntaxError
                        if (typeof sidebarArchiveToggle === 'undefined') {
                            sidebarArchiveToggle = document.getElementById('sidebar-archive-toggle');
                        }
                        if (sidebarArchiveToggle) {
                            sidebarArchiveToggle.style.display = 'inline-flex';
                        }

                        // Ensure sidebar overlays header/logo on mobile
                        function adjustSidebarZIndex() {
                            const sidebar = document.getElementById('history-sidebar');
                            if (!sidebar) return;
                            if (window.innerWidth < 768) {
                                sidebar.style.zIndex = '50';
                            } else {
                                sidebar.style.zIndex = '30';
                            }
                        }
                        window.addEventListener('resize', adjustSidebarZIndex);
                        document.addEventListener('DOMContentLoaded', adjustSidebarZIndex);
                        adjustSidebarZIndex();
                        // Make sure historyList is defined before using it
                        if (typeof historyList === 'undefined') {
                            historyList = document.getElementById('history-list');
                        }

                        function renderHistoryWithArchive() {
                            // Ensure historyList is assigned before use
                            if (!historyList) {
                                historyList = document.getElementById('history-list');
                                if (!historyList) return;
                            }
                            historyList.innerHTML = '';
                            let filtered = chatHistory.filter(item => showArchive ? item.archived : !item.archived);
                            if (filtered.length === 0) {
                                historyList.innerHTML = `<li class="text-gray-400 text-center py-8">${showArchive ? 'No archived chats' : 'No history yet'}</li>`;
                                return;
                            }
                            filtered.forEach((item, idx) => {
                                // Find the real index in chatHistory
                                const realIdx = chatHistory.indexOf(item);
                                const li = document.createElement('li');
                                li.className = 'group flex items-center justify-between bg-gray-100 dark:bg-gray-800 rounded px-3 py-2 cursor-pointer hover:bg-purple-100 dark:hover:bg-purple-900/40 transition relative';
                                const menuBtn = document.createElement('button');
                                menuBtn.type = 'button';
                                menuBtn.className = 'history-menu-btn ml-2 text-gray-400 hover:text-gray-700 dark:hover:text-gray-200 p-1 rounded-full focus:outline-none';
                                menuBtn.title = 'Options';
                                menuBtn.innerHTML = `
                                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <circle cx="5" cy="12" r="1.5"/>
                                    <circle cx="12" cy="12" r="1.5"/>
                                    <circle cx="19" cy="12" r="1.5"/>
                                    </svg>
                                `;
                                const menu = document.createElement('div');
                                menu.className = 'history-options-menu absolute right-0 mt-1 shadow-lg border dark:border-gray-700';
                                menu.innerHTML = `
                                    <button class="rename-history">Rename</button>
                                    <button class="archive-history">${showArchive ? 'Remove from Archive' : 'Archive'}</button>
                                    <button class="delete-history text-red-500">Delete</button>
                                `;
                                menuBtn.addEventListener('click', (e) => {
                                    e.stopPropagation();
                                    document.querySelectorAll('.history-options-menu').forEach(m => m.style.display = 'none');
                                    menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                                });
                                document.addEventListener('click', () => { menu.style.display = 'none'; });
                                menu.querySelector('.rename-history').addEventListener('click', (e) => {
                                    e.stopPropagation();
                                    menu.style.display = 'none';
                                    const newTitle = prompt('Rename chat:', chatHistory[realIdx].title);
                                    if (newTitle && newTitle.trim()) {
                                        chatHistory[realIdx].title = newTitle.trim();
                                        localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                                        renderHistoryWithArchive();
                                    }
                                });
                                menu.querySelector('.archive-history').addEventListener('click', (e) => {
                                    e.stopPropagation();
                                    menu.style.display = 'none';
                                    chatHistory[realIdx].archived = !showArchive;
                                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                                    renderHistoryWithArchive();
                                });
                                menu.querySelector('.delete-history').addEventListener('click', (e) => {
                                    e.stopPropagation();
                                    menu.style.display = 'none';
                                    chatHistory.splice(realIdx, 1);
                                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                                    renderHistoryWithArchive();
                                });
                                li.innerHTML = `
                                    <span class="truncate text-gray-900 dark:text-white">${item.title}</span>
                                `;
                                li.appendChild(menuBtn);
                                li.appendChild(menu);
                                li.addEventListener('click', (e) => {
                                    if (e.target.closest('.history-menu-btn') || e.target.closest('.history-options-menu')) return;
                                    loadHistory(realIdx);
                                    if (window.innerWidth < 768) closeSidebar();
                                });
                                historyList.appendChild(li);
                            });
                        }
                        // Override renderHistory
                        renderHistory = renderHistoryWithArchive;
                        archiveToggle.addEventListener('click', () => {
                            showArchive = !showArchive;
                            archiveIcon.classList.toggle('text-yellow-500', showArchive);
                            renderHistoryWithArchive();
                        });
                        // On load, render history (not archived)
                        document.addEventListener('DOMContentLoaded', () => {
                            renderHistoryWithArchive();
                        });
                    </script>
                    <!-- Replace placeholder image with your logo -->
                    <!-- No additional logo needed here; the AI avatar/logo is already shown with each AI message in the chat area. If you want to ensure the AI's logo (logo.png) is used for all AI answers, update the <img> src in addMessage: -->
                    <!-- No additional logo needed here; the AI avatar/logo is already shown with each AI message in the chat area. -->
                    <!-- In the addMessage function, replace: -->
                    <!-- <img src="https://placehold.co/30x30" alt="AI Avatar" class="w-8 h-8 rounded-full mt-1" /> -->
                    <!-- with: -->
                    <!-- <img src="logo.png" alt="AI Assistant Logo" class="w-8 h-8 rounded-full mt-1" /> -->

                </button>
            </div>
        </div>

        <!-- Chat area -->
        <div class="flex-1 overflow-y-auto p-4 chat-height" id="chat-container">
            <div class="flex flex-col space-y-4" id="messages">
                <!-- Welcome message -->
                <div class="flex flex-col items-start fade-in">
                    <div class="flex items-start space-x-2 max-w-xs sm:max-w-md md:max-w-lg">
                        <img src="logo.png" alt="AI Avatar" class="w-8 h-8 rounded-full mt-1" />
                        <div class="bg-purple-100 dark:bg-purple-900/50 p-3 rounded-2xl rounded-tl-none">
                            <p class="text-gray-800 dark:text-gray-100">Hello! I'm your smart assistant here to answer
                                all your questions in real time.</p>
                            <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Just now</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Input area -->
        <div class="bg-white dark:bg-gray-800 border-t border-gray-200 dark:border-gray-700 p-4">
            <form id="message-form" class="flex items-center space-x-2">
                <div class="flex-1 relative">
                    <input type="text" id="user-input" placeholder="Ask me anything..."
                        class="w-full p-4 pr-12 rounded-full border border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-700 text-gray-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent transition-all" />
                    <button type="button" id="clear-btn"
                        class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600 dark:hover:text-gray-300"
                        title="Clear input" aria-label="Clear input">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
                <button type="submit"
                    class="p-3 bg-blue-600 text-white rounded-full hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors"
                    title="Send message" aria-label="Send message">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                    </svg>
                </button>
            </form>
            <p class="text-xs text-gray-500 dark:text-gray-400 mt-2 text-center">Responses are generated in real-time
                and may require verification.</p>
        </div>
    </div>

    <script>
        /* Side history bar functionality */

        // Create sidebar HTML
        const sidebar = document.createElement('aside');
        sidebar.id = 'history-sidebar';
        sidebar.className = 'fixed left-0 top-0 h-full w-64 bg-white dark:bg-gray-900 border-r border-gray-200 dark:border-gray-700 shadow-lg z-30 flex flex-col transition-transform transform -translate-x-full md:translate-x-0';
        sidebar.innerHTML = `
            <div class="flex items-center justify-between p-4 border-b border-gray-200 dark:border-gray-700">
                <span class="font-bold text-lg text-gray-900 dark:text-white">History</span>
                <button id="sidebar-close" class="md:hidden p-2 rounded hover:bg-gray-100 dark:hover:bg-gray-800" title="Close sidebar">
                    <svg class="w-5 h-5 text-gray-600 dark:text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
            <ul id="history-list" class="flex-1 overflow-y-auto p-2 space-y-2"></ul>
            <div class="p-4 border-t border-gray-200 dark:border-gray-700">
                <button id="clear-history" class="w-full py-2 px-3 bg-red-500 text-white rounded hover:bg-red-600 text-sm">Clear History</button>
            </div>
        `;
        // Add "three dot" (options) button to each history item
        // This will be handled in renderHistory by adding a menu button and dropdown
        // Remove "Clear History" button and replace with icon-only button
        // sidebar.querySelector('#clear-history').outerHTML = '';
        sidebar.querySelector('#clear-history').outerHTML = `
            <button id="clear-history" class="w-10 h-10 flex items-center justify-center mx-auto bg-red-500 text-white rounded-full hover:bg-red-600 text-sm" title="Clear History" aria-label="Clear History">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
            </button>
            
        `;
        sidebar.querySelector('#clear-history').outerHTML = `
            <button id="clear-history" class="w-10 h-10 flex items-center justify-center mx-auto bg-red-500 text-white rounded-full hover:bg-red-600 text-sm" title="Clear History" aria-label="Clear History">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
            </button>
        `;
        // Add styles for the dropdown menu
        const style = document.createElement('style');
        style.innerHTML = `
        .history-options-menu {
            display: none;
            position: absolute;
            right: 0.5rem;
            top: 2.5rem;
            z-index: 50;
            min-width: 8rem;
            background: white;
            border-radius: 0.5rem;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
            border: 1px solid #e5e7eb;
        }
        .dark .history-options-menu {
            background: #1f2937;
            border-color: #374151;
        }
        .history-options-menu button {
            width: 100%;
            text-align: left;
            padding: 0.5rem 0.5rem;
            background: none;
            border: none;
            color: #374151;
            font-size: 0.95rem;
            cursor: pointer;
        }
        .dark .history-options-menu button {
            color:#d1d5db;
        }
        .history-options-menu button:hover {
            background: #f3f4f6;
        }
        .dark .history-options-menu button:hover {
            background: #4b5563;
        }
        `;
        document.head.appendChild(style);

        // Patch renderHistory to add three-dot menu
        const originalRenderHistory = renderHistory;
        renderHistory = function () {
            historyList.innerHTML = '';
            if (chatHistory.length === 0) {
                historyList.innerHTML = `<li class="text-gray-400 text-center py-8">No history yet</li>`;
                return;
            }
            chatHistory.forEach((item, idx) => {
                const li = document.createElement('li');
                li.className = 'group flex items-center justify-between bg-gray-100 dark:bg-gray-800 rounded px-3 py-2 cursor-pointer hover:bg-purple-100 dark:hover:bg-purple-900/40 transition';

                // Three-dot button
                const menuBtn = document.createElement('button');
                menuBtn.type = 'button';
                menuBtn.className = 'history-menu-btn ml-2 text-gray-400 hover:text-gray-700 dark:hover:text-gray-200 p-1 rounded-full focus:outline-none';
                menuBtn.title = 'Options';
                menuBtn.innerHTML = `
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <circle cx="5" cy="12" r="1.5"/>
                <circle cx="12" cy="12" r="1.5"/>
                <circle cx="19" cy="12" r="1.5"/>
                </svg>
            `;

                // Dropdown menu
                const menu = document.createElement('div');
                menu.className = 'history-options-menu absolute right-0 mt-1 shadow-lg border dark:border-gray-700';
                menu.innerHTML = `
                <button class="rename-history">Rename</button>
                <button class="archive-history">Archive</button>
                <button class="delete-history text-red-500">Delete</button>
            `;

                // Show/hide menu logic
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    // Hide all other menus
                    document.querySelectorAll('.history-options-menu').forEach(m => m.style.display = 'none');
                    menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                });
                // Hide menu on click outside
                document.addEventListener('click', () => { menu.style.display = 'none'; });

                // Rename
                menu.querySelector('.rename-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    const newTitle = prompt('Rename chat:', chatHistory[idx].title);
                    if (newTitle && newTitle.trim()) {
                        chatHistory[idx].title = newTitle.trim();
                        localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                        renderHistory();
                    }
                });
                // Archive (just mark as archived, not implemented in UI)
                menu.querySelector('.archive-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    chatHistory[idx].archived = true;
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistory();
                });
                // Delete
                menu.querySelector('.delete-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    chatHistory.splice(idx, 1);
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistory();
                });

                li.innerHTML = `
                <span class="truncate text-gray-900 dark:text-white">${item.title}</span>
            `;
                li.appendChild(menuBtn);
                li.appendChild(menu);

                li.addEventListener('click', (e) => {
                    if (e.target.closest('.delete-history')) return;
                    loadHistory(idx);
                    if (window.innerWidth < 768) closeSidebar();
                });
                li.querySelector('.delete-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    chatHistory.splice(idx, 1);
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistory();
                });
                historyList.appendChild(li);
            });
        };
        /*
         * Use a single archive icon for both "Archive" and "Remove from Archive" actions in the history menu.
         * The icon will visually indicate archived state (e.g., yellow when archived).
         */
        function getArchiveIcon(isArchived) {
            return `
                <svg class="w-5 h-5 ${isArchived ? 'text-yellow-500' : 'text-gray-400'}" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7v10a2 2 0 01-2 2H6a2 2 0 01-2-2V7m16 0V5a2 2 0 00-2-2H6a2 2 0 00-2 2v2m16 0H4" />
                </svg>
            `;
        }
        function renderHistoryWithArchive() {
            historyList.innerHTML = '';
            let filtered = chatHistory.filter(item => showArchive ? item.archived : !item.archived);
            if (filtered.length === 0) {
                historyList.innerHTML = `<li class="text-gray-400 text-center py-8">${showArchive ? 'No archived chats' : 'No history yet'}</li>`;
                return;
            }

            filtered.forEach((item, idx) => {
                const realIdx = chatHistory.indexOf(item);
                const li = document.createElement('li');
                li.className = 'group flex items-center justify-between bg-gray-100 dark:bg-gray-800 rounded px-3 py-2 cursor-pointer hover:bg-purple-100 dark:hover:bg-purple-900/40 transition relative';
                const menuBtn = document.createElement('button');
                menuBtn.type = 'button';
                menuBtn.className = 'history-menu-btn ml-2 text-gray-400 hover:text-gray-700 dark:hover:text-gray-200 p-1 rounded-full focus:outline-none';
                menuBtn.title = 'Options';
                menuBtn.innerHTML = `
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <circle cx="5" cy="12" r="1.5"/>
                    <circle cx="12" cy="12" r="1.5"/>
                    <circle cx="19" cy="12" r="1.5"/>
                    </svg>
                `;
                const menu = document.createElement('div');
                menu.className = 'history-options-menu absolute right-0 mt-1 shadow-lg border dark:border-gray-700';
                menu.innerHTML = `
                    <button class="rename-history">Rename</button>
                    <button class="archive-history flex items-center gap-2">${getArchiveIcon(item.archived)}<span>${item.archived ? 'Remove from Archive' : 'Archive'}</span></button>
                    <button class="delete-history text-red-500">Delete</button>
                `;
                menuBtn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    document.querySelectorAll('.history-options-menu').forEach(m => m.style.display = 'none');
                    menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                });
                document.addEventListener('click', () => { menu.style.display = 'none'; });
                menu.querySelector('.rename-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    const newTitle = prompt('Rename chat:', chatHistory[realIdx].title);
                    if (newTitle && newTitle.trim()) {
                        chatHistory[realIdx].title = newTitle.trim();
                        localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                        renderHistoryWithArchive();
                    }
                });
                menu.querySelector('.archive-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    chatHistory[realIdx].archived = !chatHistory[realIdx].archived;
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistoryWithArchive();
                });
                menu.querySelector('.delete-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    menu.style.display = 'none';
                    chatHistory.splice(realIdx, 1);
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistoryWithArchive();
                });
                li.innerHTML = `
                    <span class="truncate text-gray-900 dark:text-white">${item.title}</span>
                `;
                li.appendChild(menuBtn);
                li.appendChild(menu);
                li.addEventListener('click', (e) => {
                    if (e.target.closest('.history-menu-btn') || e.target.closest('.history-options-menu')) return;
                    loadHistory(realIdx);
                    if (window.innerWidth < 768) closeSidebar();
                });
                historyList.appendChild(li);
            });
        }
        // Override renderHistory
        renderHistory = renderHistoryWithArchive;
        // Insert sidebar into body
        document.body.appendChild(sidebar);
        // Assign historyList after sidebar is in the DOM
        historyList = document.getElementById('history-list');
        // Shift main content right to make space for sidebar on desktop
        function adjustMainContent() {
            const mainContainer = document.querySelector('.max-w-4xl.mx-auto.h-screen.flex.flex-col');
            if (window.innerWidth >= 768) {
                mainContainer.style.marginLeft = '17rem'; // 64px * 4 = 256px = 17rem
            } else {
                mainContainer.style.marginLeft = '1.5rem'; // 64px for mobile
            }
        }
        adjustMainContent();
        window.addEventListener('resize', adjustMainContent);
        // Sidebar toggle button (hamburger)
        const sidebarToggleBtn = document.createElement('button');
        sidebarToggleBtn.id = 'sidebar-toggle';
        sidebarToggleBtn.className = 'fixed left-4 top-4 z-40 md:hidden p-2 rounded-full bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-700 shadow hover:bg-gray-100 dark:hover:bg-gray-800';
        sidebarToggleBtn.innerHTML = `
            <svg class="w-6 h-6 text-gray-700 dark:text-gray-200" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
        `;
        document.body.appendChild(sidebarToggleBtn);

        // Responsive sidebar open/close
        function openSidebar() {
            sidebar.classList.remove('-translate-x-full');
        }
        function closeSidebar() {
            sidebar.classList.add('-translate-x-full');
        }
        sidebarToggleBtn.addEventListener('click', openSidebar);
        sidebar.querySelector('#sidebar-close').addEventListener('click', closeSidebar);

        // Hide sidebar on small screens by default
        if (window.innerWidth < 768) closeSidebar();
        window.addEventListener('resize', () => {
            if (window.innerWidth < 768) closeSidebar();
            else openSidebar();
        });
        // Assign historyList after sidebar is appended to the DOM
        var historyList = document.getElementById('history-list');
        let chatHistory = JSON.parse(localStorage.getItem('chatHistory') || '[]');
        var historyList = document.getElementById('history-list');

        // Save a new chat session
        function saveHistory(title, messages) {
            chatHistory.unshift({ title, messages, time: Date.now() });
            if (chatHistory.length > 30) chatHistory = chatHistory.slice(0, 30);
            localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
            renderHistory();
        }

        // Render history list
        function renderHistory() {
            historyList.innerHTML = '';
            if (chatHistory.length === 0) {
                historyList.innerHTML = `<li class="text-gray-400 text-center py-8">No history yet</li>`;
                return;
            }
            chatHistory.forEach((item, idx) => {
                const li = document.createElement('li');
                li.className = 'group flex items-center justify-between bg-gray-100 dark:bg-gray-800 rounded px-3 py-2 cursor-pointer hover:bg-purple-100 dark:hover:bg-purple-900/40 transition';
                li.innerHTML = `
                    <span class="truncate text-gray-900 dark:text-white">${item.title}</span>
                    <button class="delete-history ml-2 text-gray-400 hover:text-red-500" title="Delete">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                `;

                li.addEventListener('click', (e) => {
                    if (e.target.closest('.delete-history')) return;
                    loadHistory(idx);
                    if (window.innerWidth < 768) closeSidebar();
                });
                li.querySelector('.delete-history').addEventListener('click', (e) => {
                    e.stopPropagation();
                    chatHistory.splice(idx, 1);
                    localStorage.setItem('chatHistory', JSON.stringify(chatHistory));
                    renderHistory();
                });
                historyList.appendChild(li);
            });
        }

        // Load a chat session from history
        function loadHistory(idx) {
            const messagesDiv = document.getElementById('messages');
            messagesDiv.innerHTML = '';
            chatHistory[idx].messages.forEach(msg => {
                addMessage(msg.content, msg.isUser);
            });
        }

        // Clear all history
        document.getElementById('clear-history').addEventListener('click', () => {
            if (confirm('Clear all chat history?')) {
                chatHistory = [];
                localStorage.removeItem('chatHistory');
                renderHistory();
            }
        });

        // Save chat after each user/AI message
        const originalAddMessage = addMessage;
        let currentSession = [];
        addMessage = function (content, isUser) {
            originalAddMessage(content, isUser);
            currentSession.push({ content, isUser });
            // Save session as "New Chat" if first message, or use first user message as title
            let title = currentSession.find(m => m.isUser)?.content?.slice(0, 30) || 'New Chat';
            saveHistory(title, [...currentSession]);
        };

        // On page load, render history
        renderHistory();
        // WebSocket connection
        let socket;
let reconnectAttempts = 0;
const maxReconnectAttempts = 5;

function connectWebSocket() {
    socket = new WebSocket('ws://localhost:8080');

    socket.onopen = () => {
        reconnectAttempts = 0;
        addMessage('✅ Connected to server.', false);
        userInput.disabled = false;
        messageForm.querySelector('button[type="submit"]').disabled = false;
    };

    socket.onclose = () => {
        addMessage('⚠️ Disconnected from server. Attempting to reconnect...', false);
        userInput.disabled = true;
        messageForm.querySelector('button[type="submit"]').disabled = true;
        if (reconnectAttempts < maxReconnectAttempts) {
            reconnectAttempts++;
            setTimeout(connectWebSocket, 1000 * reconnectAttempts); // Exponential backoff
        } else {
            addMessage('❌ Unable to reconnect to server. Please reload the page.', false);
        }
    };

    socket.onerror = (err) => {
        addMessage('❌ WebSocket error. See console for details.', false);
        console.error('WebSocket error:', err);
    };

    socket.onmessage = (event) => {
        const response = event.data;
        addMessage(response, false);
        if (!response.startsWith('🕒') && !response.startsWith('⏳')) {
            userInput.disabled = false;
            messageForm.querySelector('button[type="submit"]').disabled = false;
            userInput.focus();
            waitingForResponse = false;
        }
    };
}

connectWebSocket();

        // Theme toggle functionality
        const themeToggle = document.getElementById('theme-toggle');
        const themeIcon = document.getElementById('theme-icon');
        const html = document.documentElement;

        function setThemeIcon(isDark) {
            // Remove all children from themeIcon
            while (themeIcon.firstChild) {
                themeIcon.removeChild(themeIcon.firstChild);
            }
            themeIcon.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
            // Create new path element
            const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
            path.setAttribute('stroke-linecap', 'round');
            path.setAttribute('stroke-linejoin', 'round');
            path.setAttribute('stroke-width', '2');
            if (isDark) {
                path.setAttribute('d', 'M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z');
            } else {
                path.setAttribute('d', 'M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z');
            }
            themeIcon.appendChild(path);
        }

        const currentTheme = localStorage.getItem('theme') || 'light';
        if (currentTheme === 'dark') {
            html.classList.add('dark');
            setThemeIcon(true);
        } else {
            setThemeIcon(false);
        }

        themeToggle.addEventListener('click', () => {
            const isDark = !html.classList.contains('dark');
            html.classList.toggle('dark');
            localStorage.setItem('theme', isDark ? 'dark' : 'light');
            setThemeIcon(isDark);
        });

        // Chat functionality
        const messageForm = document.getElementById('message-form');
        const userInput = document.getElementById('user-input');
        const chatContainer = document.getElementById('chat-container');
        const clearBtn = document.getElementById('clear-btn');

        function addMessage(content, isUser) {
            const messageElement = document.createElement('div');
            messageElement.className = `flex flex-col items-${isUser ? 'end' : 'start'} fade-in`;

            const timestamp = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });

            // Message bubble and avatar
            let inner = '';
            if (isUser) {
                inner = `
                <div class="flex items-end space-x-2 justify-end max-w-xs sm:max-w-md md:max-w-lg">
                    <div class="bg-blue-600 text-white p-3 rounded-2xl rounded-br-none shadow">
                        <p>${content.replace(/\n/g, '<br>')}</p>
                        <p class="text-xs text-gray-200 mt-1 text-right">${timestamp}</p>
                    </div>
                    <img src="image.png" alt="User Avatar" class="w-8 h-8 rounded-full ml-1 mt-1" />
                </div>
                `;
            } else {
                inner = `
                <div class="flex items-start space-x-2 max-w-xs sm:max-w-md md:max-w-lg">
                    <img src="logo.png" alt="AI Assistant Logo" class="w-8 h-8 rounded-full mt-1" />
                    <div class="bg-purple-100 dark:bg-purple-900/50 p-3 rounded-2xl rounded-tl-none">
                        <p class="text-gray-800 dark:text-gray-100">${content.replace(/\n/g, '<br>')}</p>
                        <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">${timestamp}</p>
                    </div>
                </div>
                `;
            }
            messageElement.innerHTML = inner;
            document.getElementById('messages').appendChild(messageElement);
            // Scroll to bottom
            const chatContainer = document.getElementById('chat-container');
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // Prevent multiple requests: disable input and send button while waiting
        let waitingForResponse = false;

        messageForm.addEventListener('submit', (e) => {
            e.preventDefault();
            if (waitingForResponse) return;
            const message = userInput.value.trim();
            if (!message) return;
            addMessage(message, true);
            if (socket.readyState === WebSocket.OPEN) {
                socket.send(message);
            } else {
                addMessage("⚠️ Unable to send message: not connected to server.", false);
            }
            userInput.value = '';
            userInput.disabled = true;
            messageForm.querySelector('button[type="submit"]').disabled = true;
            waitingForResponse = true;
        });

        socket.onmessage = (event) => {
            const response = event.data;
            // Always show server messages (queue, rate limit, etc.) as system messages
            addMessage(response, false);
            // Only re-enable input if the response is not a queue or retry message
            if (!response.startsWith('🕒') && !response.startsWith('⏳')) {
                userInput.disabled = false;
                messageForm.querySelector('button[type="submit"]').disabled = false;
                userInput.focus();
                waitingForResponse = false;
            }
        };
        // Handle Enter key to submit message
        userInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                messageForm.dispatchEvent(new Event('submit'));
            }
        });
        // Handle Escape key to clear input
        userInput.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                e.preventDefault();
                userInput.value = '';
                userInput.focus();
            }
        });
        // Handle click outside to close dropdown menus
        document.addEventListener('click', (e) => {
            if (!e.target.closest('.history-menu-btn') && !e.target.closest('.history-options-menu')) {
                document.querySelectorAll('.history-options-menu').forEach(menu => {
                    menu.style.display = 'none';
                });
            }
        });
        // Handle click outside to close sidebar
        document.addEventListener('click', (e) => {
            if (!e.target.closest('#history-sidebar') && !e.target.closest('#sidebar-toggle')) {
                closeSidebar();
            }
        });
        // Handle click on history toggle button
        document.getElementById('sidebar-toggle').addEventListener('click', () => {
            toggleSidebar();
        });
        // Function to toggle sidebar visibility

        function toggleSidebar() {
            if (sidebar.classList.contains('-translate-x-full')) {
                openSidebar();
            } else {
                closeSidebar();
            }
        }
        // Initial render of history
        renderHistoryWithArchive();
        // Add event listener to clear history button
        document.getElementById('clear-history').addEventListener('click', () => {
            if (confirm('Are you sure you want to clear all chat history?')) {
                chatHistory = [];
                localStorage.removeItem('chatHistory');
                renderHistoryWithArchive();
            }
        });
        // only use one archive toggle button
        // (Removed duplicate archiveToggle declaration and related code to avoid redeclaration error)
    </script>
</body>

</html>
