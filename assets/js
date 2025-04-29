const seatMapContainer = document.getElementById('seat-map');
const selectedSeatsDisplay = document.getElementById('selected-seats').querySelector('span');
const bookSeatsButton = document.getElementById('book-seats');
const zoomInButton = document.getElementById('zoom-in');
const zoomOutButton = document.getElementById('zoom-out');
const resetZoomButton = document.getElementById('reset-zoom');

const numRows = 8;
const numCols = 10;
const seatPrice = 15; // Price per seat

let selectedSeats = [];
let seatMatrix = []; // 2D array to store seat objects
let zoomLevel = 1;
let baseSeatWidth = 28;  // Base seat width for zoom calculations
let baseSeatHeight = 28; // Base seat height
let enlargedSeatWidth = 40; // Example enlarged width
let enlargedSeatHeight = 40; // Example enlarged height

function createSeatMap() {
    seatMapContainer.innerHTML = ''; // Clear any existing seats
    seatMatrix = []; // Reset the seat matrix

    for (let i = 0; i < numRows; i++) {
        const row = document.createElement('div');
        row.className = 'seat-row';
        const rowLabel = document.createElement('div');
        rowLabel.className = 'row-label';
        rowLabel.textContent = String.fromCharCode(65 + i); // A, B, C, ...
        row.appendChild(rowLabel);

        seatMatrix[i] = []; // Initialize row in the matrix

        for (let j = 0; j < numCols; j++) {
            const seat = document.createElement('div');
            seat.className = 'seat available';
            seat.textContent = j + 1; // Seat number
            seat.dataset.row = i;
            seat.dataset.col = j;
            seat.style.width = `${baseSeatWidth}px`;
            seat.style.height = `${baseSeatHeight}px`;
            row.appendChild(seat);
            seatMatrix[i][j] = seat; // Store seat object in matrix
        }
        seatMapContainer.appendChild(row);
    }

    // Randomly occupy some seats for demonstration
    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j < numCols; j++) {
            if (Math.random() < 0.2) { // 20% chance of being occupied
                const seat = seatMatrix[i][j];
                seat.classList.remove('available');
                seat.classList.add('occupied');
            }
        }
    }
}


function toggleSeatSelection(event) {
    const seat = event.target;
    if (seat.classList.contains('available')) {
        seat.classList.toggle('selected');
        if (seat.classList.contains('selected')) {
            selectedSeats.push({
                row: seat.dataset.row,
                col: seat.dataset.col,
                seatNumber: parseInt(seat.textContent), // Store seat number
                rowLabel: String.fromCharCode(65 + parseInt(seat.dataset.row)), // Store row label
            });
        } else {
            selectedSeats = selectedSeats.filter(s => !(s.row === seat.dataset.row && s.col === seat.dataset.col));
        }
        updateSelectedSeatsDisplay();
    }
}

function updateSelectedSeatsDisplay() {
    if (selectedSeats.length === 0) {
        selectedSeatsDisplay.textContent = 'None';
    } else {
        const seatLabels = selectedSeats.map(seat => `${seat.rowLabel}${seat.seatNumber}`);
        selectedSeatsDisplay.textContent = seatLabels.join(', ');
    }
}

function bookSeats() {
    if (selectedSeats.length === 0) {
        alert('Please select seats to book.');
        return;
    }

    let totalCost = selectedSeats.length * seatPrice;
    const seatList = selectedSeats.map(seat => `${String.fromCharCode(65 + parseInt(seat.row))}${parseInt(seat.col) + 1}`).join(', ');
    const message = `You have booked seats: ${seatList}. Total cost: $${totalCost}.  Enjoy the show!`;
    alert(message);

    selectedSeats.forEach(seat => {
        const seatElement = seatMatrix[seat.row][seat.col]; // Access seat from matrix
        seatElement.classList.remove('available', 'selected');
        seatElement.classList.add('occupied');
    });
    selectedSeats = [];
    updateSelectedSeatsDisplay();
     createSeatMap(); // re-render the seat map to show booked seats as occupied.
}

function zoomIn() {
    zoomLevel += 0.1; // Increment zoom level
    baseSeatWidth = Math.min(enlargedSeatWidth, baseSeatWidth * (1 + 0.1)); // Increase seat width
    baseSeatHeight = Math.min(enlargedSeatHeight, baseSeatHeight * (1 + 0.1)); // Increase seat height

    updateSeatSizes();
}

function zoomOut() {
    zoomLevel = Math.max(1, zoomLevel - 0.1); // Decrement zoom level, but not below 1
    baseSeatWidth = Math.max(28, baseSeatWidth / (1 + 0.1)); // Decrease seat width
    baseSeatHeight = Math.max(28, baseSeatHeight / (1 + 0.1));  // Decrease seat height
    updateSeatSizes();
}

function resetZoom() {
    zoomLevel = 1;
    baseSeatWidth = 28;
    baseSeatHeight = 28;
    updateSeatSizes();
}

function updateSeatSizes() {
    for (let i = 0; i < numRows; i++) {
        for (let j = 0; j < numCols; j++) {
            const seat = seatMatrix[i][j];
            seat.style.width = `${baseSeatWidth}px`;
            seat.style.height = `${baseSeatHeight}px`;
        }
    }
}

// Event Listeners
bookSeatsButton.addEventListener('click', bookSeats);
zoomInButton.addEventListener('click', zoomIn);
zoomOutButton.addEventListener('click', zoomOut);
resetZoomButton.addEventListener('click', resetZoom);

// Initial setup
createSeatMap();
seatMapContainer.addEventListener('click', (event) => {
    const target = event.target;
    if (target.classList.contains('seat')) {
        toggleSeatSelection(event);
    }
});
