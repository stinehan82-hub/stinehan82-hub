## Hi there 👋

This is a test site

<!DOCTYPE html>
<html lang="no">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Min Superkul Ukeplan</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        .animate-bounce {
            animation: bounce 1s infinite;
        }
    </style>
</head>
<body>
    <div class="min-h-screen bg-gradient-to-br from-indigo-200 via-purple-200 to-pink-200 p-6">
        <div class="max-w-7xl mx-auto">
            <!-- Header -->
            <div class="text-center mb-8">
                <h1 class="text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-purple-600 via-pink-600 to-orange-500 mb-2">
                    🎪 Min superkule ukeplan! 🎪
                </h1>
            </div>

            <div class="flex gap-8 items-stretch">
                <!-- Chart container -->
                <div class="flex-1">
                    <!-- Chart -->
                    <div class="bg-white rounded-3xl shadow-2xl overflow-hidden border-4 border-purple-300">
                        <div class="overflow-x-auto">
                            <table class="w-full" id="choreTable">
                                <!-- Header row will be inserted here -->
                            </table>
                        </div>
                    </div>

                    <!-- Reset button -->
                    <div class="flex justify-center mt-8">
                        <button 
                            onclick="resetWeek()"
                            class="px-8 py-4 bg-gradient-to-r from-orange-400 to-red-500 hover:from-orange-500 hover:to-red-600 text-white font-black rounded-2xl shadow-xl transition-all transform hover:scale-110 text-lg"
                        >
                            🔄 TILBAKESTILL UKEN 🔄
                        </button>
                    </div>

                    <!-- Info text -->
                    <div class="text-center mt-8">
                        <p class="text-lg font-bold text-purple-800 bg-yellow-200 rounded-2xl py-3 px-6 inline-block">
                            🎯 Klikk på rutene for å få stjerner når oppgavene er gjort! 🌟
                        </p>
                    </div>
                </div>

                <!-- Criteria explanation -->
                <div class="w-80 flex items-center">
                    <div class="bg-white rounded-2xl shadow-lg p-6 border-4 border-purple-300 w-full">
                        <h2 class="text-2xl font-black text-purple-700 mb-4 text-center">📋 Krav for<br>ukepenger:</h2>
                        <div class="space-y-3">
                            <div class="flex items-center justify-between bg-purple-50 p-3 rounded-lg">
                                <span class="text-base font-bold text-gray-800">🧸 Rydde leker</span>
                                <span class="text-xl">⭐</span>
                            </div>
                            <div class="flex items-center justify-between bg-pink-50 p-3 rounded-lg">
                                <span class="text-base font-bold text-gray-800">🍱 Matboksen</span>
                                <span class="text-xl">⭐⭐⭐</span>
                            </div>
                            <div class="flex items-center justify-between bg-purple-50 p-3 rounded-lg">
                                <span class="text-base font-bold text-gray-800">🎒 Sekken på knaggen</span>
                                <span class="text-xl">⭐⭐⭐</span>
                            </div>
                            <div class="flex items-center justify-between bg-pink-50 p-3 rounded-lg">
                                <span class="text-base font-bold text-gray-800">🍽️ Dekke på/av</span>
                                <span class="text-xl">⭐⭐</span>
                            </div>
                        </div>
                        <div class="mt-4 p-4 bg-yellow-100 rounded-lg border-2 border-yellow-400 text-center">
                            <p class="text-base font-black text-yellow-900">Når alle krav er møtt → 💰 UKEPENGER! 💰</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const days = ['Mandag', 'Tirsdag', 'Onsdag', 'Torsdag', 'Fredag', 'Lørdag'];
        const chores = [
            { name: 'Rydde leker i stua', emoji: '🧸' },
            { name: 'Ta ut matboksen fra sekken din', emoji: '🍱' },
            { name: 'Heng opp sekken din på knaggen', emoji: '🎒' },
            { name: 'Hjelpe med å dekke på/av bordet til middag', emoji: '🍽️' }
        ];
        
        const dayEmojis = {
            'Mandag': '🌙',
            'Tirsdag': '🔥',
            'Onsdag': '🌊',
            'Torsdag': '⚡',
            'Fredag': '💛',
            'Lørdag': '🎉'
        };

        let checkedItems = {};

        function toggleCheck(day, chore) {
            const key = `${day}-${chore}`;
            checkedItems[key] = !checkedItems[key];
            renderTable();
            checkCriteria();
        }

        function checkCriteria() {
            const criteria = {
                'Rydde leker i stua': 1,
                'Ta ut matboksen fra sekken din': 3,
                'Heng opp sekken din på knaggen': 3,
                'Hjelpe med å dekke på/av bordet til middag': 2
            };

            // Check if all specific criteria are met
            let allCriteriaMet = true;
            for (const [choreName, required] of Object.entries(criteria)) {
                let count = 0;
                days.forEach(day => {
                    const key = `${day}-${choreName}`;
                    if (checkedItems[key]) count++;
                });
                if (count < required) {
                    allCriteriaMet = false;
                    break;
                }
            }

            // Count total stars
            let totalStars = 0;
            days.forEach(day => {
                chores.forEach(chore => {
                    const key = `${day}-${chore.name}`;
                    if (checkedItems[key]) totalStars++;
                });
            });

            // Activate ukepengar if either criteria is met
            const ukepengerKey = 'Lørdag-ukepenger';
            const shouldActivate = allCriteriaMet || totalStars >= 9;
            
            if (shouldActivate && !checkedItems[ukepengerKey]) {
                checkedItems[ukepengerKey] = true;
                renderTable();
            } else if (!shouldActivate && checkedItems[ukepengerKey]) {
                delete checkedItems[ukepengerKey];
                renderTable();
            }
        }

        function resetWeek() {
            checkedItems = {};
            renderTable();
        }

        function renderTable() {
            const table = document.getElementById('choreTable');
            let html = '';

            // Header row
            html += '<thead><tr class="bg-gradient-to-r from-purple-400 via-pink-400 to-orange-400">';
            html += '<th class="px-6 py-6 text-left text-white font-black text-lg">OPPGAVER</th>';
            
            days.forEach(day => {
                const isLoerdag = day === 'Lørdag';
                const bgClass = isLoerdag ? 'bg-yellow-400 text-yellow-900' : '';
                html += `<th class="px-4 py-6 text-center text-white font-black whitespace-nowrap text-base ${bgClass}">
                    <div class="text-3xl mb-2">${dayEmojis[day]}</div>
                    <div>${day}</div>
                    ${isLoerdag ? '<div class="text-xl mt-2">💰 UKEPENGER!</div>' : ''}
                </th>`;
            });
            
            html += '</tr></thead>';

            // Body
            html += '<tbody>';
            chores.forEach((chore, choreIdx) => {
                const bgClass = choreIdx % 2 === 0 ? 'bg-purple-50' : 'bg-pink-50';
                html += `<tr class="${bgClass}">`;
                html += `<td class="px-6 py-6 font-bold text-gray-800 border-r-4 border-purple-300">
                    <div class="text-3xl mb-2">${chore.emoji}</div>
                    <div class="text-lg">${chore.name}</div>
                </td>`;

                days.forEach((day, dayIdx) => {
                    if (day === 'Lørdag') {
                        if (choreIdx === 0) {
                            const isChecked = checkedItems['Lørdag-ukepenger'] || false;
                            const checkClass = isChecked 
                                ? 'bg-gradient-to-br from-yellow-300 to-orange-400 text-white scale-110 animate-bounce' 
                                : 'bg-gradient-to-br from-gray-200 to-gray-300 text-gray-500';
                            const symbol = isChecked ? '💰' : '☐';
                            html += `<td rowspan="4" class="px-4 py-6 text-center border-r-4 border-purple-200">
                                <button onclick="toggleCheck('Lørdag', 'ukepenger')" 
                                    class="mx-auto flex items-center justify-center w-14 h-14 rounded-2xl transition-all transform hover:scale-125 font-bold text-xl shadow-lg ${checkClass}">
                                    ${symbol}
                                </button>
                            </td>`;
                        }
                    } else {
                        const key = `${day}-${chore.name}`;
                        const isChecked = checkedItems[key] || false;
                        const checkClass = isChecked 
                            ? 'bg-gradient-to-br from-green-400 to-lime-500 text-white scale-110 animate-bounce' 
                            : 'bg-gradient-to-br from-gray-200 to-gray-300 text-gray-500';
                        const symbol = isChecked ? '⭐' : '☐';
                        html += `<td class="px-4 py-6 text-center border-r-4 border-purple-200">
                            <button onclick="toggleCheck('${day}', '${chore.name}')" 
                                class="mx-auto flex items-center justify-center w-14 h-14 rounded-2xl transition-all transform hover:scale-125 font-bold text-xl shadow-lg ${checkClass}">
                                ${symbol}
                            </button>
                        </td>`;
                    }
                });
                
                html += '</tr>';
            });
            html += '</tbody>';

            table.innerHTML = html;
        }

        // Initial render
        renderTable();
    </script>
</body>
</html>
<!--
**stinehan82-hub/stinehan82-hub** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
