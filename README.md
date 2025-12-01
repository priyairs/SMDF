# SMDF
---------------------------------------------------------

# 🔸 **A. Sequence + Component Diagram — Hospital Management System**

### **Component Diagram (PlantUML)**

```
@startuml
package "Hospital System" {
  [Web UI] --> [API Gateway]
  [API Gateway] --> [Patient Service]
  [API Gateway] --> [Appointment Service]
  [API Gateway] --> [Billing Service]
  [API Gateway] --> [Doctor Service]
  [Patient Service] --> [Patient DB]
  [Appointment Service] --> [Appointment DB]
  [Billing Service] --> [Billing DB]
  [Doctor Service] --> [Doctor DB]
  [Notification Service] --> [Email/SMS Provider]
}
@enduml
```

> Save as `diagrams/hospital_component.puml` and render with PlantUML.

---

### **Sequence Diagram — Appointment Booking (PlantUML)**

```
@startuml
actor Patient
participant "Web UI" as UI
participant "API Gateway" as Gateway
participant "Appointment Service" as ApptSvc
participant "Doctor Service" as DocSvc
participant "Patient DB" as PDB
participant "Appointment DB" as ADB
participant "Notification Service" as Notif

Patient -> UI: Request appointment (date, doctorId, patientId)
UI -> Gateway: POST /appointments {patientId, doctorId, time}
Gateway -> ApptSvc: createAppointment(request)
ApptSvc -> PDB: validatePatient(patientId)
PDB --> ApptSvc: valid
ApptSvc -> DocSvc: checkAvailability(doctorId, time)
DocSvc --> ApptSvc: available
ApptSvc -> ADB: insert(appointmentRecord)
ADB --> ApptSvc: confirmedRecord
ApptSvc -> Notif: sendConfirmation(patientId, appointmentInfo)
Notif --> Patient: Email/SMS confirmation
ApptSvc --> Gateway: 201 Created {appointmentId}
Gateway --> UI: 201 Created {appointmentId}
UI --> Patient: Show confirmation
@enduml
```

> Save as `diagrams/hospital_sequence_appointment.puml`.

---

# 🔸 **B. Functional & Non-Functional requirements — Online Course Reservation System**

### **Functional Requirements**

```
- User registration & authentication (email/phone verification)
- Browse courses (search, filter by category/level/date)
- View course details (syllabus, instructor, schedule, capacity)
- Book/reserve course slot (seat reservation)
- Waitlisting when a slot is full
- Payment processing for paid courses (card/UPI/netbanking)
- Cancel registration and refund rules
- Admin panel: create/update courses, manage capacity, view enrollments
- Calendar integration & reminders (email/ICS invites)
- View booking history and certificates (if applicable)
- Notifications: booking, cancellation, reminder, waitlist promotion
- Reporting: occupancy, revenue, course popularity
```

### **Non-Functional Requirements**

```
- Concurrency/Consistency: atomic seat allocation to prevent double-booking
- Scalability: support spikes during course launches (horizontal scaling)
- Performance: booking response < 2s under normal load
- Availability: 99.9% uptime for core reservation service
- Security: TLS, input validation, secure payment handling (PCI best practices)
- Maintainability: modular service design, clear APIs for course/booking/payment
- Data Retention & Compliance: store user/payment data per legal requirements
- Monitoring & Logging: central logs, error alerts, metrics (latency, success-rate)
- Backup & Recovery: regular DB backups and tested restore process
```

---

# 🔸 **C. Create & configure Jenkins Freestyle Project — Build Java Web App (Maven)**

### **Assumptions**

```
- Jenkins already installed and reachable at http://localhost:8080
- Git repo exists and contains a Maven Java web application (pom.xml, src/)
- Jenkins has permission to access Git (or use credentials)
```

### **Step 1 — Open Jenkins**

```
http://localhost:8080
```

### **Step 2 — Create Freestyle Job**

```
* Click "New Item"
* Enter name: `courses_maven_build`
* Select "Freestyle project"
* Click "OK"
```

### **Step 3 — Add Job Description**

```
Build & package Courses Web Application (Maven)
```

### **Step 4 — Source Code Management**

```
* Choose: Git
* Repository URL: https://github.com/<USERNAME>/<REPO>.git
* Credentials: (add if private)
* Branches to build:
  */main
```

### **Step 5 — Build Environment (optional)**

```
* Check "Delete workspace before build starts"  (recommended for clean builds)
```

### **Step 6 — Add Build Steps**

```
* Click "Add build step" -> "Invoke top-level Maven targets"
* Goals:
  clean package
* (If using mvnw, use "Execute shell" and run ./mvnw clean package)
```

### **Step 7 — Post-build Actions**

```
* Add -> "Archive the artifacts"
  Files to archive: **/target/*.war
* (Optional) Add -> "Publish JUnit test result report"
  Test report XMLs: **/target/surefire-reports/*.xml
```

### **Step 8 — Save & Run**

```
* Click "Apply" -> "Save"
* Click "Build Now"
* Monitor Console Output to validate build
```
---
# ----------------------------------------------------------

# 🔸 **Nagios Installation & Monitoring**

### **Step 1 — Open Docker Desktop**

### **Step 2 — Open PowerShell**

```
docker pull jasonrivers/nagios:latest
```

### **Step 3 — Run Nagios**

```
docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest
```

### **Step 4 — Open Browser**

```
http://localhost:8888
```

### **Step 5 — Login**

* Username:

```
Adminnagios
```

* Password:

```
nagios
```

### **Step 6 — Check Monitoring**

Left sidebar → **Hosts → localhost**

---

# 🔸 **Cleanup**

```
docker ps
docker stop nagiosdemo
docker rm nagiosdemo
docker images
docker rmi jasonrivers/nagios:latest
```

---
Write the Docker file and create the image and  access the image in localhost

mkdir mywebapp
cd mywebapp

echo "<h1>Hello from Docker!</h1>" > index.html
nano Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
docker build -t mywebimage .
docker run -d -p 8080:80 mywebimage
docker ps
