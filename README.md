<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Hospital Appointment Booking</title>

<style>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    background: #f4f7f9;
}

header {
    background: #0077b6;
    color: white;
    padding: 15px;
    text-align: center;
}

nav a {
    margin: 10px;
    color: white;
    text-decoration: none;
    font-weight: bold;
}

.hero {
    background: #90e0ef;
    padding: 40px;
    text-align: center;
}

section {
    padding: 30px;
}

.doctor-container {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap;
}

.doctor-card {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px gray;
    width: 200px;
    text-align: center;
}

form {
    display: flex;
    flex-direction: column;
    max-width: 400px;
    margin: auto;
    gap: 10px;
}

input, select, button {
    padding: 10px;
    font-size: 16px;
}

button {
    background: #0077b6;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background: #023e8a;
}

footer {
    text-align: center;
    padding: 10px;
    background: #0077b6;
    color: white;
    margin-top: 30px;
}

#message {
    text-align: center;
    font-weight: bold;
    margin-top: 15px;
}

#appointments {
    max-width: 500px;
    margin: 20px auto;
}

.appointment-item {
    background: white;
    padding: 10px;
    margin: 5px 0;
    border-radius: 5px;
    box-shadow: 0 0 5px gray;
}
</style>

</head>
<body>

<header>
    <h1>City Hospital</h1>
    <nav>
        <a href="#">Home</a>
        <a href="#doctors">Doctors</a>
        <a href="#appointment">Book Appointment</a>
    </nav>
</header>

<section class="hero">
    <h2>Your Health, Our Priority</h2>
    <p>Book appointments with top doctors easily</p>
</section>

<section id="doctors">
    <h2 style="text-align:center;">Our Doctors</h2>
    <div class="doctor-container">
        <div class="doctor-card">
            <h3>Dr. Ram</h3>
            <p>Cardiologist</p>
        </div>
        <div class="doctor-card">
            <h3>Dr. Priya</h3>
            <p>Dermatologist</p>
        </div>
        <div class="doctor-card">
            <h3>Dr. Kumar</h3>
            <p>Orthopedic</p>
        </div>
    </div>
</section>

<section id="appointment">
    <h2 style="text-align:center;">Book Appointment</h2>

    <form id="appointmentForm">
        <input type="text" placeholder="Patient Name" required>
        <input type="email" placeholder="Email" required>
        <input type="tel" placeholder="Phone Number" required>
        
        <select required>
            <option value="">Select Doctor</option>
            <option>Dr. Ram</option>
            <option>Dr. Priya</option>
            <option>Dr. Kumar</option>
        </select>

        <input type="date" required>
        <input type="time" required>

        <button type="submit">Book Appointment</button>
    </form>

    <p id="message"></p>

    <div id="appointments"></div>
    <button onclick="clearAppointments()">Clear All Appointments</button>
</section>

<footer>
    <p>© 2026 City Hospital</p>
</footer>

<script>
const form = document.getElementById("appointmentForm");
const message = document.getElementById("message");
const list = document.getElementById("appointments");

// Load saved appointments
window.onload = loadAppointments;

form.addEventListener("submit", function(e) {
    e.preventDefault();

    const name = form.querySelector("input[type='text']").value.trim();
    const email = form.querySelector("input[type='email']").value.trim();
    const phone = form.querySelector("input[type='tel']").value.trim();
    const doctor = form.querySelector("select").value;
    const date = form.querySelector("input[type='date']").value;
    const time = form.querySelector("input[type='time']").value;

    if (!name || !email || !phone || !doctor || !date || !time) {
        showMessage("⚠️ Fill all fields!", "red");
        return;
    }

    if (!email.includes("@")) {
        showMessage("❌ Invalid email!", "red");
        return;
    }

    if (phone.length < 10) {
        showMessage("❌ Invalid phone number!", "red");
        return;
    }

    const hour = parseInt(time.split(":")[0]);
    if (hour < 9 || hour > 18) {
        showMessage("⏰ Select time between 9AM - 6PM", "red");
        return;
    }

    const appointment = `${name} - ${doctor} on ${date} at ${time}`;

    saveAppointment(appointment);
    displayAppointment(appointment);

    showMessage("✅ Appointment Booked!", "green");
    form.reset();
});

function showMessage(text, color) {
    message.innerHTML = text;
    message.style.color = color;
}

function saveAppointment(data) {
    let appointments = JSON.parse(localStorage.getItem("appointments")) || [];
    appointments.push(data);
    localStorage.setItem("appointments", JSON.stringify(appointments));
}

function loadAppointments() {
    let appointments = JSON.parse(localStorage.getItem("appointments")) || [];
    appointments.forEach(displayAppointment);
}

function displayAppointment(data) {
    const div = document.createElement("div");
    div.className = "appointment-item";
    div.textContent = data;
    list.appendChild(div);
}

function clearAppointments() {
    localStorage.removeItem("appointments");
    list.innerHTML = "";
}
</script>

</body>
</html>
