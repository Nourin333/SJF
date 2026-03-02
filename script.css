 let processes = [];
    let ganttData = [];

    function addProcess() {
        const pid = document.getElementById('pid').value;
        const at = parseInt(document.getElementById('arrival').value);
        const bt = parseInt(document.getElementById('burst').value);

        if (!pid || isNaN(at) || isNaN(bt) || at < 0 || bt <= 0) {
            alert("Please enter valid positive values.");
            return;
        }

        processes.push({ pid, at, bt, ct: 0, tt: 0, wt: 0, completed: false });
        updateTable();
        
        // Clear inputs
        document.getElementById('pid').value = '';
        document.getElementById('arrival').value = '';
        document.getElementById('burst').value = '';
    }

    function updateTable() {
        const tbody = document.getElementById('tableBody');
        tbody.innerHTML = '';
        processes.forEach(p => {
            tbody.innerHTML += `
                <tr>
                    <td>${p.pid}</td>
                    <td>${p.at}</td>
                    <td>${p.bt}</td>
                    <td>${p.ct || '-'}</td>
                    <td>${p.tt || '-'}</td>
                    <td>${p.wt || '-'}</td>
                </tr>
            `;
        });
    }

    function calculateSJF() {
        if (processes.length === 0) return;

        let currentTime = 0;
        let completedCount = 0;
        let n = processes.length;
        ganttData = [];

        // Reset completion status
        processes.forEach(p => p.completed = false);

        while (completedCount < n) {
            // Find processes that have arrived and are not completed
            let availableProcesses = processes.filter(p => p.at <= currentTime && !p.completed);

            if (availableProcesses.length > 0) {
                // SJF Logic: Pick process with smallest Burst Time
                availableProcesses.sort((a, b) => a.bt - b.bt);
                let currentProcess = availableProcesses[0];

                // Gantt entry
                ganttData.push({ pid: currentProcess.pid, start: currentTime, end: currentTime + currentProcess.bt });

                currentTime += currentProcess.bt;
                currentProcess.ct = currentTime;
                currentProcess.tt = currentProcess.ct - currentProcess.at;
                currentProcess.wt = currentProcess.tt - currentProcess.bt;
                currentProcess.completed = true;
                completedCount++;
            } else {
                // CPU Idle Time
                let nextArrival = Math.min(...processes.filter(p => !p.completed).map(p => p.at));
                ganttData.push({ pid: 'Idle', start: currentTime, end: nextArrival });
                currentTime = nextArrival;
            }
        }
        updateTable();
    }

    function toggleGantt() {
        const container = document.getElementById('gantt-container');
        const chart = document.getElementById('gantt-chart');
        
        if (ganttData.length === 0) {
            alert("Please calculate scheduling first!");
            return;
        }

        container.style.display = 'block';
        chart.innerHTML = '';

        ganttData.forEach((item, index) => {
            const box = document.createElement('div');
            box.className = `gantt-box ${item.pid === 'Idle' ? 'idle' : ''}`;
            box.style.width = `${(item.end - item.start) * 30}px`; // Scaling
            box.innerHTML = `<span>${item.pid}</span><span class="gantt-time">${item.start}</span>`;
            
            // Add final time for last box
            if (index === ganttData.length - 1) {
                const endCap = document.createElement('span');
                endCap.className = 'gantt-time';
                endCap.style.left = '100%';
                endCap.innerText = item.end;
                box.appendChild(endCap);
            }
            
            chart.appendChild(box);
        });
    }

    function calculateAverage() {
        if (processes.length === 0 || processes[0].ct === 0) {
            alert("Calculate scheduling first!");
            return;
        }

        let totalTT = processes.reduce((acc, p) => acc + p.tt, 0);
        let totalWT = processes.reduce((acc, p) => acc + p.wt, 0);
        let avgTT = (totalTT / processes.length).toFixed(2);
        let avgWT = (totalWT / processes.length).toFixed(2);

        const resDiv = document.getElementById('avg-results');
        resDiv.style.display = 'block';
        resDiv.innerHTML = `
            Average Turnaround Time: ${avgTT}ms <br>
            Average Waiting Time: ${avgWT}ms
        `;
    }
