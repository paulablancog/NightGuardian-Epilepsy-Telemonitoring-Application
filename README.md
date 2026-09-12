# 🌙 Night Guardian: Epilepsy Telemonitoring Platform

**Night Guardian** is a full-stack telemedicine platform designed to remotely monitor epilepsy patients by recording and analyzing **ECG** (electrocardiogram) and **ACC** (accelerometer) data using a **BITalino biomedical board**.  
It enables patients to safely perform home recordings while doctors receive structured medical data, symptoms, and signal reports through a secure communication system.

This platform was developed for the **Telemedicine course (CEU San Pablo University)** and focuses on biomedical acquisition, distributed systems, and secure medical communication.

## ⭐ Key Features

### Real-Time Biomedical Monitoring
- Acquisition of **ECG** and **ACC** signals via BITalino.
- Real-time visualization and preprocessing (centering, normalization, fixed Y-axis).
- Supports long recording sessions (minutes to hours).

### Seizure Symptom Tracking
- Patient-friendly interface to report symptoms.
- Calendar heatmap with color-coded symptom categories.

### TCP/IP Network Communication
- Custom-designed JSON-based communication protocol.
- Multithreaded central server supporting multiple client connections.
- Bi-directional request/response design using message queues.

### Secure Authentication & Data Management
- Public-key encryption for secure authentication.
- Role-based access (Patient / Doctor / Admin).
- Local storage using **SQLite + JDBC**.
- Atomic multi-database operations for admin changes.

### Cross-platform Desktop Applications (Java Swing)
- **Patient App:** Acquire signals, report symptoms, view doctor feedback.
- **Doctor App:** Review signals, comment medical reports, track symptoms.
- **Admin App:** Manage all users, patients, and doctors.

## Architecture Overview

The platform is composed of **three independent applications**, each with a dedicated GitHub repository:

###  1. Patient Application  
📌 **EpilepsyPatient** → https://github.com/MamenCortes/EpilepsyPatient  
- Connects to BITalino and streams biomedical data.
- Sends recordings and metadata to server.
- Reports symptoms and retrieves doctor information.

###  2. Doctor Application  
📌 **EpilepsyDoctor** → https://github.com/MamenCortes/EpilepsyDoctor  
- Retrieves patient data and ECG recordings.
- Provides signal visualization using JFreeChart.
- Allows adding medical comments to signals.

###  3. Admin Application  
📌 **EpilepsyAdmin** → https://github.com/MariaMM04/EpilepsyAdmin  
- Registers users and assigns roles.
- Links patients to doctors.
- Manages SQLite databases and CRUD operations.

---

## 🔌 Communication Protocol (JSON over TCP)

All communication is performed using a custom JSON messaging protocol over TCP sockets.

### Example: Login Request
```json
{
  "type": "LOGIN_REQUEST",
  "data": {
    "email": "patient@mail.com",
    "password": "1234",
    "access_permits": "Patient"
  }
}
```

### Example: Doctor Response
```json
{
  "type": "REQUEST_DOCTOR_BY_EMAIL_RESPONSE",
  "status": "SUCCESS",
  "doctor": {
    "id": 3,
    "name": "Dr. Lopez",
    "surname": "MgGill"
    "Department": "Oncology"
    "specialty": "Neurology"
  }
}
```
Each connected client is handled by a dedicated `ClientHandler` thread, using a thread-safe `CopyOnWriteArrayList` and a graceful shutdown protocol using `STOP_CLIENT`.

## 📦 Technologies Used

### Frontend / Desktop
- **Java Swing**
- **MigLayout**
- GIF animations for recording status

### Device Integration
- **BITalino (r)evolution** biomedical board  
- Custom preprocessing for ECG signals (centering, normalization, fixed Y-axis)

### Backend
- **TCP/IP sockets**
- **Multithreading**
- **SQLite + JDBC**
- **JSON (Gson)** messaging protocol
- **Public Key Encryption** for secure authentication

### Testing
- **JUnit 5**
- **Mockito**
- Unit tests for:
  - Server lifecycle  
  - ClientHandler logic  
  - Client communication protocol  
  - Socket shutdown behavior  

## 🚀 How to Run

### Patient & Doctor Applications
Both include a `main` entry point.

Compile:

```bash
mvn clean install
````

Run:

```bash
java -jar target/EpilepsyPatient.jar
```

Inside the app, set the **server IP and port** to connect.

## 📂 Project Structure (Simplified Overview)
```
EpilepsyPatient/
   ├── network/        # TCP Client + Listener Thread
   ├── ui/             # Swing UI (recording screen, symptom calendar, menu, etc.)
   ├── bitalino/       # ECG processing & visualization
   └── pojos/          # Patient, Doctor, Signal, Report, User, Role classes

EpilepsyDoctor/
   ├── network/
   ├── ui/
   ├── pojos/

EpilepsyAdmin/
   ├── org.example
     ├── Databases
     ├── entities_medicaldb
     ├── entities_securitydb
     ├── JDBC
     └── encryptation
   ├── ui
   ├── network
   ├── exceptions
   └── encryptation
```

## 👥 Authors

This project was developed as part of the **Telemedicine course at CEU San Pablo University** by:

* [@MamenCortes](https://github.com/MamenCortes)
* [@MariaMM04](https://github.com/MariaMM04)
* [@MartaSanchezDelHoyo](https://github.com/MartaSanchezDelHoyo)
* [@paulablancog](https://github.com/paulablancog)
* [@Claaublanco4](https://github.com/Claaublanco4)

## 🧪 Future Improvements

* AI-based seizure detection
* Cloud-based server deployment
* Mobile companion app
* Automatic alerting system
* Smart ECG segmentation

